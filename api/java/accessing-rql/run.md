---
layout: api-command
language: Java
permalink: api/java/run/
command: run
related_commands:
    connect: connect/
io:
    -   - any
        - null
---

# Command syntax #

{% apibody %}
query.run(conn)
{% endapibody %}

<img src="/assets/images/docs/api_illustrations/run.png" class="api_command_illustration" />

# Description #

Run a query on a connection, returning either a single JSON result or
a cursor, depending on the query.

You can pass the following options using [optArg](/api/java/optarg/):

- `use_outdated`: whether or not outdated reads are OK (default: `false`).
- `time_format`: what format to return times in (default: `'native'`).
  Set this to `'raw'` if you want times returned as JSON objects for exporting.
- `profile`: whether or not to return a profile of the query's
  execution (default: `false`).
- `durability`: possible values are `'hard'` and `'soft'`. In soft durability mode RethinkDB
will acknowledge the write immediately after receiving it, but before the write has
been committed to disk.
- `group_format`: what format to return `grouped_data` and `grouped_streams` in (default: `'native'`).
  Set this to `'raw'` if you want the raw pseudotype.
- `noreply`: set to `true` to not receive the result object or cursor and return immediately.
- `db`: the database to run this query against as a string. The default is the database specified in the `db` [connection](/api/java/connect/) method (which defaults to `test`). The database may also be specified with the [db](/api/java/db/) command.
- `array_limit`: the maximum numbers of array elements that can be returned by a query (default: 100,000). This affects all ReQL commands that return arrays. Note that it has no effect on the size of arrays being _written_ to the database; those always have an upper limit of 100,000 elements.
- `binary_format`: what format to return binary data in (default: `'native'`). Set this to `'raw'` if you want the raw pseudotype.
- `min_batch_rows`: minimum number of rows to wait for before batching a result set (default: 8). This is an integer.
- `max_batch_rows`: maximum number of rows to wait for before batching a result set (default: unlimited). This is an integer.
- `max_batch_bytes`: maximum number of bytes to wait for before batching a result set (default: 1MB). This is an integer.
- `max_batch_seconds`: maximum number of seconds to wait before batching a result set (default: 0.5). This is a float (not an integer) and may be specified to the microsecond.
- `first_batch_scaledown_factor`: factor to scale the other parameters down by on the first batch (default: 4). For example, with this set to 8 and `max_batch_rows` set to 80, on the first batch `max_batch_rows` will be adjusted to 10 (80 / 8). This allows the first batch to return faster.

```java
for (Map<String, Object> doc : r.table("marvel").run<Cursor<Map<String, Object>>(conn)) {
    System.out.println(doc);
}
```

__Example:__ If you are OK with potentially out of date data from all
the tables involved in this query and want potentially faster reads,
pass a flag allowing out of date data in an options object. Settings
for individual tables will supercede this global setting for all
tables in the query.

```java
r.table("marvel").run(conn).optArg("use_outdated", true)
```


__Example:__ If you just want to send a write and forget about it, you
can set `noreply` to true in the options. In this case `run` will
return immediately.

```java
r.table("marvel").run(conn).optArg("noreply", true)
```


__Example:__ If you want to specify whether to wait for a write to be
written to disk (overriding the table's default settings), you can set
`durability` to `'hard'` or `'soft'` in the options.

```java
r.table("marvel").insert(r.hashMap("superhero", "Iron Man")
    .with("superpower", "Arc Reactor"))
    .run(conn).optArg("noreply", true).optArg("durability", "soft")
```


__Example:__ If you do not want a time object to be converted to a
native date object, you can pass a `time_format` flag to prevent it
(valid flags are "raw" and "native"). This query returns an object
with two fields (`epoch_time` and `$reql_type$`) instead of a native date
object.

```py
r.now().run(conn).optArg("time_format", "raw")
```

__Example:__ Specify the database to use for the query.

```py
for doc in r.table('marvel').run(conn, db='heroes'):
    print doc
```

This is equivalent to using the `db` command to specify the database:

```py
r.db("heroes").table("marvel").run(conn)
```

__Example:__ Change the batching parameters for this query.

```py
r.table("marvel").run(conn).optArg("max_batch_rows", 16).optArg("max_batch_bytes", 2048)
```
