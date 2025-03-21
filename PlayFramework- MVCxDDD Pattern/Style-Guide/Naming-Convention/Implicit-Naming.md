[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)


# Implicit Naming Convention

This section will talk about how we should use implicits in our objects.

We will always use givens in the objects, without name. unless if needed

```scala
// ✅
case class Task(id:IdTask,name:NameTask)
object Task:
  given Format[Task] = Json.format[Task]
/*
❌
  Don't put name in the given because most of the
  time it won't be used, only do so if needed
*/
case class Task(id:IdTask,name:NameTask)
object Task:
  given jsonConverter: Format[Task] = Json.format[Task]

// ❌ Don't use implicit syntax
case class Task(id:IdTask,name:NameTask)
object Task:
  implicit val jsonConverter: Format[Task] = Json.format[Task]
```
---
[Previous: Naming Convention](Style-Guide/Naming-Convention) | [Next: Package Naming](Style-Guide/Naming-Convention/Package-Naming)