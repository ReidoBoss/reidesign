[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

# Style Pattern

We will follow these patterns throughout the application, so it's essential to read and understand this section first.

## Import Pattern

Follow this [Importing Pattern](Style-Guide/Importing).

## case class Parameter Ordering

Follow this [Ordering Pattern](Style-Guide/Models/Domain/Parameter-Ordering).

## Naming Pattern

Every **property** of a **Persona** should be at the first.
Example :

```scala
// ❌ Do not do this
case class Workspace(id: WorkspaceId,name: WorkspaceName, taskId: TaskId)
                             ↓        ↓      ↓             ↓
// ✅ Do this
case class Workspace(id:IdWorkspace,name:NameWorkspace,idTask:IdTask)
```

## Scope pattern

Since we are using **scala 3**, we will use the new syntaxes of **scala 3**.

### classes, case classes, and etc.

#### ✅ Do This

```scala
object Workspace:
  def test() = ???
```

#### ❌ Do not do this

```scala
object Workspace {
  def test = {
    /* Implementation Here */
  }
}
```

If a file has one or more object or classes, put an **end marker**`<object or class' name>` at the end of the object to promote cleanliness in the code.

```scala
package object domain:

  opaque type IdTask = UUID
  object IdTask:
    /* Implementations */
  end IdTask // end marker

  opaque type NameTask = String
  object NameTask:
    /* Implementations */
  end NameTask // end marker
```

### more scope Example patterns

#### avoid redundant curly braces and parenthesis

**❌ Do not do this**

```scala
(1 == 2) match {
  case true =>
  case false =>
}
// ------------------------------------------------
given Format[NameTask] with {
  def reads(json: JsValue): JsResult[NameTask] = json.validate[String]
  def writes(o: NameTask): JsValue             = JsString(o.toString())
}
```

**✅ Do this**

```scala
1 == 2 match
  case true =>
  case false =>
// ------------------------------------------------
given Format[NameTask] with
  def reads(json: JsValue): JsResult[NameTask] = json.validate[String]
  def writes(o: NameTask): JsValue             = JsString(o.toString())
```

---

[Previous: Home](Home) | [Next: Naming Convention](Style-Guide/Naming-Convention)