## [Home](Home) | [Style Guide](Style-Guide)

# FormHandler Object

In the `FormHandler` object, this is the object that will be called in the controllers for handling forms. and we will have two functions here. one public, which is the **function** `execute` and a **private function** `invalid` for handling invalid form validations.

Note: Always make a case class in every Forms, to be put in the `domain` folder in `models`, refer to [Domain's Forms Section](Style-Guide/Models/Domain/Forms).

## Execute Function

`def execute` function will be used to validate the JSON that is sent by the client and then perform an action in the `services`

```scala
  def execute[T, R <: ResponseSuccess](
      form: Form[T]
  )(
      action: T => EitherT[Future, ResponseError, R]
  )(using
      Persona,
      Request[AnyContent],
      MessagesProvider,
      ExecutionContext
  ): Future[Result] = ???

```

This function is curried into **three**.

### First curry

#### `form` parameter

In the `form` parameter, You will put here the `form` that will be used to validate JSON from the frontend

```scala
def create = secureAction.async { implicit request =>
    FormHandler.execute(ProjectForm.create)(???)(???)
}
```

### Second curry

In the `action` parameter, it is a higher-order function which will return what is the `case class` of that Form and then the result should be `EitherT[Future, ResponseError, R]`, for more understanding about `EitherT` data type, read the [documentation](https://typelevel.org/cats/datatypes/eithert.html).

#### We have three ways to use this

- First, Since our model's `services` takes a `form domain` most of the time and always return `EitherT[Future, ResponseError, R <: ResponseSuccess]` we can use it like this

```scala
def create = secureAction.async { implicit request =>
    FormHandler.execute(ProjectForm.create)(projectService.create)(???)
}
```

- Second, avoid this style if possible but only use if needed so.

```scala
def create = secureAction.async { implicit request =>
  FormHandler.execute(ProjectForm.create)(projectForm => projectService.create(projectForm))(???)
}
```

### Third Curry

In the third curry, we don't have to put it in the parameter since it is all about implicits, if you're not familiar with the `using` keyword and you are from scala 2. please refer to scala 3's [using clauses](https://docs.scala-lang.org/scala3/reference/contextual/givens.html).

```scala
(using
    Persona,
    Request[AnyContent],
    MessagesProvider,
    ExecutionContext
)
```

as you can see, we will use 4 implicits here

- **Persona implicit**
  - Persona.scala will be located in model's domain folder

**Note:** Always remember to put individual Persona for every controller, name should be relative to controller's name

```scala
trait Persona {
  def toString: String
}

object Persona {
  given Project: Persona with  {
    override def toString = "PROJECT"
  }
}
```

The use of persona is to make use of the **private function** `invalid`

```scala
  private def invalid[T](
      error: Form[T]
  )(using persona: Persona, mp: MessagesProvider): Result =
    BadRequest(
      Json.obj(
        /*
            this will make the code become "INVALID_PROJECT_FORM"
            if the persona used is Persona.Project
        */
        "code" -> s"INVALID_${persona.toString()}_FORM",
        "data" -> error.errorsAsJson
      )
    )

  def execute[T, R <: ResponseOK](
      form: Form[T]
  )(
      action: T => EitherT[Future, ResponseError, R]
  )(using
      Persona,
      Request[AnyContent],
      MessagesProvider,
      ExecutionContext
  ): Future[Result] =
    form
      .bindFromRequest()
      .fold(
        error => Future(invalid(error)), // invalid function is used here
        _.toResult
      )
```

- **Request[AnyContent]** implicit
  - we need this implicit to to be able to use `bindFromRequest` of the form.

```scala
    form.bindFromRequest()
```

- **MessagesProvider implicit**
  - we need this because it is one of form's implicit
- **ExecutionContext implicit**
  - we need this to return **Future[Result]** values

---

[Previous: Forms](Style-Guide/Controllers/Forms)
