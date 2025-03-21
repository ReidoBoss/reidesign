[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

# Form domains
- Form domain `case classes` are used by services and forms from controllers.
- The purpose of this `case classes` is for creating actions in the services.

```scala
  // TaskCreate.scala
  case class TaskCreate(
      idParentTask: Option[UUID],
      idWorkspace: UUID,
      name: String
  )

  object TaskCreate:
    // there is unapply here to be used in controller folder's form. Check Forms in controller's wiki
    def unapply(o: TaskCreate) = 
      Some(Tuple.fromProductTyped(o))



  // TaskService.scala
  def create(task: TaskCreate) = ???

```

- Should follow [Cruddy By Design](Style-Guide/Controller/Cruddy-By-Design) Convention but only create, update, retrieve, and delete

   - Most of the time retrieve won't be used because its common to just use one value in retrieving like idUser and we don't need a case class to wrap one value.

![Folder Structure](uploads/1d891930b6e1a8b9ebcc919f972d0de0/image.png)

---
[Previous: Parameter Ordering](Style-Guide/Models/Domain/Parameter-Ordering) | [Next: Responses](Style-Guide/Models/Domain/Responses)