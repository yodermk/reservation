# Schedule

Determines days of the week this transportation line operates, as well as stops and times.

## Properties

### days

Array of days of the week, and whether we operate on that day.

### segments

List of pairs of (Departure Port code, Departure Time, Arrival Port code, Arrival Time).

### dst_anchor

If ports have different daylight savings time regimes, which one defines
the schedule during changes?  Index into the `segments` array with
zero being the first departure port and n (length of array) being the
final arrival port.

