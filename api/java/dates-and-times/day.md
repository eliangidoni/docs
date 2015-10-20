---
layout: api-command
language: Java
permalink: api/java/day/
command: day
related_commands:
    now: now/
    time: time/
---

# Command syntax #

{% apibody %}
time.day() &rarr; number
{% endapibody %}

# Description #

Return the day of a time object as a number between 1 and 31.

__Example:__ Return the users born on the 24th of any month.

```java
r.table("users").filter(
    row -> row.g("birthdate").day().eq(24)
).run(conn);
```


