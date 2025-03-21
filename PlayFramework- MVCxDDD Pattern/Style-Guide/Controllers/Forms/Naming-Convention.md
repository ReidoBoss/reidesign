## [Home](Home) | [Style Guide](Style-Guide)

# Controller Forms Naming Convention

## Implement [CRUDdy by Design](Style-Guide/Controllers/Cruddgy-By-Design)

- Always put it an object. Relative to the Controller that will use it so if the Controller that will use it is named `ProjectController`, the name of the object should be `ProjectForm`

Example:

```scala
object ProjectForm:
  val create = Form(
    mapping(
      "name"        -> nonEmptyText(maxLength = 50),
      "idWorkspace" -> uuid
    )(ProjectCreate.apply)(ProjectCreate.unapply)
  )

  val delete = Form(
    single(
      "id" -> uuid
    )
  ) // No need to have a case class if you only need one

  val update = Form(
    mapping(
      "id"          -> uuid,
      "newName"     -> nonEmptyText(maxLength = 50),
      "idWorkspace" -> uuid
    )(ProjectUpdate.apply)(ProjectUpdate.unapply)
  )
```

---

[Previous: Forms](Style-Guide/Controllers/Forms)
