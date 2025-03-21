[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)


# Table

This is what the repository instance should look like.

```scala
import javax.inject.Inject
import javax.inject.Singleton
import scala.concurrent.Future
import scala.concurrent.ExecutionContext
import play.api.db.slick._
import slick.jdbc.JdbcProfile

@Singleton
class WorkspaceMemberRepo @Inject() (
    protected val dbConfigProvider: DatabaseConfigProvider
)(using ExecutionContext) extends HasDatabaseConfigProvider[JdbcProfile]:
  import dbConfig.profile.api._
```

## Table

Inside the instance. It should look something like this

```scala

  protected class Tasks(tag: Tag) extends Table[Task](tag, "TASKS"):
    val id          = column[IdTask]("ID", O.PrimaryKey)
    val name        = column[NameTask]("name", O.PrimaryKey)
    val idWorkspace = column[IdWorkspace]("ID", O.PrimaryKey)
    def *           = (id, name, idWorkspace).mapTo[Task]

  // Check Table Action Wiki for for info about Table Object
  object Table extends TableQuery(new Tasks(_)):
    def get = ???
    def find = ???

```

### Notes to remember

1. We don't need to create a Foreign Key instance here. just make it so in the **evolution's** query
2. Make sure the name of the **repo**, and the **table** is Relative to the **domain** that it will map to

```scala
WorkspaceMember // the domain
WorkspaceMembers // the table instance naming should be the plural word of the domain
WorkspaceMemberRepo // the repo instance
```

---

[Previous: Repo](Style-Guide/Models/Repo) | [Next: Actions](Style-Guide/Models/Repo/Actions)
