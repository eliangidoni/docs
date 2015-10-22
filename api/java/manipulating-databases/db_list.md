---
layout: api-command
language: Java
permalink: api/java/db_list/
command: dbList
related_commands:
    dbCreate: db_create/
    dbDrop: db_drop/
    tableCreate: table_create/
---

# Command syntax #

{% apibody %}
r.dbList() &rarr; array
{% endapibody %}

# Description #

List all database names in the system. The result is a list of strings.

__Example:__ List all databases.

```java
r.dbList().run(conn);
```
