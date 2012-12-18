# opening_hours.js #

[opening_hours](http://wiki.openstreetmap.org/wiki/Key:opening_hours) tag is used in [OpenStreetMap](http://openstreetmap.org) project to describe time ranges when a specific facility (for example, a cafe) is open. As it has pretty complex syntax which require special parsing and additional processing to extract some useful information (e.g. whether a facility is open at specific time, next time it's going to open/close, or a readable set of working hours), this library was written.

Examples of some complex real-life opening_hours values:

```
Mo,Tu,Th,Fr 12:00-18:00;Sa 12:00-17:00; Th[3] off; Th[-1] off
```

a library which works from 12:00 to 18:00 on workdays except Wednesday, and from 12:00 to 17:00 on Saturday. It also has breaks on third and last Thursday of each month.

```
00:00-24:00; Tu-Su 8:30-9:00 off; Tu-Su 14:00-14:30 off; Mo 08:00-13:00 off
```

around-the-clock shop with some breaks.

## Library API

```javascript
var oh = new opening_hours('Mo-Fr 12:00');

var now = new Date();

var state = 'The facility is now ' + oh.isOpen(now) ? 'open' : 'close';
var next = 'And that will change on' + oh.nextChange(now);
```

## Features

This library is on the early stage of development, so only the most basic opening_hours features are supported. Still, these should cover larger part of real-world data.

### General rules ###

* Supports multiple subsets of rules separated by semicolon ```Mo 10:00-20:00; Tu 12:00-18:00```
* Supports off keyword ```Mo-Fr 10:00-20:00; 12:00-14:00 off```
* Handles exception rules correctly, e.g ```Mo-Fr 10:00-16:00; We 12:00-18:00``` is processed according to wiki, e.g. rule for Wednesday override, not extend previous rule

### Time ranges ###

* Supports set of time ranges ```10:00-12:00,14:00-16:00```
* Supports 24/7 keyword
* Doesn't support ranges that wrap over midnight ```10:00-26:00```, ```10:00-02:00```
* Doesn't support open end
* Doesn't support ommitting time range ```Mo-Fr 10:00-20:00; Tu off```

### Weekday ranges ###

* Supports set of weekdays and weekday ranges ```Mo-We,Fr```
* Supports weekdays which wrap to the next weeh ```Fr-Mo```
* Doesn't support constrained weekdays ```Th[1]```, ```Fr[-1]```

### Month ranges ###

* Not supported

### Monthday ranges ###

* Not supported

### Week ranges ###

* Not supported

### Other ###

* Doesn't support sunrise/sunset keywords
* Doesn't support PH/SH keywords

## Test ##

Simple node.js based test framework is bundled. You can run it with ```node test.js``` or with make.

## Author ##

* [Dmitry Marakasov](https://github.com/rodneyrehm) <amdmi3@amdmi3.ru>

## License ##

* opening_hours.js is published under the New (2-clause) BSD license
