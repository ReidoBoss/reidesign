[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)


# Services Naming Convention

## Injecting Repositories

### Do this ✅

```scala
@Singleton
class TaskService @Inject() (
    val taskRepo: TaskRepo,
)(using ExecutionContext):
  def create = ???
```

### Do not do this ❌

```scala
@Singleton
class TaskService @Inject() (
    val task: TaskRepo,
)(using ExecutionContext):
  def create = ???
```

## Apply [Cruddy By Design](Style-Guide/Cruddy-By-Design) pattern

Ensure that all services follow [Cruddy By Design](Style-Guide/Cruddy-By-Design) pattern.

Since the majority of the codebase follows this pattern, maintaining consistency in services helps promote consistent structure throughout the project.

## 1:1 Relationship with Controller

To maintain consistency across the codebase, the service layer should mirror the controller structure. For example, if a controller is named `TaskController`, its corresponding service should be named `TaskService`. This **1:1** relationship helps promote consistent structure throughout the project.

### Mirror The Actions of Controller

If this is the `actions` or `functions` of `TaskController`:

```scala
@Singleton
class TaskController @Inject() (
    cc: ControllerComponents,
    taskService: TaskService,
    secureAction: SecureAction
)(using ExecutionContext)
    extends AbstractController(cc)
    with I18nSupport {

  def create = secureAction.async { implicit request =>
    FormHandler.execute(TaskForm.create)(
      taskService.create(request.idUser)
    )
  }

  def all(idWorkspace: UUID) = secureAction.async { implicit request =>
    taskService.all(idWorkspace).fold(makeStatus, makeStatus)
  }

  def update = secureAction.async { implicit request =>
    FormHandler.execute(TaskForm.update)(taskService.update)
  }

  def delete = secureAction.async { implicit request =>
    FormHandler.execute(TaskForm.delete)(taskService.delete)
  }
}

```

Then then `actions` of `TaskService` should be:

```scala
// TaskService
def create = ???
def all = ???
def update = ???
def delete = ???
```

## Private actions

You can put private `functions` in services but make sure the name is **descriptive**.

```scala
// TaskService
private def getValidMembers = ???
```

---
[Previous: Overview](Style-Guide/Models/Services/Overview) | [Next: Routes](Style-Guide/routes)