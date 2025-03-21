[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)


# Domain Responses

## Response Naming Convention

- While the codebase **domain** case classes typically follow the one-case-class-per-file rule, **Responses** are an exception.
- **Responses** represent the data sent to the frontend and should be placed in the **domain** folder.
- **Never** place anything in `Response.scala` as this file is intended for the main domain.

### This is the Reponse.scala:

```scala
// Response.scala
import play.api.libs.json.JsValue
import play.api.mvc.Results.Status

trait Response:
  val code: String
  val status: Status
  val data: Option[JsValue]
  // this will convert the case classes extending to it to Result
  def toResult:Result =
    status(
      Json.obj(
        "code" -> code,
        "data" -> data
      )
    )

trait ResponseError extends Response
trait ResponseSuccess extends Response
```

## Response Case Class Naming Convention

- Every response `file` should start with the `Response` keyword, followed by the name of the entity it belongs to.

  ### Example:

  - `ResponseWorkspace.scala`

- Responses should extend either `ResponseError` or `ResponseSuccess`, depending on the type of response.
- Use comments like `/*  Successful Response  */` and `/*  Error Response  */` for easy navigation.

### Example:

```scala
// ResponseWorkspace.scala
/*  Successful Responses  */
case class WorkspaceCreated(workspace: Workspace) extends ResponseSuccess:
  val code   = "WORKSPACE_CREATED"
  val status = Created
  val data   = Some(Json.toJson(workspace))

/*  Error Responses  */
case class WorkspaceUserNotFound() extends ResponseError:
  val code   = "WORKSPACE_USER_NOT_FOUND"
  val status = NotFound
  val data   = None
```

Note: There could be many error responses and successful responses

## Response Code Naming Convention

- **case class names** should follow PascalCase.
- **code values** should be in **UPPER_SNAKE_CASE**, relative to the case class name.

### Example:

```scala
case class WorkspaceUserNotFound() extends ResponseError:
  val code   = "WORKSPACE_USER_NOT_FOUND"
  val status = NotFound
  val data   = None
```

---

[Previous: Forms](Style-Guide/Models/Domain/Forms) | [Next: package.scala](Style-Guide/Models/Domain/package-scala)
