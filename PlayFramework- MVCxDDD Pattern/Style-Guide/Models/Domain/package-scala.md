---
title: package.scala
---


[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

# Domain's `package.scala`

This is a special Scala file that allows you to define package-wide members, such as shared utilities, type aliases, or implicit.

In here we will be sharing a shared data type for common names in the domain.

## Example

```scala
case class Workspace(id:UUID,name:String)
case class Task(id:UUID,name:String)
```

`id` and `name` are repeated names with the same data type `UUID` and `String` in both `Workspace` and `Task`.

## The use of `package.scala` in the domain

In the `repo`, we will use **overloaded methods** and if we have same type signature. it will put a compilation error.

```scala
  // TaskRepo's Table object
  def get(id: UUID): Future[Option[Task]] =
    db.run(this.filter(_.id === id).result.headOption)
  // Compilation Error
  def get(idWorkspace: UUID): Future[Option[Task]] =
    db.run(this.filter(_.idWorkspace === idWorkspace).result.headOption)
```

This code will give compilation error since it has the same type signature in the parameter. and we will be forced to create custom actions.

```scala
  // TaskRepo's Table object
  def get(id: UUID): Future[Option[Task]] =
    db.run(this.filter(_.id === id).result.headOption)

  // No more error but we created a custom action
  def getByIdWorkspace(idWorkspace: UUID): Future[Option[Task]] =
    db.run(this.filter(_.idWorkspace === idWorkspace).result.headOption)
```

To solve that issue we will use and still implement [CRUDdy](Style-Guide/Controllers/Cruddy-By-Design) actions in the `repo` we will use **`WrappedType[T]`** trait. which will only wrap one type and we can still enforce type-safety.

### Inside `package.scala`

```scala
import java.util.UUID
import scala.deriving.Mirror.ProductOf
import scala.reflect.ClassTag
import play.api.libs.json._
import slick.jdbc.PostgresProfile.api._
import slick.jdbc.JdbcType

package object domain:

  /* -- Task -- */
  opaque type IdTask = UUID
  object IdTask:
    def apply(x: UUID): IdTask   = x

    extension (x:IdTask)
      def value:String = x

    given Format[IdTask] =
      formatter.json.format(_.validate[String], x => JsString(x.toString()))
    given Formatter[IdTask] =
      formatter.form.format(uuidFormat)(_.value, IdTask.apply)
  end IdTask

  opaque type NameTask = String
  object NameTask:
    def apply(x: String): NameTask   = x

    extension (x:NameTask)
      def value:String = x

    given Format[NameTask] =
      formatter.json.format(_.validate[String], JsString(_))

    given Formatter[NameTask] =
      formatter.form.format(stringFormat)(_.value, NameTask.apply)

  end NameTask


  // /* -- Workspace -- */
  opaque type IdWorkspace = UUID
  object IdWorkspace:
    def apply(x: UUID): IdWorkspace   = x

    extension (x:IdWorkspace)
      def value:String = x

    given Format[IdWorkspace] =
      formatter.json.format(_.validate[String], x => JsString(x.toString()))

    given Formatter[IdWorkspace] =
      formatter.form.format(uuidFormat)(_.value, IdWorkspace.apply)

  end IdWorkspace

  opaque type NameWorkspace = String
  object NameWorkspace:
    def apply(x: String): NameWorkspace   = x

    extension (x:NameWorkspace)
      def value:String = x

    given Format[NameWorkspace] =
      formatter.json.format(_.validate[String], JsString(_))

    given Formatter[NameWorkspace] =
      formatter.form.format(stringFormat)(_.value, NameWorkspace.apply)

  end NameWorkspace
  /* -- End Of Workspace -- */
```

**Note:** if a variable name is common in domain case classes. implement `opaque types`, if you're not familiar with it. read the [documentation](https://docs.scala-lang.org/scala3/book/types-opaque-types.html) of opaque types.

### Example

```scala
case class Workspace(id: IdWorkspace,name: NameWorkspace)
case class Task(id:IdTask,name:NameTask)
```

As you can see here, both **`id`** and **`name`** is present in classes **`Workspace`** and **`Task`**. which means we have to put `opaque types` for them.

### How to structure our `opaque types`

1. Inside the `package object domain` create an opaque type like this

```scala
opaque type IdTask
```

2. Make the object which consists of the implicits and the extension which will provide the `value`.

```scala
object IdTask:
  def apply(x: UUID): IdTask   = x

  extension (x:IdTask)
    def value:String = x

  given Format[IdTask] =
    formatter.json.format(_.validate[String], x => JsString(x.toString()))
  given Formatter[IdTask] =
    formatter.form.format(uuidFormat)(_.value, IdTask.apply)

end IdTask
```

3. Make `JDBC givens` in the Repo's `package.scala`, do not put it in the object. Since it will only be used by the Repositories and sometimes will be shared across multiple Repositories.

The reason for this is so if we use foreign keys in another Tables, we won't have to put another `JdbcType[T]` implicits for them again, it will just be redundant

#### ✅ Do this

```scala
package object repo:
  import domain.*

  given JdbcType[IdTask] =
    MappedColumnType.base[IdTask, UUID](_.value, IdTask.apply)

  given JdbcType[NameTask] =
    MappedColumnType.base[NameTask, String](_.value, NameTask.apply)

end repo

```

#### ❌ Do not do this

```scala
@Singleton
class TaskRepo @Inject() (
    protected val dbConfigProvider: DatabaseConfigProvider
)(using ExecutionContext)
    extends HasDatabaseConfigProvider[JdbcProfile]:
  import dbConfig.profile.api._

  given JdbcType[IdTask] =
    MappedColumnType.base[IdTask, UUID](_.value, IdTask.apply)

  given JdbcType[NameTask] =
    MappedColumnType.base[NameTask, String](_.value, NameTask.apply)

  protected class Tasks(tag: Tag) extends Table[Task](tag, "TASKS"):
    val id  = column[IdTask]("ID", O.PrimaryKey)
    val name = column[NameTask]("ID", O.PrimaryKey)
    def *   = (id, name).mapTo[Task]
```

Now, we can use `IdTask` like `UUID` while still maintaining type-safe development.
## `private object formatter`
This is a helper `object` to help reduce boiler plate in `implicits` of the `opaque types`
```scala
  private object formatter:
    object json:

      def format[T](
          fromJson: JsValue => JsResult[T],
          toJson: T => JsValue
      ) =
        new Format[T]:
          def reads(json: JsValue): JsResult[T] = fromJson(json)
          def writes(o: T): JsValue             = toJson(o)

    end json

    object form:

      def format[A, B](
          baseFormatter: Formatter[A]
      )(to: A => B, from: B => A): Formatter[B] =
        new:
          def bind(key: String, data: Map[String, String]) =
            baseFormatter.bind(key, data).map(to)
          def unbind(key: String, value: B) =
            baseFormatter.unbind(key, from(value))

    end form

  end formatter
```
---

[Previous: Responses](Style-Guide/Models/Domain/Responses) | [Next: Repo](Style-Guide/Models/Repo)