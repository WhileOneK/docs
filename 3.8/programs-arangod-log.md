---
layout: default
description: ArangoDB Server Log Options
---
# ArangoDB Server Log Options

## Log levels and topics

ArangoDB's log output is grouped into topics. `--log.level` can be specified
multiple times at startup, for as many topics as needed. The log verbosity and
output files can be adjusted per log topic. For example

```
--log.level startup=trace --log.level queries=trace --log.level info
```

will log messages concerning startup at trace level, AQL queries at trace level
and everything else at info level.

In a configuration file, it is written like this:

```
[log]
level = startup=trace
level = queries=trace
level = info
```

The available log levels are:

- `fatal`: only logs fatal errors
- `error`: only logs errors
- `warning`: only logs warnings and errors
- `info`: logs information messages, warnings and errors
- `debug`: logs debug and information messages, warnings and errors
- `trace`: logs trace, debug and information messages, warnings and errors

Note that levels `debug` and `trace` will be very verbose.

See [Log Levels](monitoring-log-levels.html) in the Monitoring chapter for a
detailed description of the different levels.

Some relevant log topics available in ArangoDB 3 are:

- `agency`: information about the Agency
- `collector`: information about the WAL collector's state
- `compactor`: information about the collection datafile compactor
- `datafiles`: datafile-related operations
- `mmap`: information about memory-mapping operations (including msync)
- `performance`: performance-related messages
- `queries`: executed AQL queries, slow queries
- `replication`: replication-related info
- `requests`: HTTP requests
- `startup`: information about server startup and shutdown
- `threads`: information about threads

See more [log levels](http/administration-and-monitoring.html#modify-and-return-the-current-server-log-level)

## Log outputs

The log option `--log.output <definition>` allows directing the global
or per-topic log output to different outputs. The output definition `<definition>`
can be one of

- `-` for stdin
- `+` for stderr
- `syslog://<syslog-facility>`
- `syslog://<syslog-facility>/<application-name>`
- `file://<relative-path>`

The option can be specified multiple times in order to configure the output
for different log topics. To set up a per-topic output configuration, use
`--log.output <topic>=<definition>`, e.g.

    queries=file://queries.txt

logs all queries to the file "queries.txt".

The old option `--log.file` is still available in 3.0 for convenience reasons. In
3.0 it is a shortcut for the more general option `--log.output file://filename`.

The old option `--log.requests-file` is still available in 3.0. It is now a shortcut
for the more general option `--log.output requests=file://...`.

Using `--log.output` also allows directing log output to different files based on 
topics. For example, to log all AQL queries to a file "queries.log" one can use the 
options:

```
--log.level queries=trace --log.output queries=file:///path/to/queries.log
```

To additionally log HTTP request to a file named "requests.log" add the options:

```
--log.level requests=info --log.output requests=file:///path/to/requests.log
```

If you specify `--log.file-mode octalvalue` then any newly created log
file will use "octalvalue" as file mode. Please note that the `umask`
value will be applied as well.

If you specify `--log.file-group name` then any newly created log file
will try to use "name" as group name. Please note that you have to be
a member of that group. Otherwise the group ownership will not be
changed. Please note that this option is only available under Linux
and Mac. It is not available under Windows.

## Forcing direct output

The option `--log.force-direct` can be used to disable logging in an extra
logging thread. If set to `true`, any log messages are immediately printed in the
thread that triggered the log message. This is non-optimal for performance but
can aid debugging. If set to `false`, log messages are handed off to an extra
logging thread, which asynchronously writes the log messages.

## Time format

The option `--log.time-format` controls the time format used in log output.
The possible values for this option are:

Format                  | Example                  | Description
:-----------------------|:------------------------ |:-----------
`timestamp`             | 1553766923000            | unix timestamps, in seconds
`timestamp-millis`      | 1553766923000.123        | unix timestamps, in seconds, with millisecond precision
`timestamp-micros`      | 1553766923000.123456     | unix timestamps, in seconds, with microsecond precision
`uptime`                | 987654                   | seconds since server start
`uptime-millis`         | 987654.123               | seconds since server start, with millisecond precision
`uptime-micros`         | 987654.123456            | seconds since server start, with microsecond precision
`utc-datestring`        | 2019-03-28T09:55:23Z     | UTC-based date and time in format YYYY-MM-DDTHH:MM:SSZ 
`utc-datestring-millis` | 2019-03-28T09:55:23.123Z | like `utc-datestring`, but with millisecond precision
`local-datestring`      | 2019-03-28T10:55:23      | local date and time in format YYYY-MM-DDTHH:MM:SS

## Escaping

`--log.escape value`

This option toggles the escaping of log output. 

If set to `true`, the following characters in the log output are escaped:

- the carriage return character (hex `0d`)
- the newline character (hex `0a`)
- the tabstop character (hex `09`)
- any other characters with an ordinal value less than hex `20`

If the option is set to `false`, no characters are escaped. Characters with
an ordinal value less than hex `20` will not be printed in this mode but will
be replaced with a space character (hex `20`).

A side effect of turning off the escaping is that it will reduce the CPU 
overhead for the logging. However, this will only be noticeable when logging
is set to a very verbose level (e.g. debug or trace).

The default value for this option is `true`.

## Color logging

`--log.color value`

Logging to terminal output is by default colored. Colorful logging can be 
turned off by setting the value to false.

## Source file and Line number

Log line number: `--log.line-number`

Normally, if an human readable fatal, error, warning or info message is
logged, no information about the file and line number is provided. The
file and line number is only logged for debug and trace message. This option
can be use to always log these pieces of information.

## Prefix

Log prefix: `--log.prefix`

This option specifies a prefix for log messages.

Example: `arangod ... --log.prefix "-->"`

```
2020-07-23T09:46:03Z --> [17493] INFO ...
```

## Process ID, Thread ID and Name

Log Process identifier: `--log.process` (introduced in 3.8.0)

Log thread identifier: `--log.thread`

Log thread name: `--log.thread-name`

When log output is generated, the process ID is emitted as part of the log
information by default. This can be turned off by adjusting the `--log.process`
option.

The thread ID is not emitted by default, but it can be enabled by setting the
option `--log.thread`.

To also log thread names, it is possible to set the `--log.thread-name`
option. By default `--log.thread-name` is set to `false`.

Here is an example that only contains the process ID (19355 in this case):

```
2010-09-20T13:04:01Z [19355] ... ready for business
```

And here is an example that also contains the thread ID in addition:

```
2010-09-20T13:04:17Z [19371-18446744072487317056] ... ready for business
```

And another example with process and thread identifier logging disabled,
but thread name logging turned on:

```
2010-09-20T13:04:29Z [main] ... ready for business
```

## IDs

Log IDs: `--log.ids true`

Since ArangoDB 3.5, each log invocation in the ArangoDB source code contains
a unique log ID, which can be used to quickly find the location in the source
code that produced a specific log message. These log IDs are shown by
default, unless the option `--log.ids` is set to `false`.

Log IDs are printed as 5-digit hexadecimal identifiers in square brackets
between the log level and the log topic, e.g.

```
2020-06-22T21:16:48Z [39028] INFO [144fe] {general} using storage engine 'rocksdb'
```
(where `144fe` is the log ID).

## Role

Log role: `--log.role`

When set to `true`, this option will make the ArangoDB logger print a single
character with the server's role into each logged message. The roles are:

- U: Undefined / unclear (used at startup)
- S: Single server
- C: Coordinator
- P: Primary / DB-Server
- A: Agent

The default value for this option is `false`, so no roles will be logged.

## JSON log output

<small>Introduced in: 3.8.0</small>

Toggle JSON log output: `--log.use-json-format`

This option can be used to switch log output to JSON format.
Each log message then produces a separate line with JSON-encoded log data,
which can be consumed by applications.

The attributes produced for each log message JSON object are:

| Key        | Value      |
|:-----------|:-----------|
| `time`     | date/time of log message, in format specified by `--log.time-format`
| `prefix`   | only emitted if `--log.prefix` is set
| `pid`      | process id, only emitted if `--log.process` is set
| `tid`      | thread id, only emitted if `--log.thread` is set
| `thread`   | thread name, only emitted if `--log.thread-name` is set
| `role`     | server role (1 character), only emitted if `--log.role` is set
| `level`    | log level (e.g. `"WARN"`, `"INFO"`)
| `file`     | source file name of log message, only emitted if `--log.file-name` is set
| `line`     | source file line of log message, only emitted if `--log.file-name` is set 
| `function` | source file function name, only emitted if `--log.file-name` is set
| `topic`    | log topic name
| `id`       | log id (5 digit hexadecimal string), only emitted if `--log.ids` is set
| `message`  | the actual log message payload

### Log API Access

<small>Introduced in: 3.4.11, 3.5.6, 3.6.5, 3.7.1</small>

`/_admin/log` control: `--log.api-enabled`

Credentials data is not written to log files. Nevertheless, some logged
data might be sensitive depending on the context of the deployment. For
example, if request logging is switched on, user requests and
corresponding data might end up in log files.
Therefore, a certain care with log files is recommended.

Since the database server offers an API to control logging and query
logging data, this API has to be secured properly. By default, the API
is accessible for admin users (administrative access to the `_system`
database). However, one can lock this down further.

The possible values for this option are:

 - `true`: The API `/_admin/log` is accessible for admin users.
 - `jwt`: The API `/_admin/log` is accessible only for the superuser
   (authentication with JWT token and empty username).
 - `false`: The API `/_admin/log` is not accessible at all.

The default value is `true`.

## Logging to memory buffers

<small>Introduced in: 3.8.0</small>

Log to memory: `--log.in-memory`

This option can be used to toggle storing log messages in memory, from which
they can be consumed via the `/_admin/log` HTTP API and via the Web UI.
By default, this option is turned on, so log messages are consumable via the
API and UI. Turning this option off will disable that functionality, save a
tiny bit of memory for the in-memory log buffers and prevent potential log
information leakage via these means.