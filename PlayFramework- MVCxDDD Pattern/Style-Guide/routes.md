[Home](Home) | [Pattern](Style-Guide/Pattern) | [Naming Convention](Style-Guide/Naming-Convention) | [Importing](Style-Guide/Importing) | [Controllers](Style-Guide/Controllers) | [Models](Style-Guide/Models) | [Routes](Style-Guide/routes) | [Configurations](Style-Guide/Configurations)

---

# Routes

Make sure to follow [Restful-Api](https://restfulapi.net/) best practices.

## Make sure to use plural words

### ❌ Do no do this

```
GET  /me/workspace  controllers.WorkspaceController.all
```

### ✅ Do this

```
GET  /me/workspaces  controllers.WorkspaceController.all
```

## use `/me` to signify ownership if needed

```
GET  /me/workspaces  controllers.WorkspaceController.all
```

- This signifies to get all my workspaces

```
GET  /me/invites/workspaces  controllers.WorkspaceInviteController.all
```

- This signifies to get all my invites

### Here i don't need to use `/me` because it doesn't have a direct relationship to `me`.

```
PATCH   /invites  controllers.WorkspaceInviteController.update
POST    /invites  controllers.WorkspaceInviteController.create
DELETE  /invites  controllers.WorkspaceInviteController.delete
```


# Route Design: Avoid Misrepresenting Resource Relationships

Including `admin` in the route path does not accurately reflect the domain's structure. Routes should represent the resource itself, not imply administrative ownership unless it is directly relevant to the resource. Adding "admin" in the path can create confusion and mislead developers about the relationship between the resource and the persona.

### Why It’s Problematic

The way this route is structured:

```plaintext
POST   /admin/lessons
GET    /admin/lessons
PUT    /admin/lessons
PATCH  /admin/lessons
```

It suggests that `lessons` are owned by or directly associated with an `admin`. However, the `admin` does not own or have a direct relationship with `lessons`; instead, the `admin` is simply a persona authorized to perform actions (e.g., create, retrieve, update) on `lessons`.

As a developer familiar with RESTful API design, this could be interpreted as implying:

- There is a direct connection between `admin` and `lessons`, such as a `has-a` or `owns` relationship.
- The `lessons` resource is inherently tied to the `admin` entity.

This is incorrect because `admin` is merely an actor who performs operations on the resource.

### Correct Approach

The routes should focus on the resource itself and not include unnecessary persona-specific prefixes unless they are contextually necessary. Instead of the above, you can structure your routes like this:

```plaintext
POST   /lessons
GET    /lessons
PUT    /lessons
PATCH  /lessons
```

### Key Takeaway

Routes should:

- Represent the resource and its hierarchy clearly.
- Avoid persona-related prefixes unless they are essential to the resource context.
- Use HTTP methods and resource-based paths to convey intent.

---

### Learn More

To avoid issues with route design, ensure you are familiar with the principles of [RESTful APIs](https://restfulapi.net/). Proper understanding will help maintain clarity, consistency, and scalability in your application.
--- 
[Previous: Models](Style-Guide/Models) | [Next: Configurations](Style-Guide/Configurations)