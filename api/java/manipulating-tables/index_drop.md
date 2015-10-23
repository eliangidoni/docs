---
layout: api-command
language: Java
permalink: api/java/index_drop/
command: indexDrop
related_commands:
    indexCreate: index_create/
    indexList: index_list/

---

# Command syntax #

{% apibody %}
table.indexDrop(indexName) &rarr; object
{% endapibody %}

# Description #

Delete a previously created secondary index of this table.

__Example:__ Drop a secondary index named 'code_name'.

```java
r.table("dc").indexDrop("code_name").run(conn);
```


