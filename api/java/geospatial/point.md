---
layout: api-command
language: Java
permalink: api/java/point/
command: point
related_commands:
    line: line/
    polygon: polygon/
    circle: circle/
---
# Command syntax #

{% apibody %}
r.point(longitude, latitude) &rarr; point
{% endapibody %}

# Description #

Construct a geometry object of type Point. The point is specified by two floating point numbers, the longitude (&minus;180 to 180) and latitude (&minus;90 to 90) of the point on a perfect sphere. See [Geospatial support](/docs/geo-support/) for more information on ReQL's coordinate system.

__Example:__ Define a point.

```java
r.table("geo").insert(
    r.hashMap("id", 1)
     .with("name", "San Francisco")
     .with("location", r.point(-122.423246, 37.779388))
).run(conn);
```
