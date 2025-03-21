---
title: Actions
---
[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)


# Table Actions

This section will talk about the `methods` or `functions` in the Table's Repo.

## [CRUDdy naming Convention](Style-Guide/Controllers/Cruddy-By-Design)

We will stil implement `CRUDdy by Design` in our `methods` or `functions`.

## Object `Table`

The object `Table` should look like this. this is where we put our basic queries.

```scala
  object Table extends TableQuery(new Tasks(_)):

    @targetName("getIdTask")
    def get(id: IdTask): Future[Option[Task]] =
      db.run(this.filter(_.id === id).result.headOption)

    @targetName("getIdWorkspace")
    def get(id: IdWorkspace): Future[Option[Task]] =
      db.run(this.filter(_.idWorkspace === id).result.headOption)

    @targetName("getNameTask")
    def get(name: NameTask): Future[Option[Task]] =
      db.run(this.filter(_.name === name).result.headOption)

  end Table
```

make sure to always put the annotation `@targetName` to distinguish ambiguity in the compiler.

```scala
import scala.annotation.targetName
```

---
[Previous: Table](Style-Guide/Models/Repo/Table) | [Next: package.scala](Style-Guide/Models/Repo/package-scala)