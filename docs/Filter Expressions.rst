Filter Expressions
==================

Filter expressions can be included in overloads for *GetRangeValues( )*
and *GetWindowValues( )*. The filter expression is applied to events
returned such that the user can conditionally filter out certain events.

QiTypeCodes
------------

**Supported**

The following types are supported for use within a filter expression:

-  Enum
-  Boolean
-  Byte
-  Guid
-  DateTime
-  TimeSpan
-  DateTimeOffset
-  Decimal
-  Double
-  Single
-  ByteArray
-  Long (Int64)
-  Int (Int32)
-  Short (Int16)
-  UInt (UInt32)
-  ULong (Uint64)
-  UShort (Uint16)

**Not Supported**

The following types are not supported for use within a filter
expression:

-  Array
-  IEnumerable
-  IDictionary
-  IList
-  DateTimeOffset
-  Guid
-  NullableGuid
-  QiType
-  QiTypeProperty
-  NullableDateTime
-  TimeSpan

Logical operators
------------

**Supported**

The following logical operators are supported for use within a filter
expression:

+------------+-----------------------------------------------------+
| Operator   | Comment                                             |
+============+=====================================================+
| eq         | Equal to                                            |
+------------+-----------------------------------------------------+
| ne         | Not equal                                           |
+------------+-----------------------------------------------------+
| ge         | Gerater than or equal to                            |
+------------+-----------------------------------------------------+
| le         | Less than or equal to                               |
+------------+-----------------------------------------------------+
| lt         | Less than                                           |
+------------+-----------------------------------------------------+
| gt         | Greater than                                        |
+------------+-----------------------------------------------------+
| ( )        | Parenthesis can be used to effect operation order   |
+------------+-----------------------------------------------------+
| or         | Or logical operator                                 |
+------------+-----------------------------------------------------+
| and        | And logical operator                                |
+------------+-----------------------------------------------------+
| not        | Not logical operator                                |
+------------+-----------------------------------------------------+
| -          | Negation                                            |
+------------+-----------------------------------------------------+

**Logical Operator Examples**

| These examples assume that the event Qi Type includes a field named
  ‘Value’ of type double: - "Value eq 1.0"
| - "Value ne 15.6" - "Value ge 5.0" - "Value le 8.0" - "Value gt 5.0" -
  "Value lt 4.0" - "Value gt 2.0 and Value lt 9.0" - "Value gt 6.0 or
  Value lt 2.0" - "not (Value eq 1.0)"

Math functions
------------

**Supported**

The following math functions are supported for use within a filter
expression:

+-----------+---------+
| function  | Comment |
+===========+=========+
| add       | Additio |
|           | n       |
+-----------+---------+
| sub       | Subtrac |
|           | tion    |
+-----------+---------+
| mul       | Multipl |
|           | ication |
+-----------+---------+
| div       | Divisio |
|           | n       |
+-----------+---------+
| mod       | Modulo  |
+-----------+---------+
| round     | Rounds  |
|           | to      |
|           | nearest |
|           | numeric |
|           | compone |
|           | nt      |
|           | without |
|           | a       |
|           | decimal |
|           | with    |
|           | midpoin |
|           | t       |
|           | rounded |
|           | away    |
|           | from 0. |
|           | (e.g.   |
|           | 0.5     |
|           | rounds  |
|           | to 1,   |
|           | -0.5    |
|           | rounds  |
|           | to -1)  |
+-----------+---------+
| floor     | Rounds  |
|           | down to |
|           | nearest |
|           | numeric |
|           | compone |
|           | nt      |
|           | without |
|           | a       |
|           | decimal |
+-----------+---------+
| ceiling   | Rounds  |
|           | up to   |
|           | nearest |
|           | numeric |
|           | compone |
|           | nt      |
|           | without |
|           | a       |
|           | decimal |
+-----------+---------+

**Math Function Examples**

These examples assume that the event Qi Type includes a field named
‘Value’ of type double. - "Value add 3.0 gt 5.0" - "Value sub 5.0 lt
4.0" - "Value mul 2.0 lt 9.0" - "Value div 2.0 eq 3.0" - "Value mod 7.0
eq 0.0" - "Value add -3.0 gt 5.0" - "round(Value) eq 16" - "floor(Value)
eq 15" - "ceiling(Value) eq 16"

String functions
------------

**Supported**

String operations are case sensitive. Character index in a string is
0-based. The following string functions are supported for use within a
filter expression:

+---------------+-----------------------------------------------------------------+
| function      | Comment                                                         |
+===============+=================================================================+
| endswith      | Compare character at end of input string                        |
+---------------+-----------------------------------------------------------------+
| startwith     | Compare character at start of input string                      |
+---------------+-----------------------------------------------------------------+
| length        | Looks at string length                                          |
+---------------+-----------------------------------------------------------------+
| indexof       | Looks at character starting at given index                      |
+---------------+-----------------------------------------------------------------+
| substring     | Look at characters within another string at specific location   |
+---------------+-----------------------------------------------------------------+
| substringof   | Look for characters anywhere in another string                  |
+---------------+-----------------------------------------------------------------+
| tolower       | Convert characters to lower case                                |
+---------------+-----------------------------------------------------------------+
| toupper       | Convert characters to upper case                                |
+---------------+-----------------------------------------------------------------+
| trim          | Remove whitespace from front and end of string                  |
+---------------+-----------------------------------------------------------------+
| concat        | Concatenate strings together                                    |
+---------------+-----------------------------------------------------------------+
| replace       | Replace one set of characters with another                      |
+---------------+-----------------------------------------------------------------+

**String function examples**

These examples assume that the event Qi Type includes a field named
‘sValue’ of type string:

+-----+-----+
| Exa | Res |
| mpl | ult |
| e   |     |
+=====+=====+
| end | tru |
| swi | e   |
| th( | if  |
| sVa | Val |
| lue | ue  |
| ,   | end |
| 'XY | s   |
| Z’) | wit |
|     | h   |
|     | the |
|     | cha |
|     | rac |
|     | ter |
|     | s   |
|     | ‘XY |
|     | Z’  |
+-----+-----+
| sta | tru |
| rts | e   |
| wit | if  |
| h(s | Val |
| Val | ue  |
| ue, | sta |
| 'Va | rts |
| l') | wit |
|     | h   |
|     | the |
|     | cha |
|     | rac |
|     | ter |
|     | s   |
|     | ‘Va |
|     | l’  |
+-----+-----+
| len | tru |
| gth | e   |
| (sV | of  |
| alu | len |
| e)  | gth |
| eq  | of  |
| 11  | str |
|     | ing |
|     | val |
|     | ue  |
+-----+-----+
| ind | tru |
| exo | e   |
| f(s | if  |
| Val | the |
| ue, | 5th |
| 'ab | and |
| ')  | 6th |
| eq  | cha |
| 4   | rac |
|     | ter |
|     | s   |
|     | are |
|     | ‘ab |
|     | ’   |
+-----+-----+
| sub | tru |
| str | e   |
| ing | ‘a  |
| (sV | b’  |
| alu | is  |
| e,  | fou |
| 10) | nd  |
| eq  | in  |
| 'a  | sVa |
| b'  | lue |
|     | at  |
|     | ind |
|     | ex  |
|     | 10  |
+-----+-----+
| sub | tru |
| str | e   |
| ing | if  |
| of( | cha |
| 'va | rac |
| l', | ter |
| Val | s   |
| ue) | ‘va |
|     | l’  |
|     | are |
|     | any |
|     | whe |
|     | re  |
|     | in  |
|     | sVa |
|     | lue |
+-----+-----+
| tol | cha |
| owe | nge |
| r(s | sVa |
| Val | lue |
| ue) | to  |
| eq  | low |
| 'va | er  |
| l5' | cas |
|     | e   |
|     | and |
|     | com |
|     | par |
|     | es  |
|     | to  |
|     | ‘va |
|     | l5’ |
+-----+-----+
| tou | cha |
| ppe | nge |
| r(s | sVa |
| Val | lue |
| ue) | to  |
| eq  | upp |
| 'AB | er  |
| C'  | cas |
|     | e   |
|     | and |
|     | com |
|     | par |
|     | es  |
|     | to  |
|     | ‘AB |
|     | C’  |
+-----+-----+
| tri | tri |
| m(s | m   |
| Val | whi |
| ue) | tes |
| eq  | pac |
| ‘va | e   |
| ll2 | fro |
| 2’  | m   |
|     | fro |
|     | nt  |
|     | and |
|     | end |
|     | of  |
|     | sVa |
|     | lue |
|     | and |
|     | com |
|     | par |
|     | e   |
|     | to  |
|     | ‘va |
|     | l22 |
|     | ’   |
+-----+-----+
| con | add |
| cat | cha |
| (sV | rac |
| alu | ter |
| e,' | s   |
| xyz | to  |
| ')  | sVa |
| eq  | lue |
| 'da | s   |
| taV | and |
| alu | com |
| e\_ | par |
| 7xy | e   |
| z'  | to  |
|     | ‘da |
|     | taV |
|     | alu |
|     | e\_ |
|     | 7xy |
|     | z’  |
+-----+-----+
| rep | rep |
| lac | lac |
| e(s | e   |
| Val | any |
| ue, | ‘L’ |
| 'L' | in  |
| ,'D | sVa |
| ')  | lue |
| eq  | wit |
| 'Do | h   |
| g1' | ‘D’ |
|     | and |
|     | com |
|     | par |
|     | e   |
|     | to  |
|     | ‘Do |
|     | g1’ |
+-----+-----+

DateTime functions
------------

**Supported**

The following DateTime functions are supported for use within a filter
expression:

+------------+----------------------------------+
| Function   | Comment                          |
+============+==================================+
| year       | Get year value from DateTime     |
+------------+----------------------------------+
| month      | Get month value from DateTime    |
+------------+----------------------------------+
| day        | Get day value from DateTime      |
+------------+----------------------------------+
| hour       | Get hour value from DateTime     |
+------------+----------------------------------+
| minute     | Get minute value from DateTime   |
+------------+----------------------------------+
| second     | Get second value from DateTime   |
+------------+----------------------------------+

**DateTime Function Examples**

These examples assume that the event Qi Type includes a field named
‘TimeId’ of type DateTime:

-  "year(TimeId) eq 2015"
-  "month(TimeId) eq 11"
-  "day(TimeId) eq 3"
-  "hour(TimeId) eq 1"
-  "minute(TimeId) eq 5"
-  "second(TimeId) eq 3"

TimeSpan functions
------------

**Supported**

The following TimeSpan functions are supported for use within a filter
expression:

+------------+----------------------------------+
| function   | Comment                          |
+============+==================================+
| years      | Get year value from TimeSpan     |
+------------+----------------------------------+
| days       | Get day value from TimeSpan      |
+------------+----------------------------------+
| hours      | Get hour value from TimeSpan     |
+------------+----------------------------------+
| minutes    | Get minute value from TimeSpan   |
+------------+----------------------------------+
| seconds    | Get second value from TimeSpan   |
+------------+----------------------------------+

**TimeSpan Function Examples**

These examples assume that the event Qi Type includes a field named
‘TimeSpanValue’ of type TimeSpan:

-  "years(TimeSpanValue) eq 1"
-  "days(TimeSpanValue) eq 22"
-  "hours(TimeSpanValue) eq 1"
-  "minutes(TimeSpanValue) eq 1"
-  "seconds(TimeSpanValue) eq 2"
