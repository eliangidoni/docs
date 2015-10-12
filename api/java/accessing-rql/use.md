---
layout: api-command
language: Java
permalink: api/java/use/
command: use
related_commands:
    connect: connect/
    reconnect: reconnect/
    close: close/
---

# Command syntax #

{% apibody %}
conn.use(dbName)
{% endapibody %}

# Description #

Change the default database on this connection.

__Example:__ Change the default database so that we don't need to
specify the database when referencing a table.

```java
conn.use("marvel");
r.table("heroes").run(conn);  // refers to r.db("marvel").table("heroes")
```
