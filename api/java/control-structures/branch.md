---
layout: api-command
language: Java
permalink: api/java/branch/
command: branch
related_commands:
    do: do/
---

# Command syntax #

{% apibody %}
r.branch(test, true_action[, test2, else_action, ...], false_action) &rarr; any
{% endapibody %}

# Description #

Perform a branching conditional equivalent to `if-then-else`.

The `branch` command takes 2n+1 arguments: pairs of conditional expressions and commands to be executed if the conditionals return any value but `false` or `null` (i.e., "truthy" values), with a final "else" command to be evaluated if all of the conditionals are `false` or `null`.

```
r.branch(test1, val1, test2, val2, elseval)
```

is the equivalent of the Java statement

```java
if (test1) {
    return val1;
} else if (test2) {
    return val2;
} else {
    return elseval;
}
```

__Example:__ Test the value of x.

```java
int x = 10;
r.branch(r.expr(x).gt(5), "big", "small").run(conn);

// Result
"big"
```

__Example:__ Categorize heroes by victory counts.

```java
r.table("marvel").map(hero -> r.branch(
    hero.g("victories").gt(100),
    hero.g("name").add(" is a superhero"),
    hero.g("victories").gt(10),
    hero.g("name").add(" is a hero"),
    hero.g("name").add(" is very nice")
)).run(conn);

```

If the documents in the table `marvel` are:

```json
[
    { "name": "Iron Man", "victories": 214 },
    { "name": "Jubilee", "victories": 49 },
    { "name": "Slava", "victories": 5 }
]
```

The results will be:

```json
[
    "Iron Man is a superhero",
    "Jubilee is a hero",
    "Slava is very nice"
]
```
