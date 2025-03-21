[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

# Domain Naming Convention

## `case class` and file names must match

```scala
// Task.scala
case class Task(id:IdTask,name:NameTask)
```

## `case classes` must be PascalCased

```scala
// Task.scala
case class WorkspaceMember // ✅
case class workspaceMember // ❌
case class workspacemember // ❌
...
```

## `case classes` should be singular

```scala
  case class WorkspaceMember // ✅
  case class WorkspaceMembers // ❌
```

---
[Previous: Overview](Style-Guide/Models/Domain/Overview) | [Next: Parameter Ordering](Style-Guide/Models/Domain/Parameter-Ordering)