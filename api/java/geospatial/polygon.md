---
layout: api-command
language: Java
permalink: api/java/polygon/
command: polygon
related_commands:
    point: point/
    line: line/
    circle: circle/
---
# Command syntax #

{% apibody %}
r.polygon([lon1, lat1], [lon2, lat2], [lon3, lat3], ...) &rarr; polygon
r.polygon(point1, point2, point3, ...) &rarr; polygon
{% endapibody %}

# Description #

Construct a geometry object of type Polygon. The Polygon can be specified in one of two ways:

* Three or more two-item arrays, specifying latitude and longitude numbers of the polygon's vertices;
* Three or more [Point](/api/java/point) objects specifying the polygon's vertices.

Longitude (&minus;180 to 180) and latitude (&minus;90 to 90) of vertices are plotted on a perfect sphere. See [Geospatial support](/docs/geo-support/) for more information on ReQL's coordinate system.

If the last point does not specify the same coordinates as the first point, `polygon` will close the polygon by connecting them. You cannot directly construct a polygon with holes in it using `polygon`, but you can use [polygonSub](/api/java/polygon_sub) to use a second polygon within the interior of the first to define a hole.


__Example:__ Define a polygon.

```java
r.table("geo").insert(
    r.hashMap("id", 101)
     .with("rectangle", r.polygon(
        r.array(-122.423246, 37.779388),
        r.array(-122.423246, 37.329898),
        r.array(-121.88642, 37.329898),
        r.array(-121.88642, 37.779388))
    )
).run(conn);
```
