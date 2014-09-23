# timely [![Build Status](https://secure.travis-ci.org/sackio/timely.png?branch=master)](http://travis-ci.org/sackio/timely)

A simple module for schedules and calendars in Javascript

## Getting Started
Install the module with: `npm install timely`

```javascript
var Timely = require('timely')
  , cal = new Timely();
```

## Overview
Timely is a simple way to represent basic chronological events in Javascript. It is useful for storing scheduling information in a Javascript object, while providing validation and utility methods to adhere to scheduling rules.

Each instances of timely is an object consisting of properties `events`, `settings`, and `schedules`

`events` is an array of objects representing events. Each object is an event with the following keys:

* **uuid** - unique identifier of event (set automatically)
* **name** - name of the event (`String`, required)
* **description** - description of the event (anything, optional)
* **start** - `Date` (and time) of start (required)
* **stop** - `Date` (and time) of stop (required)
* **schedules** - `Array` of the schedules this event includes (elements are `String`)
* **avoid_events** - `Array` of events that MUST NOT overlap with this event (elements can be `RegExp`, used to match against event names, or `String` used to match event names or uuids)
* **require_events** - `Array` of events that MUST overlap with this event (elements can be `RegExp`, used to match against event names, or `String` used to match event names or uuids)
* **avoid_schedules** - `Array` of schedules that MUST NOT have events that overlap with this event (elements can be `RegExp`, used to match against schedule names, or `String` used to match schedule names or uuids)
* **require_schedules** - `Array` of schedules that MUST have events that overlap with this event (elements can be `RegExp`, used to match against schedule names, or `String` used to match schedule names or uuids)
* **precede_events** - `Array` of events that MUST follow this event (elements are `Object` with keys `event` - `String` or `RegExp` of the event, and key `required` - `Boolean`, if `true` event must actually follow event, if `false` event must only follow event if it occurs)

`schedules` is an array of the schedules included in events. Schedules represent categories of events, which might further represent personnel schedules, resource calendars, a series of events, etc. Each schedule is an `Object` which includes the following keys:

* **name** - name for schedule - must be unique
* **description** - additional information / metadata for the schedule

In addition, the schedule named `*` is reserved to represent the schedule of all events. It serves as a way to reference all events as a collective group.

`settings` is an object of options for the instances. It can include:

* **start** - `Date` (and time) of calendar start. No events may precede this if set.
* **stop** - `Date` (and time) of calendar stop. No events may follow this if set.

## Methods
Timely provides methods to manage and validate the schedules and events it represents

* **addEvent(event, options)** - Add an event. If times are omitted, method tries to find a suitable time that fits a `duration` option. If no suitable time can be found, an error is returned. If the event already exists (i.e. a passed `uuid` matches a current event) the existing event is updated and validated. If invalid and `flexible` is passed as an option, a suitable time is sought. If it is found, the event is moved, it not an error is returned.
* **removeEvent(event, options)** - Removes an event from the schedule. Optionally scoped by schedule
* **validate()** - Validates all events
* **validateSchedules(schedules)** - Validate all events in passed schedules
* **validateEvents(events)** - Validate a specific event
* **findTime(event, options)** - Find a valid time for an event
* **getSchedule(schedules, options)** - Return events for a schedule or group of schedules. Optionally scoped by `start` and `stop` and `union` (return group of events shared by all passed schedules)

## License
Copyright (c) 2014 Ben Sack
Licensed under the MIT license.
