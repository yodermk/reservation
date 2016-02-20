# Reservable

An interface to a specific instance of something that can be reserved.
Examples: An airline flight on a specific date, a hotel for a specific night, 
an event at a venue on a specific date.  Note that, say, an airline's
Flight #501, would have an instance of this for each date that it runs.
Here we represent one complete instance of it.
It can optionally have assigned seats or different classes of service.

## Properties

These will be set when a specific instance of this is created.

### arrangement

An instance of `arrangement` to define seating (or hotel room) layout. It is
technically optional since you may have an event or something with no specific
seating, but you will certainly want this for flights or hotel rooms.

Without an `arrangement` there can only be one class; however you can still
put arbitrary data in the booking request that could indicate certain privileges
on entry.

### maximum_entry

Only if there is no `arrangement`, this is the maximum number of tickets we can
sell for this event, though we can overbook if enabled.

### assigned\_location\_on_book

Whether a specific assigned location (seat, room, etc) should be associated with
this record while booking. Only valid if there is an `arrangement`.

Allowed values: `no`, `always`, `to_percentage`

The value `always` is incompatible with overbooking.  If `to_percentage` is set,
you need to set the value in `assign_to_percentage`

Can also be a dictionary of class names to values, if this may be different per class.

### overbook

Boolean; whether this `Reservable` can be overbooked.  Must set `overbook_percentage`.

Can also be a dictionary of class names to values, if this may be different per class.

### assign\_to\_percentage

Only allow assigning location when booking until this percentage of the booking class
is assigned.

Can also be a dictionary of class names to values, if this may be different per class.

### overbook_percentage

Number greater than 100 which would indicate the factor to which we can overbook this
`Reservable`.

Can also be a dictionary of class names to values, if this may be different per class.

### booking_array

An array of booking IDs with class and number of items booked and SVG element names

## Methods

### acquire

Used to acquire an item for booking.  This is not quite the same as a booking from the user's
perspective, because the final booking may be contingent on other resources being available
or final payment being processed.  This resource can easily be released if things don't pan out.

Parameters:

*  `booking_id`: the transaction number
*  `class`: desired class of service
*  `qty`: quantity of items
*  `extra`: extra key-value dictionary with parameters that may make sense for some types
*  `port_from`, `port_to`: used on transportation routes serving more than two ports

Returns:
Boolean or code?

### get_availability

Returns the number of items available at a given class, or all classes

Parameters:

* `class`: optional class type
* `port_from`, `port_to`: used on transportation routes serving more than two ports

Returns:
Integer number of items available in class, or dictionary of classes and availability (all)

