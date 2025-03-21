[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

---
# Package naming

In this section we will talk about how we should name our packages

It's pretty simple. If you have a folder called `domain` which is under of folder `models`
This is what you should do:

```scala
package models.domain
```
if the scala file is on the folder level
```scala
package controllers
```

even if the folder structure is

```scala
models/domain
```
if you need a nested package. 
```scala
package example.test
```
Separate it into two: 
```scala
package example
package test
```
---

[Previous: Implicit Naming](Style-Guide/Naming-Convention/Implicit-Naming) | [Next: Importing](Style-Guide/Importing)