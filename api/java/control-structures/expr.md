---
layout: api-command
language: Java
permalink: api/java/expr/
command: expr
io:
    -   - r
        - value
---

# Command syntax #

{% apibody %}
r.expr(value) &rarr; value
{% endapibody %}

# Description #

Construct a ReQL JSON object from a native object.

If the native object is of type `bytes[]`, then `expr` will return a binary object. See [binary](/api/java/binary) for more information.

__Example:__ Objects wrapped with expr can then be manipulated by ReQL API functions.

```java
import com.rethinkdb.model.MapObject;

// Create object { "a": "b" }
MapObject newData = new MapObject().with("a", "b");

// merge with { "b": [1, 2, 3] }
r.expr(newData).merge(r.hashMap("b", r.array(1, 2, 3))).run(conn);
```
