Filter text can be included in overloads for the GetRangeValues or GetWindowValues Qi Library methods. This filter is applied to the events that are found by the call, such that the user can effect which events are returned (i.e. conditionally filter out certain events).   

## Supported QiTypeCodes

Fields of the following types can be used within Filter Text.

-Enum
-Boolean
-Byte
-Guid
-DateTime
-TimeSpan
-DateTimeOffset
-Decimal
-Double
-Single
-ByteArray
-Long	(Int64)
-Int	(Int32)
-Short	(Int16)
-UInt	(UInt32)
-ULong	(Uint64)
-UShort	(Uint16)

## Non-supported QiTypeCodes
Arrays, IEnumerable, IDictionary, IList, DateTimeOffset, Guid, NullableGuid, QiType, QiTypeProperty
NullableDateTime, TimeSpan

## Supported logical operators

eq, ne, ge, le, lt. gt, not, (, ), or, and, and also negation (i.e. ‘-‘), 

| operator | Comment |
| -------- | ------- |
| eq | Equal to |
| ne | Not equal |
| ge | Gerater than or equal to |
| le | Less than or equal to |
| lt | Less than |
| gt | Greater than |
| ( ) | Parenthesis can be used to effect operation order |
| or | Or logical operator |
| and | And logical operator |
| not | Not logical operator |

### Logical Operator Examples

These examples assume that the event Qi Type includes a field named ‘Value’ of type double. 
•	"Value eq 1.0"	
•	"Value ne 15.6"
•	"Value ge 5.0"
•	"Value le 8.0"
•	"Value gt 5.0"
•	"Value lt 4.0"
•	"Value gt 2.0 and Value lt 9.0"
•	"Value gt 6.0 or Value lt 2.0"
•	"not (Value eq 1.0)"

## Math functions
add, sub, mul, div, mod, round, floor, ceiling

| function | Comment |
| -------- | ------- |
| add | addition |
| sub | subtract |
| mul | Multiply |
| div | Division |
| mod | Modulo |
| round | Rounds to nearest numeric component without a decimal with midpoint rounded away from 0.  (e.g. 0.5 rounds to 1, -0.5 rounds to -1) |
| floor | Rounds down to nearest numeric component without a decimal. |
| ceiling | Rounds up to nearest numeric component without a decimal. |

### Math Function Examples

These examples assume that the event Qi Type includes a field named ‘Value’ of type double. 
•	"Value add 3.0 gt 5.0"
•	"Value sub 5.0 lt 4.0"
•	"Value mul 2.0 lt 9.0"
•	"Value div 2.0 eq 3.0"
•	"Value mod 7.0 eq 0.0"
•	"Value add -3.0 gt 5.0"
•	"round(Value) eq 16"
•	"floor(Value) eq 15"
•	"ceiling(Value) eq 16"

## String functions:
endswith, startswith, length, indexof, substring, substringof,  tolower, toupper, trim, concat , replace

String operations are case sensitive.  Character index in a string is 0-based.

| function | Comment |
| endswith | Compare character at end of input string  |
| startwith | Compare character at start of input string |
| length | Looks at string length |
| indexof | Looks at character starting at given index |
| substring | Look at characters within another string at specific location |
| substringof | Look for characters anywhere in another string |
| tolower | Convert characters to lower case |
| toupper | Convert characters to upper case |
| trim | Remove whitespace from front and end of string |
| concat | Concatenate strings together |
| replace | Replace one set of characters with another |

###String function examples

These examples assume that the event Qi Type includes a field named ‘sValue’ of type string.

•	"endswith(sValue, 'XYZ’)" 	 –true if Value ends with the characters ‘XYZ’
•	"startswith(sValue, 'Val')" –true if Value starts with the characters ‘Val’
•	"length(sValue) eq 11"	 -true of length of string value
•	"indexof(sValue, 'ab') eq 4"  -true if the 5th and 6th characters are ‘ab’
•	"substring(sValue, 10) eq 'a b'" –true ‘a b’ is found in sValue at index 10
•	"substringof('val', Value)"	     -true if characters ‘val’ are anywhere in sValue
•	"tolower(sValue) eq 'val5'" – Change sValue to lower case and compares to ‘val5’
•	"toupper(sValue) eq 'ABC'"	– Change sValue to upper case and compares to ‘ABC’
•	"trim(sValue) eq ‘vall22’" – Trim whitespace from front and end of sValue and compare to ‘val22’
•	"concat(sValue,'xyz') eq 'dataValue_7xyz' add characters to sValues and compare to ‘dataValue_7xyz’
•	“replace(sValue,'L','D') eq 'Dog1'"; - replace any ‘L’ in sValue with ‘D’ and compare to ‘Dog1’

## DateTime Functions
year, month, day, hour, minute, second 

| function | Comment |
| -------- | ------- |
| year | Get year value from DateTime |
| month | Get month value from DateTime |
| day | Get day value from DateTime |
| hour | Get hour value from DateTime  |
| minute | Get minute value from DateTime |
| second | Get second value from DateTime |

###DateTime Function Examples

These examples assume that the event Qi Type includes a field named ‘TimeId’ of type DateTime.

•	"year(TimeId) eq 2015"
•	"month(TimeId) eq 11"
•	"day(TimeId) eq 3"
•	"hour(TimeId) eq 1"
•	"minute(TimeId) eq 5"
•	"second(TimeId) eq 3"

##TimeSpan Functions
years, days, hours, minutes, seconds

| function | Comment |
| -------- | ------- |
| years | Get year value from TimeSpan |
| days | Get day value from TimeSpan |
| hours | Get hour value from TimeSpan |
| minutes | Get minute value from TimeSpan |
| seconds | Get second value from TimeSpan |

###TimeSpan Function Examples

These examples assume that the event Qi Type includes a field named ‘TimeSpanValue’ of type 
TimeSpan.

•	"years(TimeSpanValue) eq 1"
•	"days(TimeSpanValue) eq 22"
•	"hours(TimeSpanValue) eq 1"
•	"minutes(TimeSpanValue) eq 1"
•	"seconds(TimeSpanValue) eq 2"

## OData functions not supported.
contains, fractionalseconds, has, contains, date, time, totaloffsetminutes, now, maxdatetime, mindatetime, totalseconds, $it,  $root, $expand, $select, $orderby, $skip, $top, $count, $search, $format, any, all, isof, cast, geo.distance, geo.intersects, geo.length
