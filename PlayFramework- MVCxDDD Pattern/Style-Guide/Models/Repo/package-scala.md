[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

# Repo package.scala

In here, we will put the implicits of the Custom opaque types. 

```scala
package object repo:
  import domain.*

  given JdbcType[IdTask] =
    MappedColumnType.base[IdTask, UUID](_.value, IdTask.apply)

  given JdbcType[NameTask] =
    MappedColumnType.base[NameTask, String](_.value, NameTask.apply)

end repo

```
---
[Previous: Actions](Style-Guide/Models/Repo/Actions) | [Next: Services](Style-Guide/Models/Services/Overview)