QiStream Behavior
==================

The ``QiStreamBehavior`` object determines how data-read operations are performed when an index to be read falls between, before, or after stream data in the stream. For example, for an index that falls between existing data events, you might want an interpolated value returned or you might instead want the value from the preceding event returned. Similarly, if the read index occurs before or after all of the stream's data, the stream behavior determines whether extrapolation is applied. A ``QiStreamBehavior`` object is defined and included in the definition of a stream (similar to the way a QiType is used with a QiStream).
Note that, if you do not assign a specific Stream Behavior object to a stream, the default behavior is assumed.

QiStreamBehavior management via the Qi Client Libraries is performed through the ``IQiMetadataService`` interface, which may be accessed via the ``QiService.GetMetadataService( )`` helper.

The Stream Behavior object consists of the following properties:

==================        ==================
Id                        A string that is used to identify the behavior
Name                      A string that you use to name the behavior
Mode                      The behavior’s interpolation setting (type = QiStreamMode) 
ExtrapolationMode         The behavior’s extrapolation setting (type = QiStreamExtrapolation) 
Overrides                 A list of overrides to the Mode setting when individual properties have
                          different interpolation behaviors
==================        ==================

+------------------+--------------------------------+--------------------------------------------------+
|Property          |Type                            |Details                                           |
+==================+================================+==================================================+
|Id                |String                          |Unique identifier used to reference this behavior |
+------------------+--------------------------------+--------------------------------------------------+
|Name              |String                          |Optional descriptor                               |
+------------------+--------------------------------+--------------------------------------------------+
|Mode              |QiStreamMode                    |Interpolation behavior setting that applies to all|       |
|                  |                                |value properties (unless overridden by Override   |
|                  |                                |list)                                             |
+------------------+--------------------------------+--------------------------------------------------+
|Overrides         |IList<QiStreamBehaviorOverride> |A list of QiStreamBehaviorOverride items that is  |
|                  |                                |used to set a different interpolation behavior    |
|                  |                                |than the mode to a type property                  |
+------------------+--------------------------------+--------------------------------------------------+
|ExtrapolationMode |QiStreamExtrapolation           |Controls extrapolation behavior for the stream    |
+------------------+--------------------------------+--------------------------------------------------+

Stream behavior objects are always referenced by the Id property. A
stream can be changed to use a different stream behavior or the stream
behavior itself can be changed after it is created or configured.


**Rules for ``QiStreamBehavior`` Id**

1. Not case sensitive
2. Spaces are allowed
3. Cannot start with two underscores ("\_\_")
4. Cannot contain forward slashes or backslashes ("/" or "\\")
5. Can contain a maximum of 260 characters

**Methods affected by QiStreamBehavior**

`GetValue <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html#getvalue>`__
  Stream behavior is applied when the index is between, before, or after all data.

`GetValues <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html#getvalues>`__
  Stream behavior is applied when an index determined by the call is between, before, or after all data.

`GetWindowValues <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html#getwindowvalues>`__
  Stream behavior is applied to indexes between, before, or after data when the calls Boundary parameter is set to ExactOrCalculated.

`GetRangeValues <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html#getrangevalues>`__
  Stream behavior is applied to indexes between, before, or after data when the calls Boundary parameter is set to ExactOrCalculated.




The code in the following example shows how to define and create a simple
``QiStreamBehavior``:

::

    String behaviorid = “myFirstBehavior”;
    QiStreamBehavior simpleBehavior = new QiStreamBehavior()
    {
        Id = behaviorId,
        Mode = QiStreamMode.StepwiseContinuousLeading,
    };
    _metadataService.GetOrCreateBehavior(createdBehavior);

After the stream behavior is defined, the behavior can be applied to a
stream as shown in the next example:

::

    string streamId = "MyFirstStream";
    string streamType = "mySimpleType";
    QiStream stream1 = new QiStream()
    {
        Id = streamId,
        TypeId = streamType,
        BehaviorId = “MyFirstBehavior”
    }
    _metadataService.GetOrCreateStream(stream1);

Interpolation
------------

When read methods affected by QiStreamBehavior (as shown above) are
given an index that occurs between two values in a stream, the
**Mode** object determines which values are retrieved. The
table below shows how a stream behaves for the mode values
listed:

+---------------------------+--------------------------------+--------------------------------------------------+
|Mode                       |Enumeration value               |Operation                                         |
+===========================+================================+==================================================+
|Default                    |0                               |Continuous                                        |
+---------------------------+--------------------------------+--------------------------------------------------+
|Continuous                 |0                               |Interpolates the data using previous and next     |
|                           |                                |index values                                      |
+---------------------------+--------------------------------+--------------------------------------------------+
|StepwiseContinuousLeading  |1                               |Returns the data from the previous index          |
+---------------------------+--------------------------------+--------------------------------------------------+
|StepwiseContinuousTrailing |2                               |Returns the data from the next index              |
+---------------------------+--------------------------------+--------------------------------------------------+
|Discrete                   |3                               |Returns ‘null’                                    |
+---------------------------+--------------------------------+--------------------------------------------------+

When **Mode** is set to continuous (or left at the default value), calls to read the
value of the ``QiStreamBehavior`` return ``0=Default``. Stream behavior
can also be used to give different mode settings to different data
properties within the stream’s type using overrides. For example, using an override
allows for a **Discrete** mode setting for one property and a **Continuous**
mode setting for another.

When the Stream Behavior is set to **Continuous** or **Default**, read methods
attempt to return an interpolated value for indexes that occur between two
existing data events in a stream. This interpolation cannot always be performed, 
such as when the type is not numeric.

The table below describes how the **Continuous** or **Default** **Mode** affects
indexes that occur between data in a stream:

***Mode* = Continuous or Default**

+---------------------------+--------------------------------+--------------------------------------------------+
|Type                       |Result for an index between     |Comment                                           |
|                           |data in a stream                |                                                  |
+===========================+================================+==================================================+
|Numeric Types              |Interpolated*                   |Rounding is done as needed for integer types      |
+---------------------------+--------------------------------+--------------------------------------------------+
|Time related Types         |Interpolated                    |DateTime, DateTimeOffset, TimeSpan                |
+---------------------------+--------------------------------+--------------------------------------------------+
|Nullable Types             |Returns ‘null’                  |Cannot reliably interpolate due to possibility of |
|                           |                                |a null value                                      |
+---------------------------+--------------------------------+--------------------------------------------------+
|Array and List Types       |Returns ‘null’                  |                                                  |
+---------------------------+--------------------------------+--------------------------------------------------+
|String Type                |Returns ‘null’                  |                                                  |
+---------------------------+--------------------------------+--------------------------------------------------+
|Boolean Type               |Returns value of nearest index  |                                                  |
+---------------------------+--------------------------------+--------------------------------------------------+
|Enumeration Types          |Returns Enum value at 0         |This may have a value for the enumeration         |
+---------------------------+--------------------------------+--------------------------------------------------+
|GUID                       |                                |                                                  |
+---------------------------+--------------------------------+--------------------------------------------------+
|Version                    |Returns ‘null’                  |                                                  |
+---------------------------+--------------------------------+--------------------------------------------------+
|IDictionary or Ienumerable |Returns ‘null’                  |                                                  |
+---------------------------+--------------------------------+--------------------------------------------------+

\*When extreme values are involved in an interpolation (for example
Decimal.MaxValue) the call might result in a BadRequest exception if the
interpolation cannot complete successfully.

Extrapolation
------------

In addition to interpolation settings, stream behavior is also used to
define how the stream extrapolates data. ``ExtrapolationMode`` acts as
a master switch to determine whether extrapolation occurs and at
which end of the data. When defined, ``ExtrapolationMode`` works with the
**Mode** to determine how a stream responds to requests for an index
that precedes or follows all of the data in the stream.

The following tables show how ``ExtrapolationMode`` affects returned
values for each **Mode** value:

**ExtrapolationMode with Mode\ =Default or Continuous**

+---------------------+---------------------+----------------------------+---------------------------+
| ExtrapolationMode   | Enumeration value   | Index before data          | Index after data          |
+=====================+=====================+============================+===========================+
| All                 | 0                   | Returns first data value   | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+
| None                | 1                   | Return null                | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Forward             | 2                   | Returns first data value   | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Backward            | 3                   | Return null                | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+

***ExtrapolationMode* with *Mode*\ =Discrete**

+---------------------+---------------------+---------------------+--------------------+
| ExtrapolationMode   | Enumeration value   | Index before data   | Index after data   |
+=====================+=====================+=====================+====================+
| All                 | 0                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+
| None                | 1                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+
| Forward             | 2                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+
| Backward            | 3                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+

***ExtrapolationMode* with *Mode*\ =StepwiseContinuousLeading**

+---------------------+---------------------+----------------------------+---------------------------+
| ExtrapolationMode   | Enumeration value   | Index before data          | Index after data          |
+=====================+=====================+============================+===========================+
| All                 | 0                   | Returns first data value   | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+
| None                | 1                   | Return null                | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Forward             | 2                   | Returns first data value   | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Backward            | 3                   | Return null                | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+

***ExtrapolationMode* with *Mode*\ =StepwiseContinuousTrailing**

+---------------------+---------------------+----------------------------+---------------------------+
| ExtrapolationMode   | Enumeration value   | Index before data          | Index after data          |
+=====================+=====================+============================+===========================+
| All                 | 0                   | Returns first data value   | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+
| None                | 1                   | Return null                | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Forward             | 2                   | Returns first data value   | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Backward            | 3                   | Return null                | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+

For additonal information about the effect of stream behaviors, see the
documentation on the `read
method <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html>`__
you are using.

Overrides
------------

As described above, the interpolation behavior for the values in a
stream is determined by the stream behavior *Mode*; however, individual
data types can be overridden to conform to another behavior by setting
the *Overrides* property. In this way you can have different interpolation 
behaviors for different parameters within the stream data’s type. Without
the overrides, all properties inherit the interpolation behavior defined by
the *Mode* object of the stream behavior.

The *Override* object has the following structure:

::

    string QiTypePropertyId
    QiStreamMode Mode

Note that when using the override list the *Mode* setting of Discrete
cannot be overridden. If the *Mode* is set to Discrete a null value is
returned for the entire event. If a Discrete setting is desired for one
of the types within a stream and a different setting (for example,
StepwiseContinuousLeading) is desired for other properties within the
stream, set the *Mode* to StepwiseContinuousLeading and use the override
list to set the desired property to Discrete.
