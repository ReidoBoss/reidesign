[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

# Domain Overview

This section provides an overview of the **model's domain** guidelines and conventions.

## Naming Convention

- **case class and file names must match.**
- **case classes must be PascalCased.**
- **case classes should be singular.**

## Example:

```scala
// Person.scala ✅
case class Person(id:IdPerson, name:NamePerson , age: Int) ✅

// Persons.scala ❌
case class Persons(id:IdPerson, name:NamePerson , age: Int) ✅
```

## One case class per File

For easier navigation and better organization, each `.scala` file should contain **only one case class**.

### Do This:

```scala
// Person.scala
case class Person(id:IdPerson, name:NamePerson)

// PersonInformation.scala
case class PersonInformation(id:IdPerson, name:NamePerson, age: Int,address:String)
```

### Do Not Do This:

```scala
// Person.scala
case class Person(id:IdPerson, name:NamePerson)
case class PersonInformation(id:IdPerson, name:NamePerson, age: Int, address:String)
```

## File Names Should Start with the Main Persona

For clarity and a more hierarchical understanding, the **main Persona** should come first in file names, especially if the name consists of multiple words.

### Example:

```scala
WorkspaceMember.scala ✅
MemberWorkspace.scala ❌
```

## Include Implicits in the Singleton Object

To maintain proper serialization and deserialization, include the **implicit formats** of a case class within its companion object.

### Example:

```scala
case class Person(id:IdPerson, name:NamePerson , age: Int)

object Person:
  given Format[Person] = Json.format[Person]
```

## Main Domain case classes Should Be Concise

The **main domain** refers to core entities that represent primary business objects. Keep their names short and easy to understand.

### Example:

```scala
case class Task // main domain
case class TaskCreate // not a main domain
```

Main domains are **highly related** to a repo's table:

```scala
def add(person: Person): Future[Int] = db.run(this += person)
```

## Provide a `toDomain` Method If Needed

For non-main domains that need to be converted to a main domain, provide a `toDomain` method to ensure easy transformation.

### Example:

```scala
case class PersonInformation(name: NamePerson, age: Int, address: String):
  def toDomain: Person = Person(name, age)


val personWithAddress = PersonInformation("Joebert", 23, "Tejero")
val person = personWithAddress.toDomain
```

### Avoid:

```scala
case class PersonInformation(name: NamePerson, age: Int, address: String)
val personWithAddress = PersonInformation("Joebert",23,"Tejero")
val person = Person(personWithAddress.name,personWithAddress.age)
```

---

[Previous: Domain](Style-Guide/Models/Domain) | [Next: Naming Convention](Style-Guide/Models/Domain/Naming-Convention)
