---
layout: api-command
language: Java
permalink: api/java/gt/
command: gt
related_commands:
    eq: eq
    ne: ne/
    ge: ge/
    lt: lt/
    le: le/
---

# Command syntax #

{% apibody %}
value.gt(value[, value, ...]) &rarr; bool
{% endapibody %}

# Description #

Compare values, testing if the left-hand value is greater than the right-hand.

__Example:__ Test if a player has scored more than 10 points.

```java
r.table("players").get(1)("score").gt(10).run(conn);
```

__Example:__ Test if variables are ordered from lowest to highest, with no values being equal to one another.

```java
int a = 10;
int b = 20;
int c = 15;
r.gt(a, b, c).run(conn);
```

This is the equivalent of the following:

```java
r.gt(a, b).and(r.gt(b, c)).run(conn);
```
