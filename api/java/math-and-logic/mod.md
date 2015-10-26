---
layout: api-command
language: Java
permalink: api/java/mod/
command: mod
related_commands:
    add: add/
    sub: sub/
    mul: mul/
    div: div/
---

# Command syntax #

{% apibody %}
number.mod(number) &rarr; number
{% endapibody %}

Find the remainder when dividing two numbers.

__Example:__ It's as easy as 2 % 2 = 0.

```java
r.expr(2).mod(2).run(conn);
```

