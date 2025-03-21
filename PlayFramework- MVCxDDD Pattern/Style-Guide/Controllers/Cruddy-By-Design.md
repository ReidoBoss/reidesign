## [Home](Home) | [Style Guide](Style-Guide)

## [CRUDdy by Design](https://github.com/adamwathan/laracon2017/blob/master/readme.md) in Controller

### Note:

Remember rule **NWCA** _(Never Write Custom Action)_

#### Example:

```scala
/* Task Controller */
def createTask = ??? ❌
def create = ??? ✅
```

### If a custom action should be made, make another controller

#### Example

```scala
/* TaskController.scala */
def updateTaskThumbnaiil = ???
/*
    if this happens, go create another controller
    dedicated for updating the task thumbnail
*/

/* SOLUTION */
/* TaskThumbnailController.scala */
def update = ???
```

### CRUD-Based Naming Convention

When naming methods for CRUD operations, follow this standardized convention to maintain clarity and consistency throughout the application:

```scala
def create = ??? /* Logic for creating a new resource */
def all = ??? /* Fetch all resources without any filters */
def find = ??? /* Fetch a single resource by unique identifier */
def get = ??? /* Fetch a filtered list of resources */
def delete = ??? /* Logic for deleting a resource */
def update = ??? /* Update the entire resource */
def edit = ??? /* Update a portion of the resource */
```

### HTTP Methods Mapping

Each CRUD action should align strictly with its corresponding HTTP method for consistency with RESTful API design principles:

- `create` → `POST`
- `all` → `GET`
- `find` → `GET`
- `get` → `GET`
- `delete` → `DELETE`
- `update` → `PUT` (Complete replacement of the resource)
- `edit` → `PATCH` (Partial update of the resource)

[Previous:Controllers](Style-Guide/Controllers)
