---
layout: post
title: "Log For Humans and Machines"
---

Logging is one of the easiest ways for an application to communicate information
to humans or other machines. How many requests has the application received or
how quickly has it served responses? How slow are the slowest actions and how
often do they occur? These can be easily communicated through logs.

We log everything at Heroku. To make logging consumable by humans and machines,
we standardized around key-value formatting. With the use of common keys, humans
and machines can understand what an application is experiencing everywhere.

## Common Practices

Logs should have an `app` key with the value matching the application or project
name. These key-values will look like `app=api`, `app=foo-bot`, or something
similar. This key-value pair becomes essential for filtering when multiple log
streams are consumed by a single source.

The `at` key is used to indicate the overall action or purpose of the code being
executed at the time of logging. If the code’s purpose is to output application
state, each log line should have an `at=info` key-value pair. The `at` value may
be `seed` for logs generated when the application is seeding the database.

The `fn` key can be used to show what function or method is being executed when
the log line is generated. This key-value is very useful for troubleshooting.

Logs can be "tagged" with additional key-values to indicate what actions have or
have not occurred. A tag is a key with a value of `true`.

Application errors can be passed to logs with a key `error` and value as the
message string.

Key-value pairs that are meant to be read by machines are prefixed with
`measure.`. This allows to fast and easy scanning.

```
app=example at=seed user=jane@example.com fn=create exists=true
app=example at=seed user=john@example.com fn=create
app=example at=seed user=john@example.com fn=verify error="No DOB"
```

The logs above show the application was seeding a database with two users, Jane
and John. When the `create` function was called for Jane's user, it checked to
see if the user already existed. Because her user already existed, the log was
tagged with `exists=true`. John's user, however, did not exist and was created.
When the `verify` function was called for his user an error occurred and the
message was logged.

## Machines Do It Better

Humans can only read and process a handful of logs at one time, but machines can
process an endless supply. Whether logs are received at the rate of ten per
minute or ten thousand per second, machines do it better.

Trends, historical views, alerting, and much more can be offered by machines
consuming logs. These tasks, at scale, would be impossible for a human.

```
app=example at=write measure.items=1000 measure.elapsed=5
app=example at=write measure.items=1500 measure.elapsed=10
app=example at=write measure.items=1010 measure.elapsed=4
```

The above logs indicate an application has completed three write actions. The
number of items written is shown with `measure.items` and the value is intended
to be consumed by a machine. The value of `measure.elapsed` shows how long the
action took to complete in milliseconds and is also meant for machine consumers.

To make log parsing easy and consistent across languages, a series of libraries
have been created to do this for us. These libraries exist for [Golang][1],
[Python][2], [Erlang][3], [Node.JS][4], [Ruby][5], and [Java][6].

## Conclusion

Logging is necessary for every application. Machines consuming logs can only
produce information as useful the log data they are given. Applications should
take full advantage of how and when logs are generated. Log for humans and
machines.

[1]: https://github.com/kr/logfmt
[2]: https://github.com/jkakar/logfmt-python
[3]: https://github.com/tsloughter/logfmt-erlang
[4]: https://github.com/csquared/node-logfmt
[5]: https://github.com/cyberdelia/logfmt-ruby
[6]: https://github.com/naaman/logfmt-java
