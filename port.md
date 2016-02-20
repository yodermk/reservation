# Port

Defines a transportation port where a passenger can embark or disembark.

## Properties

### name

User friendly name of this port

### code

Code by which this will be known to the system.

### tz

Timezone information which must include DST regime.

### local_ports

Array of other port codes in the system which can be reached by local transportation.
For example perhaps a ferry port and an airport are 5 miles apart, but we want to 
be able to offer customers connecting itineraries.  Each element in the array can
include payload text showing how the user can transit to the other port.

### description

User friendly description of the environs of this port.  What is to do nearby?
How to get there?  Etc.

### contact_info

Data structure with phone and address and email

### schedule

Array of days of the week with hour ranges (possibly more than one) each day.
