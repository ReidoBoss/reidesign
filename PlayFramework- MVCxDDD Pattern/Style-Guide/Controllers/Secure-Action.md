## [Home](Home) | [Style Guide](Style-Guide)

# Secure Action

## Overview

In controllers, we always use **Secure Action** to check if a user is authenticated. This approach helps us avoid redundancy in the code and ensures consistency across the application.

## Folder Structure

Below is the folder structure for reference:

![Folder Structure](uploads/d8942ba100f7c737f96507d022cd659e/image.png)

## Example Code

Here's an example implementation:

```scala
  /*
    We will use Wrapped Request so that we can access the user's id if ever
    They are authenticated
   */
  case class AuthenticatedRequest[A](
      idUser: UUID,
      request: Request[A]
  ) extends WrappedRequest[A](request)

  class SecureAction @Inject() (
      val parser: BodyParsers.Default
  )(using ExecutionContext) extends ActionBuilder[AuthenticatedRequest, AnyContent]:

    override def invokeBlock[A](
        request: Request[A],
        block: AuthenticatedRequest[A] => Future[Result]
    ) =
      request.session.get("idUser") match
        case None => UnauthorizedResult
        case Some(idUser) =>
          val ID_USER = UUID.fromString(idUser)
          block(AuthenticatedRequest(ID_USER, request))
```

## Example Usage

Here's an example usage in a Controller:

### User logging in

```scala
@Singleton
class UserSessionController @Inject() (
    cc: ControllerComponents,
    userService: UserService
)(using ExecutionContext)
    extends AbstractController(cc)
    with I18nSupport:
  /*
    This is a abstracted code but inside here, The code already got the ID of the user
    stored in the request because it has became a Wrapped Request.
  */
  def create = Action.async { implicit request =>
    FormHandler.execute(UserSessionForm.create)(userService.compare)
  }
```

### Using Secure Action

```scala
@Singleton
class ProjectController @Inject() (
    val cc: ControllerComponents,
    val secureAction: SecureAction, // Make sure this is Injected
    projectService: ProjectService
)(using ExecutionContext):

  def all(idWorkspace: UUID) = secureAction.async {
    implicit request =>
      println(request.idUser) // this will print the logged in User's ID
      projectService
        .filter(idWorkspace)
        .fold(makeStatus, makeStatus)
  }
```

If the user hasn't successfully logged in, they will not have a session.
leading them to have no access to this Project Controller.

```scala
request.session.get("idUser") match
  case None => UnauthorizedResult // Because of this
  case Some(idUser) =>
    val ID_USER = UUID.fromString(idUser)
    block(AuthenticatedRequest(ID_USER, request))
```

---

[Previous:Controllers](Style-Guide/Controllers)
