---
layout: api-command
language: Java
permalink: api/java/le/
command: le
related_commands:
    eq: eq/
    ne: ne/
    gt: gt/
    ge: ge/
    lt: lt/
---

# Command syntax #

{% apibody %}
value.le(value[, value, ...]) &rarr; bool
{% endapibody %}

# Description #

Compare values, testing if the left-hand value is less than or equal to the right-hand.

__Example:__ Test if a player has scored 10 points or less.

```java
r.table("players").get(1)("score").le(10).run(conn);
```

__Example:__ Test if variables are ordered from highest to lowest.

```java
int a = 20;
int b = 10;
int c = 15;
r.le(a, b, c).run(conn);
```

This is the equivalent of the following:

```java
r.le(a, b).and(r.le(b, c)).run(conn);
```
