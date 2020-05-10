---
title: Versioning Your API
layout: post
---

Choosing an API versioning strategy can be difficult. There are many approaches and
each come with their own pros and cons. Two of the most common practices are to version
in the URL or with headers.  For Heroku's public API we decided to version with the
`Accept` header.

<!-- more -->

## URL Versioning is Dead Simple

If you're not familiar with URL versioning it's simply having the version number as
part of the URI, for instance, `example.com/v2/resources`. This approach is very clear
and easy to call in code or at the command-line. The problem I see with URL versioning
is that it's not forgiving. If/when the version is no longer available the server will
return a `404` response.

## Accept Header Versioning is Forgiving

To version with the `Accept` header the API has to listen for a custom media type and
respond accordingly. Github chose the media type `application/vnd.github.v3+json`
where `3` is the API version. For Heroku's API we decided on
`application/vnd.heroku+json; version=3`, also where `3` is our version of the API.

In section [14.1 of the HTTP Spec][1] the document indicates that multiple media types
can be passed in the `Accept` header. If the first media type requested isn't available
the next type can be returned and it continues down the line. If the following `Accept`
header is sent to an endpoint that does not have a version 3 response it will disregard
the Heroku media type and return the default response as JSON.

```
Accept: application/vnd.heroku+json; version=3, application/json
```

I prefer this approach over URL versioning because of the ability to set multiple
types in one request.

## Understanding a Custom Media Type

A media type consists of two or more parts---type, subtype, and optional parameters.
The [Media Type Spec][2] states the `application` type is meant for content that is
to  be processed by applications before being viewed or usable by a user. That
sounds perfect for an API.

Sub types that begin with `vnd` are vendor specific media types. These media types
are defined and controlled by the vendor. Creation of and modifications to vendor
media types aren't subject to community reviews.

The `+json` indicates that the content is a JSON structure. This is key to custom
media types, whether a vendor type or experimental (prefixed with `x-`), because
it shows how the data should be parsed once received.

Lastly, parameters allow for additional information to be passed without changing the
media type. A `version` parameter can be used to support multiple API versions during
a transition period.

In the case of Heroku's API we will respond to `application/json` by default and
`application/vnd.heroku+json; version=2` and `application/vnd.heroku+json; version=3`
upon request. The default response will render the same as `vnd.heroku+json; version=2`
until version 3 is completed and widely adopted. At that point the default
`application/json` response will render the same as `vnd.heroku+json; version=3` and
version 2 will be available through the custom media type.

[1]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1
[2]: http://tools.ietf.org/html/rfc4288#page-9
