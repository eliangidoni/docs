---
layout: api-command
language: Java
permalink: api/java/avg/
command: avg
related_commands:
    map: map/
    reduce: reduce/
    count: count/
    sum: sum/
    min: min/
    max: max/
    group: group/
---

# Command syntax #

{% apibody %}
sequence.avg([field | function]) &rarr; number
{% endapibody %}

# Description #

Averages all the elements of a sequence.  If called with a field name,
averages all the values of that field in the sequence, skipping
elements of the sequence that lack that field.  If called with a
function, calls that function on every element of the sequence and
averages the results, skipping elements of the sequence where that
function returns `null` or a non-existence error.

Produces a non-existence error when called on an empty sequence.  You
can handle this case with `default`.

__Example:__ What's the average of 3, 5, and 7?

```java
r.expr(r.array(3, 5, 7)).avg().run(conn);
```

__Example:__ What's the average number of points scored in a game?

```java
r.table("games").avg("points").run(conn);
```

__Example:__ What's the average number of points scored in a game,
counting bonus points?

```java
r.table("games").avg(
    game -> game.g("points").add(game.g("bonus_points"))
).run(conn);
```

__Example:__ What's the average number of points scored in a game?
(But return `null` instead of raising an error if there are no games where
points have been scored.)

```java
r.table("games").avg("points").default_(null).run(conn);
```
