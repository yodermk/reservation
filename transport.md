# Transport

Represents one route (flight, ferry, bus line) that has the same schedule every run.

## Properties

### number

Route number (Flight #285 or whatever)

### schedules

Maps date ranges (which can be indefinite) to a schedule.
This allows us to change the schedule effective some date in the future,
or have a special holiday schedule or whatever.

