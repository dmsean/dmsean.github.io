---
layout: post
title:  "TimescaleDB"
date:   2020-04-01 14:34:07 +1000
categories: metrics timescale 
---
One of the things that I enjoy doing the most is collecting metrics. These
metrics come from a variety of places and serve a number of different
purposes depending on the metrics being logged. Some examples include:


* Home Automation sensors
* Docker containers
* Host metrics
* Body weight and related measurements
* Weight training sessions
 
Once upon a time, these lived inside a Dockerized instance of InfluxDB. Things
like Docker and Host metrics would be sent from Telegraf, whilst anything
else would be sent via HTTP using the InfluxDB line protocol. This worked well,
and if I had to do it all over again, I'd be more than happy to use that stack
again. It was light on disk usage, I liked the integration with TICK services
like Chronograf, and the built in HTTP line protocol was easy to integrate
into new services. The problem came when I wanted to do a slightly more complex
query.

InfluxDB has what they refer to as 'SQL-like' syntax, which as the name suggests
means that the syntax shares a lot of similarities with SQL. This doesn't mean
that it implements _all_ of SQL's features, merely a small subset. On the surface,
and certainly up to a certain point in what I was looking to achieve, this
worked pretty well. But I started to bump into scenarios where I was looking to
gather metrics across different 'measurements', into somewhat of a new
super-metric. So I started to look into Flux, which at the time I was looking,
in an experimental phase, but provided some more complex functionality. 

In my eyes, Flux had a few flaws. Firstly, it was ugly. It was completely
unfamiliar to me, which in itself was not an issue, but it didn't seem to be
very intuitive to write. This alone was not enough to rule it out. Secondly,
it seemed to perform worse than using the standard InfluxDB query language.
Anecdotally of course, but I couldn't justify adding performance overhead this
early in my journey to metric enlightenment. Regardless, I was sensible enough
not to rule it for just this fact alone. But in my quest to find a solution
to my 'complex' problem, these two issues lead me to see if there were any
viable alternatives out there. Enter TimescaleDB.

Finding TimescaleDB lead me to a new, third problem with Flux; that it wasn't SQL.
Back then, I wasn't great at SQL, so I wasn't looking to migrate to SQL in
order to make my life easier. I had also rewritten my queries in Flux, so I
wasn't making a completely uninformed decision. But I had read a couple of
blog posts by the
[Influx](https://www.influxdata.com/blog/why-were-building-flux-a-new-data-scripting-and-query-language/)
and [Timescale](https://blog.timescale.com/blog/sql-vs-flux-influxdb-query-language-time-series-database-290977a01a8a/)
teams, which solidified the decision I was going to make. The fact that I had
to either learn Flux or learn SQL made choosing SQL a no-brainer. Both would
solve my problem, but learning Flux would mean absolutely nothing to the rest
of my Software Developer life. It simply wasn't going to transfer over to
anything else. It wouldn't help me at the job I was at, and it wasn't going to
be a skill that I could list on my resume. So that was it. I replaced InfluxDB
with TimescaleDB. 

Since the transition to TimescaleDB, things have been going smoothly. The only
issue that I have is that disk usage is higher than with InfluxDB, but that's
mostly a non-issue. The pros far outweigh the cons. The biggest pros being that
I've solved the issues of more complex queries, whilst learning a lot about SQL
in the process. I used to miss the HTTP line protocol from InfluxDB, but that
wasn't anything that either Postgrest or a custom listener couldn't handle. I
opted for the custom listener route, which I'll detail in a future post.
