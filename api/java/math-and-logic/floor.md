---
layout: api-command
language: Java
permalink: api/java/floor/
command: floor
related_commands:
    ceil: ceil/
    round: round/
---
# Command syntax #

{% apibody %}
r.floor(number) &rarr; number
number.floor() &rarr; number
{% endapibody %}

# Description #

Rounds the given value down, returning the largest integer value less than or equal to the given value (the value's floor).

__Example:__ Return the floor of 12.345.

```java
r.floor(12.345).run(conn);

// Result:
12.0
```

The `floor` command can also be chained after an expression.

__Example:__ Return the floor of -12.345.

```java
r.expr(-12.345).floor().run(conn);

// Result:
-13.0
```

__Example:__ Return Iron Man's weight, rounded down with `floor`.

```java
r.table("superheroes").get("ironman")("weight").floor().run(conn);
```
