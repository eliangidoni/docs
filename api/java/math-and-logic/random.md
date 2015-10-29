---
layout: api-command
language: Java
permalink: api/java/random/
command: random
related_commands:
    'sample': sample/
io:
    - r
    - number
---

# Command syntax #

{% apibody %}
r.random() &rarr; number
r.random(number[, number]) &rarr; number
r.random(integer[, integer]) &rarr; integer
{% endapibody %}

# Description #

Generate a random number between given (or implied) bounds. `random` takes zero, one or two arguments, and can also take an [optArg](/api/java/optarg) of `float`.

- With __zero__ arguments, the result will be a floating-point number in the range `[0,1)` (from 0 up to but not including 1).
- With __one__ argument _x,_ the result will be in the range `[0,x)`, and will be integer unless `.optArg("float", true)` is given as an option. Specifying a floating point number without the `float` option will raise an error.
- With __two__ arguments _x_ and _y,_ the result will be in the range `[x,y)`, and will be integer unless `.optArg("float", true)` is given as an option.  If _x_ and _y_ are equal an error will occur, unless the floating-point option has been specified, in which case _x_ will be returned. Specifying a floating point number without the `float` option will raise an error.

Note: The last argument given will always be the 'open' side of the range, but when generating a floating-point number, the 'open' side may be less than the 'closed' side.

__Example:__ Generate a random number in the range `[0,1)`

```java
r.random().run(conn);
```


__Example:__ Generate a random integer in the range `[0,100)`

```java
r.random(100).run(conn);
r.random(0, 100).run(conn);
```


__Example:__ Generate a random number in the range `(-2.24,1.59]`

```java
r.random(1.59, -2.24).optArg("float", true).run(conn)
```

