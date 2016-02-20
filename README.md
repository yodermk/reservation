This is an un-announced, in-progress braindump. I will start talking about it when it's more or less complete. :-)
It will be open source, but for now consider it (C)2016 Micah Yoder, with a licence grant allowing you to read it
and think about it. :-)  It does not yet even have a formal name.

# The Open Reservations System

## What the heck is this?

Think of the great reservation systems run by large airlines and hotels.  They are closed and highly
proprietary.  As open source and other open technologies continue to gain inroads into more and more
applications, it seems that this is the right time for the forces of openness to disrupt the big
reservations systems.  This system should be pretty general, supporting:

* Airlines
* Ferries
* Trains
* Busses
* Hotels
* Rental cars
* Tours
* Special events
* Conference sessions
* etc

This will support atomically booking multiple items, such as multiple flights that make an itinerary,
or a certain set of sessions at a conference.  For travel, it will support routing.  For example you want
to fly from Portland to San Antonio.  Our system will note that you must book a flight from Portland
to Denver, and another from Denver to San Antonio.  It will make checks for appropriate connections.

On the backend, this system should work out of the box with a number of open source "big data"
solutions to minimize the work we need to do, and to produce amazing reports and such for
those who need them.

On the front-end, we should have a Javascript-based "single-page" application that allows exploration
of options in a truly user-friendly way.  Even the best "big name" reservation systems leave a lot to
be desired, and small-name systems are often pretty terrible.  With this, I dream of even the smallest
operator having a reservation system that puts the big guys to shame in terms of usability.  And they
get it for free!

## Federated!

It should be possible for any two or more providers in this system to connect their systems together to
allow booking "package deals".  For example a resort might want to combine forces with a regional
airline to offer package deals.  Or simply two airlines, or an airline and a ferry company, offering
connections to each others' routes.  Customers are usually a lot more confident if they can buy whole
itineraries on a single ticket.  Providing a component that would allow federated access to legacy
reservations systems would seem reasonable.

## Parts of the system

`reservable` - The interface to one of anything that can be reserved.  That could include a specific flight
on a specific day, a hotel for a specific night.  Note that this is the entire "thing" - flight or hotel - not an
individual seat or room.  Those are accounted for within this class.  It will support the ability to overbook
a resource (accounting for no-shows) and different classes of service.

A `transport` represents a transportation route with schedules determined by one or more `schedule`
instances.

`port` - represents a physical port served by a `transport` line. Contains info such as location and timezone
including Daylight Savings Time regime.

`arrangement` - basically a map of "seats" (for a transport line) or rooms for a hotel.  Maps an SVG graphic
to bookable items of various classes.

## Architecture

These various parts would basically all be remote-procedure-call driven microservices.  They would be as decoupled
as possible, in complying with best practices for cloud applications.

Every night a job would run to create `reservable`s for a given date x days in the future (if booking is allowed, say,
360 days ahead).

When a reservation comes in via an end user endpoint (or via the web portal), it would send a message through a
message queue (probably Kafka) and try to acquire the necessary individual resources.  If any part failed, the parts
that were acquired could be released.

Kafka could allow various subsystems to keep their own view of the system and support other things like 
reporting and analytics.

## Status

In design phase and looking to see who might be interested!  No code written at this point.

## License

To be decided when we start coding.  I (Micah) am open to almost any open source license except the AGPL
(don't want to make things *too* restrictive).  I'd probably lean toward the GPL v3 if I were choosing, but
there may be good arguments for a BSD license, which I'm not opposed to.  If we go GPL, we may want
to make some parts LGPL.  We should insist though that the use of any interface over a network socket does
*not* constitute linking and would not compel such code to fall under copyleft.

## Languages

Also to be decided.  Python would seem the most natural for getting a minimum viable product out quickly.
(And for crying out loud use Python 3 - don't start another new project with the old version!!! Not even "dual"
support -- let's just fully take advantage of Python 3 and the great stuff it offers!)

There's lots of room for other languages though.  Go?  C++?  Scala?  Where they make sense, by all means use them.

Sadly, Javascript will be necessary on the front-end.

## Data Storage

We'll utilize a variety of data backends.  We should use PostgreSQL for basic information, but I can see NoSQL being used
for specific parts. Perhaps Neo4j (a graph database) for transportation routing. Perhaps Cassandra for reservations.
Perhaps ElasticSearch for textual information and any stats where a Kibana dashboard would be beneficial. Probably plenty
of uses for a Redis cache.

Not for long term storage, but a pub/sub pipeline like Kafka should be used to tie things together.

## Deploying

This should be a very easy, turnkey, cloud-based deploy.  There should be an Ansible playbook to create the needed
servers and set up everything to just work.

Most likely it would be built on CentOS 7.x based servers.  Since our scripts will likely manage it all, is there even
a reason to give the user a choice here?  This also has the advantage of not having to worry about differences in
distributions or whether some dependency might be met.

Or maybe a container-based deploy?  To be decided ...

## Who is behind this?

Micah Yoder, for now.  But I can't do it by myself -- not even close.  My initial objective is to get others thinking
about it.  Then we'll get your name in here too!

Pull requests on these `.md` files are encouraged.  I'm sure there's a lot that I'm missing.

If you just want to talk about it, my email is: `yoderm AT gmail DOT com`

Thanks for looking!
