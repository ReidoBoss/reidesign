---
title: Parameter Ordering
---
[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)


# `case class` Parameter Ordering

To maintain consistency and clarity across our domain models, we follow specific rules for ordering parameters.

Order:

```
1. Primary Key
2. Details
3. Foreign Keys
4. createdAt and deletedAt
```

## 1. Primary Key

Always put the id of that domain first and make sure that is the primary key of a domain.

```scala
case class Task(
  id:IdTask,
  other info...
)
```

- The ID must represent the primary key for the domain.
- The ID should always be placed first, followed by other relevant properties of the domain.

## 2. Details

Put the details of that domain and then make sure it is in order based on how relevant it is.

```scala
case class Task(
  id:IdTask,
  name:IdTask,
  description:Option[String],
  dueDate:LocalDateTime,
  ... other info
)
```

`name` of the task should be first since it is more relevant than description.
`description`, description should be less relevant than `name` since it is optional, but that doesn't mean that.
`dueDate`, should be less relevant than `description`.

## 3. Foreign Keys

Put the foreign keys after the details.

```scala
case class Task(
  id:IdTask,
  name:IdTask,
  description:Option[String],
  dueDate:LocalDateTime,
  idProject:IdProject
)
```

`IdProject` is the foreign key of this domain.

## 4. createdAt, deletedAt

This is important since we have to know if when a domain is created and deleted and most of the time, we will put default values here, hence we should put them at the last.

```scala
case class Task(
  id: IdTask,
  name: String,
  description: Option[String],
  dueDate: LocalDateTime,
  idProject: IdProject,
  createdAt: LocalDateTime = LocalDateTime.now,
  deletedAt: Option[LocalDateTime] = None
)
```

As you can see, we have default values for `createdAt` and `deletedAt`.

---

[Previous: Naming Convention](Style-Guide/Models/Domain/Naming-Convention) | [Next: Forms](Style-Guide/Models/Domain/Forms)