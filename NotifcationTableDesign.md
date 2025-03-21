# Notification Table Design

This design is inspired from these two designs:
- [CRUDdy by Design](https://github.com/adamwathan/laracon2017)
- [Designing a Notificaton Design](https://tannguyenit95.medium.com/designing-a-notification-system-1da83ca971bc)

## Key Values

### 1. Sender

The sender will be the one who is Responsible for the notification.

#### There are two types of senders:

1. **User** as a Sender
   - A user responsible for sending the notification the the receiveres, will have the `ID_SENDER` in the `"NOTIFICATIONS"` Table.
2. **System** as a Sender
   - Useful for something like a **reminder** notification.
   - Since a system doesn't have an `ID`, if the `ID_SENDER` is null, it should be understood that it is a `System as a Sender`.

### 2. Receiver

- The one that will receive the notification.
- will have the `ID_RECEIVER` in the `"NOTIFICATIONS"` Table.

### 3. Notification Entity

This will describe what kind of entity that notification is.

We should have a different table for this called `NOTIFICATION_ENTITIES` with a column `NAME` which should be named like this `TASK, WORKSPACE, TASK_DUE_REMINDER`, This should be descriptive.

### 4. Notification Entity Type

The entity type should only be limited to `CREATE,READ,UPDATE,DELETE`
which is the basic CRUD.

# ERD

![image](https://github.com/user-attachments/assets/b6f66fa8-30a1-4c46-b372-25f7cfc0e3e1)

## Example

### Creating a `Task` Notification.

`INSERT` a new **Notification Entity** in the `NOTIFICATON_ENTITIES` Table.

```sql
INSERT INTO "NOTIFICATION_ENTITIES" (name)
VALUES ('TASK');
```

Now the notification should look like this .
![image](https://github.com/user-attachments/assets/8ecb36bb-62e7-4a66-a139-1a75dd67c8b3)

`"Sender"` `"Created"` a `"Task"` at `"TimeStamp"`.

### Creating a `Task Reminder` Notification.

`INSERT` a new **Notification Entity** in the `NOTIFICATON_ENTITIES` Table.

```sql
INSERT INTO "NOTIFICATION_ENTITIES" (name)
VALUES ('TASK_DUE_REMINDER');
```

Now the notification table should look like this .
![image](https://github.com/user-attachments/assets/28b6c6f1-623d-45e5-b314-ee53410eefd2)

`"Sender"` - null so it is a System's Notification
`"Created"` a `"TASK_DUE_REMINDER"` at `"TimeStamp"`.
