---
layout: api-command
language: Java
permalink: api/java/distance/
command: distance
related_commands:
    polygon: polygon/
    line: line/
---
# Command syntax #

{% apibody %}
geometry.distance(geometry) &rarr; number
r.distance(geometry, geometry) &rarr; number
{% endapibody %}

# Description #

Compute the distance between a point and another geometry object. At least one of the geometry objects specified must be a point.

Optional arguments available with `distance` are:

* `geo_system`: the reference ellipsoid to use for geographic coordinates. Possible values are `WGS84` (the default), a common standard for Earth's geometry, or `unit_sphere`, a perfect sphere of 1 meter radius.
* `unit`: Unit to return the distance in. Possible values are `m` (meter, the default), `km` (kilometer), `mi` (international mile), `nm` (nautical mile), `ft` (international foot).

If one of the objects is a polygon or a line, the point will be projected onto the line or polygon assuming a perfect sphere model before the distance is computed (using the model specified with `geo_system`). As a consequence, if the polygon or line is extremely large compared to Earth's radius and the distance is being computed with the default WGS84 model, the results of `distance` should be considered approximate due to the deviation between the ellipsoid and spherical models.


__Example:__ Compute the distance between two points on the Earth in kilometers.

```java
r.distance(
    r.point(-122.423246,37.779388),
    r.point(-117.220406,32.719464)
).optArg("unit", "km").run(conn);

// Result:
734.1252496021841
```
