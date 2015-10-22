---
layout: api-command
language: Java
permalink: api/java/db_drop/
command: dbDrop
related_commands:
    dbCreate: db_create/
    dbList: db_list/
    tableCreate: table_create/
---

# Command syntax #

{% apibody %}
r.dbDrop(dbName) &rarr; object
{% endapibody %}

# Description #

Drop a database. The database, all its tables, and corresponding data will be deleted.

If successful, the command returns an object with two fields:

* `dbs_dropped`: always `1`.
* `tables_dropped`: the number of tables in the dropped database.
* `config_changes`: a list containing one two-field object, `old_val` and `new_val`:
    * `old_val`: the database's original [config](/api/java/config) value.
    * `new_val`: always `null`.

If the given database does not exist, the command throws `ReqlRuntimeError`.

__Example:__ Drop a database named 'superheroes'.

```java
r.dbDrop("superheroes").run(conn);
```

Return:

```json
{
    "config_changes": [
        {
            "old_val": {
                "id": "e4689cfc-e903-4532-a0e6-2d6797a43f07",
                "name": "superheroes"
            },
            "new_val": null
        }
    ],
    "tables_dropped": 3,
    "dbs_dropped": 1
}
```

