The Stream Behavior is applied to a stream and effects how certain data read operations will be performed. The Stream Behavior must first be defined and then can be applied to the stream when it is created (GetOrCreateStream method) or updated (UpdateStream method). Stream Behavior is always referenced with its Id.

The Stream Behavior can be changed between reads to change how the read acts. The Stream could also be set to use a different Stream Behavior.

The default behavior for a stream (when a defined Stream Behavior is not applied to the stream) is Mode = ‘Continuous’ and Extrapolation = ‘All’. 

## Stream Behavior Object

```c#
QiStreamExtrapolation ExtrapolationMode
string Id
QiStreamMode Mode { get; set; }
string Name
IList<QiStreamBehaviorOverride> Overrides 
```
-`Id` -- unique identifier used to reference this behavior
-`Name` -- Optional descriptor.
- `Mode` -- behavior setting to be applied to all ‘value’ parameters in the Type of Stream to which this is applied
- `ExtrapolationMode` -- 

`QiStreamMode` is an enumeration whose permissible values are:

- Continuous (=0)
- StepwiseContinuousLeading (=1)
- StepwiseContinuousTrailing (=2)
- Discrete (=3)

`ExtrapolationMode` is an enumeration:
- All (=0)
- None (=1)
- Forward (=2)
- Backward (=3)


The `QiStreamBehaviorOverride` object has the following structure:
```c#
string QiTypePropertyId
QiStreamMode Mode
```

It is used to apply a behavior override to a specific property of an event, rather than all properties of the event.


## Stream Behavior Modes
When running a query method, if an index lands between 2 values in the stream, then the stream behavior is used to determine what is returned. The Stream Behavior can be set to one of these values: 

- Continuous:  value is interpolated using previous and next events (see Chart below for exceptions)
- StepwiseContinuousLeading: value is obtained from previous event. 
- StepwiseContinuousTrailing value is obtained from next event. 
- Discrete:  NULL value is returned

There are cases where ‘null’ cannot be used. For example when a GetValue call is done on a stream that has a behavior using a Continuous Mode and a element with a Discrete override it will attempt to set this Discrete element to ‘null’.  But in cases where this cannot be done (i.e. a non-nullable type) then the default value will be used. 

The chart below describes how the Types act when the Behavior is set to Continuous:

| Type	| When Behavior = Continuous and index between events is addressed |
| ------ | ---------------------------------------------------------------- |
| Numeric Floating Point Types Single, Double, Decimal | Interpolation |
| Numeric Integer Types Int16, int32, int64, uint16, uint32, uint64, byte, Sbyte, Char | Interpolation (rounding) |
| Time related Types DateTime, DateTimeOffset, TimeSpan	| Interpolation |
| Nullable Types NullableBoolean, NullableChar, NullableSByte, NullableByte, NullableInt16, NullableUInt16, NullableInt32, NullableUInt32, NullableInt64, NullableUInt64, NullableSingle, NullableDouble, NullableDecimal, NullableDateTime, NullableGuid, NullableDateTimeOffset, NullableTimeSpan | Returns null (= Discrete Behavior) |
| Array and List Types BooleanArray, CharArray, SByteArray,ByteArray, Int16Array, UInt16Array, Int32Array, UInt32Array, Int64Array, UInt64Array, SingleArray,DoubleArray, DecimalArray, DateTimeArray, StringArray, GuidArray, DateTimeOffsetArray, TimeSpanArray, VersionArray, IList | Returns null (= Discrete Behavior) |
| String | Returns null (= Discrete Behavior) |
| Boolean | Returns the value of nearest event |
| Enumeration Types SByteEnum, ByteEnum, Int16Enum, UInt16Enum, Int32Enum, UInt32Enum, Int64Enum,UInt64Enum | Returns ‘0’ which may be the value of a defined enumeration element. |
| Guid | Returns Guid.Empty   |
| QiType, QiTypeProperty | Returns null (= Discrete Behavior) |
| Version | Returns null (= Discrete Behavior) |
| IDictionary, IEnumerable | Null |

All values in the stream type will be ‘set’ to the Stream Behavior Mode. Continuous is the default if not set. Individual Type Values can be overridden to act as another behavior. In this way the user can have different values within the same event to have a different behavior.  Note that when doing this, the Main Behavior Mode is still used to determine whether an event is returned for an index between data. If the main Behavior Mode is set to ‘Discrete’ then no event is returned for the call, regardless of any overrides.

ExtrapolationMode
All:  extrapolation done at both start and end of data in stream. <DEFAULT>
Forward: extrapolation done at the end of the stream (not at the front). 
Backward: extrapolation done at the front of the stream (not at the end). 
None: no extrapolation done  

The ExtrapolationMode (stream behavior parameter) comes in to play for a stream in the following conditions:
-GetValue (and GetValues) when an index is used that is before or after all of the data in the stream
-GetWindowValues when the start index is before all event in the stream or when the end index  is after all events in the stream
-GetRangeValues when the ‘start index’ is before all the data (or after all the data)
-GetIntervals …on indexes on each side of an interval

| Behavior | Extrapolation | Before Start of Stream | After End of Stream | Empty Stream |
| -------- | ------------- | ---------------------- | ------------------- | ------------ |
| Continuous | All | First Event Fields | Last Event Fields | Null |
| Continuous | None | Null | Null | Null |
| Continuous | Backward | First Event Fields | Null | Null |
| Continuous | Forward | Null | Last Event Fields | Null |
| Discrete | All | Null | Null | Null |
| Discrete | None | Null | Null | Null |
| Discrete | Backward | Null | Null | Null |
| Discrete | Forward | Null | Null | Null |
| StepwiseContinuousLeading | All | Null | Last Event Fields | Null |
| StepwiseContinuousLeading | None | Null | Null | Null |
| StepwiseContinuousLeading | Backward | Null | Null | Null |
| StepwiseContinuousLeading | Forward | Null | Last Event Fields | Null |
| StepwiseContinuousTrailing | All | First Event Fields | Null | Null |
| StepwiseContinuousTrailing | None | Null | Null | Null |
| StepwiseContinuousTrailing | Backward | First Event Fields | Null | Null |
| StepwiseContinuousTrailing | Forward | Null | Null | Null |

## Naming Rules for Behavior Identifiers
1.	Case sensitive
2.	Allows spaces.
   
## Stream Behavior Methods

*DeleteBehavior*
```
void DeleteBehavior(string behaviorId);
Task DeleteBehaviorAsync(string behaviorId);
```

*REST*
```
Qi/Behaviors/{behaviorId}
```

HTTP DELETE

*Parameters*

`behaviorId` -- id of the behavior to delete; the behavior must not be associated with any streams

Deletes behavior from server.

*GetBehavior*
```
QiStreamBehavior GetBehavior(string behaviorId);
Task<QiStreamBehavior> GetBehaviorAsync(string behaviorId);
```
*REST*
```
Qi/Behaviors/{behaviorId}
```

HTTP GET

*Parameters*

`behaviorId` -- id of the behavior definition to retrieve

Gets a behavior object from server.

*GetBehaviors*
```
IEnumerable<QiStreamBehavior> GetBehaviors();
Task<IEnumerable<QiStreamBehavior>> GetBehaviorsAsync();
```

Returns IEnumerable of all behaviors for the tenant.

*GetOrCreateBehavior*
```
QiStreamBehavior GetOrCreateBehavior(QiStreamBehavior entity);
Task<QiStreamBehavior> GetOrCreateBehaviorAsync(QiStreamBehavior entity);
```

*REST*
```
Qi/Behaviors
```

HTTP POST
Body is serialized QiStreamBehavior entity

*Parameters*

`entity` -- a QiStream object to add to the Qi Service for the current tenant.  
Creates a StreamBehavior (or returns it if it already exists). 

If `entity` already exists on the server by `Id`, that existing behavior is returned to the caller unchanged.

*UpdateBehavior*
```
void UpdateBehavior(string behaviorId, QiStreamBehavior entity);
Task UpdateBehaviorAsync(string behaviorId, QiStreamBehavior entity);
```
*REST*
```
Qi/Behaviors/{behaviorId}
```

HTTP PUT
Body is serialized QiStreamBehavior (updated)

*Parameters*

`behaviorId` -- identifier of the stream behavior to update
`entity` -- updated stream behavior 

Permitted changes: 

•	Override list
•	BehaviorMode
•	ExtrapolationMode
•	Name

An override list can be added to add, remove or change the override Mode on parameters within the type. 
UpdateBehavior replaces the stream’s existing behavior with entity.  If certain aspects of the existing behavior are meant to remain, they must be included in entity.

This is a list of parameters from the `Type` (`QiTypePropertyId`) that are to be given different behaviors (`Mode`).  Each parameter.

The overrides list is used in cases where the user desires the stream to have different behaviors for different values in the stream events. 

`Extrapolation`
QiStreamExtrapolation can have one of 4 values.

1.	All
2.	None
3.	Forward
4.	Backward

This indicates whether indexes that are read before or after all data should attempt to return an event or not.
