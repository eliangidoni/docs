---
layout: api-command
language: Ruby
permalink: api/ruby/http/
command: http
io:
    -   - r
        - value
    -   - r
        - stream
---

# Command syntax #

{% apibody %}
r.http(url[, options]) &rarr; value
r.http(url[, options]) &rarr; stream
{% endapibody %}

# Description #

Retrieve data from the specified URL over HTTP.  The return type depends on the `result_format` option, which checks the `Content-Type` of the response by default.

__Example:__ Perform an HTTP `GET` and store the result in a table.

```rb
r.table('posts').insert(r.http('http://httpbin.org/get')).run(conn)
```

See [the tutorial](/docs/external-api-access/) on `r.http` for more examples on how to use this command.

# Options #

## General Options ##

* `timeout`: timeout period in seconds to wait before aborting the connect (default `30`).
* `reattempts`: number of retry attempts to make after failed connections (default `5`).
* `redirects`: number of redirect and location headers to follow (default `1`).
* `verify`: if `true`, verify the server's SSL certificate (default `true`).
* `result_format`: string specifying the format to return results in. One of the following:
    * `text`: always return a string.
    * `json`: parse the result as JSON, raising an error on failure.
    * `jsonp`: parse the result as [Padded JSON][jsonp].
    * `binary`: return a binary object.
    * `auto`: parse the result based on its `Content-Type` (the default):
        * `application/json`: as `json`
        * `application/json-p`, `text/json-p`, `text/javascript`: as `jsonp`
        * `audio/*`, `video/*`, `image/*`, `application/octet-stream`: as `binary`
        * anything else: as `text`

[jsonp]: https://en.wikipedia.org/wiki/JSONP

## Request Options

* `method`: HTTP method to use for the request. One of `GET`, `POST`, `PUT`, `PATCH`, `DELETE` or `HEAD`. Default: `GET`.
* `auth`: object giving authentication, with the following fields:
    * `type`: `basic` (default) or `digest`
    * `user`: username
    * `pass`: password in plain text
* `params`: object specifying URL parameters to append to the URL as encoded key/value pairs. `{ :query => 'banana', :limit => 2 }` will be appended as `?query=banana&limit=2`. Default: no parameters.
* `header`: Extra header lines to include. The value may be an array of strings or an object. Default: `Accept-Encoding: deflate;q=1, gzip;q=0.5` and `User-Agent: RethinkDB/<VERSION>`.
* `data`: Data to send to the server on a `POST`, `PUT`, `PATCH`, or `DELETE` request. For `POST` requests, data may be either an object (which will be written to the body as form-encoded key/value pairs) or a string; for all other requests, data will be serialized as JSON and placed in the request body, sent as `Content-Type: application/json`. Default: no data will be sent.

__Example:__ Perform multiple requests with different parameters.

```rb
r.expr([1, 2, 3]).map{|i|
    r.http('http://httpbin.org/get', :params => { :user => i })
}.run(conn)
```

__Example:__ Perform a `PUT` request for each item in a table.

```rb
r.table('data').map{|row|
    r.http('http://httpbin.org/put', :method => 'PUT', :data => row)
}.run(conn)
```

__Example:__ Perform a `POST` request with accompanying data.

Using form-encoded data:

```rb
r.http('http://httpbin.org/post',
       :method => 'POST',
       :data => { :player => 'Bob', :game => 'tic tac toe' }).run(conn)
```

Using JSON data:

```rb
r.http('http://httpbin.org/post',
       :method => 'POST',
       :data => r.expr(value).coerce_to('string'),
       :header => { 'Content-Type' => 'application/json' }).run(conn)
```

## Pagination

`r.http` supports depagination, which will request multiple pages in a row and aggregate the results into a stream.  The use of this feature is controlled by the optional arguments `page` and `page_limit`.  Either none or both of these arguments must be provided.

* `page`: This option may specify either a built-in pagination strategy (see below), or a function to provide the next URL and/or `params` to request.
* `page_limit`: An integer specifying the maximum number of requests to issue using the `page` functionality.  This is to prevent overuse of API quotas, and must be specified with `page`.
    * `-1`: no limit
    * `0`: no requests will be made, an empty stream will be returned
    * `n`: `n` requests will be made

At the moment, the only built-in strategy is `'link-next'`, which is equivalent to `lambda { |info| info['header']['link']['rel="next"'].default(nil) }`.

__Example:__ Perform a GitHub search and collect up to 3 pages of results.

```rb
r.http("https://api.github.com/search/code?q=addClass+user:mozilla",
       :page => 'link-next', :page_limit => 3).run(conn)
```

As a function, `page` takes one parameter, an object of the format:

```rb
{
    :params => object, # the URL parameters used in the last request
    :header => object, # the HTTP headers of the last response as key/value pairs
    :body => value # the body of the last response in the format specified by `result_format`
}
```

The `header` field will be a parsed version of the header with fields lowercased, like so:

```rb
{
    'content-length' => '1024',
    'content-type' => 'application/json',
    'date' => 'Thu, 1 Jan 1970 00:00:00 GMT',
    'link' => {
        'rel="last"' => 'http://example.com/?page=34',
        'rel="next"' => 'http://example.com/?page=2'
    }
}
```

The `page` function may return a string corresponding to the next URL to request, `nil` indicating that there is no more to get, or an object of the format:

```rb
{
    :url => string, # the next URL to request, or nil for no more pages
    :params => object # new URL parameters to use, will be merged with the previous request's params
}
```

__Example:__ Perform depagination with a custom `page` function.

```rb
r.http('example.com/pages',
       :page => lambda {|info| info['body']['meta']['next'].default(nil)},
       :page_limit => 5).run(conn)
```

# Learn more

See [the tutorial](/docs/external-api-access/) on `r.http` for more examples on how to use this command.
