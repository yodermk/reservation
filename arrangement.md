# Arrangement

Representing the physical layout of bookable entities, like the seat layout of a flight
or how chairs are arranged in a room.  Note that the same physical space can have
multiple arrangements.

This can also represent a map of a hotel and its rooms; obviously in this case the space
can have only one arrangement.

## Properties

### map_graphic

This provides an SVG file whose elements include one for each item (seat, room, etc) that
can be reserved.

### classes

An array of class names in this arrangement

### maps

A dictionary of class names each containing an array of elements in the SVG map which correspond
to bookable locations (seats, hotel rooms) in this class.

