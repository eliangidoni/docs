---
layout: api-command
language: Java
permalink: api/java/now/
command:  now
related_commands:
    time: time/
    epochTime: epoch_time/
    ISO8601: iso8601/
---

# Command syntax #

{% apibody %}
r.now() &rarr; time
{% endapibody %}

# Description #

Return a time object representing the current time in UTC. The command now() is computed once when the server receives the query, so multiple instances of r.now() will always return the same time inside a query.

__Example:__ Add a new user with the time at which he subscribed.

```java
r.table("users").insert(
    r.hashMap("name", "John")
     .with("subscription_date", r.now())
).run(conn);
```

