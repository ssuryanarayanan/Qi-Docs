Qi Stream Behaviors are applied to streams to affect how certain data read operations will be performed. The Stream Behavior object effects if interpolation and\or extrapolation will be done when the index of a read operation falls between, before or after stream data. 

A Stream Behavior object must first be defined and then can be applied to a stream when it is created (`GetOrCreateStream` method) or updated (`UpdateStream` method). 

Stream Behavior objects are always referenced by the `Id` property.

A Stream can be changed to use a different Stream Behavior or the Stream Behavior itself can be changed.

The default behavior for a stream (when a defined stream behavior is not applied to the stream) is `Mode = ‘Continuous’` and `ExtrapolationMode = ‘All’`.

The following Read Methods are effected by the Stream Behavior:
*GetValue( )*
*GetValues( )*
*GetWindowValues* (when Boundary is set to ExactOrCalculated)
*GetRangeValues* (when Boundary is set to ExactOrCalculated)

## Qi Stream Behavior Object

```
string Id
string Name
QiStreamMode Mode
IList<QiStreamBehaviorOverride> Overrides
QiStreamExtrapolation ExtrapolationMode
```

`Id` -- Unique identifier used to reference this behavior
`Name` -- Optional descriptor
`Mode` -- Interpolation behavior setting to all ‘value’ properties (unless overridden by Override list)
`Overrides` -- A list of `QiStreamBehaviorOverride items`, which is used to set a different Interpolation behavior than the Mode to a Type property
`ExtrapolationMode` -- Controls extrapolation behavior for the stream

## Stream Behavior Mode
When certain Qi Read methods (such as *GetValue( )* and *GetValues( )* are given an index that lands between two values in a stream, it is the Stream Behavior Mode that will determine what values will be retrieved. 

The chart here indicates the allowed QiStreamMode values and how they operate:

|`QiStreamMode`|Operation|
|---|---|
|0=Default|= Continuous|
|0=Continuous|Interpolates the data using previous and next index values\*|
|1=StepwiseContinuousLeading|Returns the data from the previous index|
|2=StepwiseContinuousTrailing|Returns the data from the next index|
|3=Discrete|Returns ‘null’|
         *Certain value types cannot be interpolated. See Stream Behavior for these special cases.

When the Mode is set to Continuous (or left at Default) calls to read the value of this Stream Behavior property will indicate ‘0=Default”. 

##Stream behavior overrides
As described above, the interpolation behavior for all values in the stream event type is determined by the stream behavior `Mode` property. Individual data properties can be overridden to act as another behavior by setting the `Overrides` property. In this way the user can have different interpolation behavior for different event properties within the stream. 

The Override field of the Stream Behavior is made up of a list of `QiStreamBehaviorOverride` object. The `QiStreamBehaviorOverride` object has the following structure:
	 `string QiTypePropertyId`
	 `QiStreamMode Mode`

Note that when using the override list, be aware that a Mode setting of ‘Discrete’ cannot be overridden. If the Mode is set to ‘Discrete’ a null value is returned for the entire event. If a ‘Discrete’ setting is desired for one of the type properties and a different setting (e.g. StepwiseContinuousLeading) is desired for other properties within the type, make sure to set the Mode to the StepwiseContinuousLeading and use the override list to set the other property to Discrete. 

Without the overrides, properties will get the interpolation behavior defined by the Mode field of the Stream Behavior.

##Stream behavior `QiStreamExtrapolation`
When the index of a *GetValue( )* (or *GetValues( )*) read falls before or after all of the data in a stream, the QIStreamExtrapolation setting acts as a master switch which determines whether or not extrapolation will be done. If extrapolation is done, the setting of the Stream Behavior Mode determines the value included. 

The following charts show how the `QiStreamExtrapolation` settings effect returned values:

__Stream behavior `QiStreamExtrapolation` values__

|`QiStreamExtrapolation`|Index before data|Index after data|
|---|---|---|
|0=All|Returns data of the first event|Returns data of the last event|
|1=None|No extrapolation (returns null)|No extrapolation (returns null)|
|2=Forward|No extrapolation (returns null)|Returns data of the last event|
|3=Backward|Returns data of the first event|No extrapolation (returns null)|

__`QiStreamExtrapolation` values (with Mode=Continuous)__

|`QiStreamExtrapolation`|Index before data|Index after data|
|---|---|---|
|*All*|Returns first data value|Returns last data value|
|*None*|Return null|Return null|
|*Forward*|Returns first data value|Return null|
|*Backward*|Return null|Returns last data value|

__`QiStreamExtrapolation` values (with Mode=Discrete)__

|`QiStreamExtrapolation`|Index before data|Index after data|
|---|---|---|
|*All*|Return null|Return null|
|*None*|Return null|Return null|
|*Forward*|Return null|Return null|
|*Backward*|Return null|Return null|

__`QiStreamExtrapolation` values (with Mode=StepwiseContinuousLeading)__

|`QiStreamExtrapolation`|Index before data|Index after data|
|---|---|---|
|*All*|Returns first data value|Returns last data value|
|*None*|Return null|Return null|
|*Forward*|Returns first data value|Return null|
|*Backward*|Return null|Returns last data value|

__`QiStreamExtrapolation` values (with Mode=StepwiseContinuousTrailing)__

|`QiStreamExtrapolation`|Index before data|Index after data|
|---|---|---|
|*All*|Returns first data value|Returns last data value|
|*None*|Return null|Return null|
|*Forward*|Returns first data value|Return null|
|*Backward*|Return null|Returns last data value|

## Naming Rules for Behavior Identifiers
1.	Case Insensitive
2.	Allows spaces
3.	Cannot start with two underscores ("__")
4.	Maximum of 260 characters

## Additional stream behavior topics

### Interpolation
When the Stream Behavior is set to ‘Continuous’ (or left at the default value which is also ‘Continuous’) read methods (Such as `GetValue( )`) attempt return an interpolated value for indexes that land between to existing data events in a stream. This cannot always be done (e.g. when the type is not ‘numeric’)

This chart describes how the Default Continuous Mode setting effects indexes between data in a stream:

__Stream Behavior Mode = Default (Continuous)__

|`Type`|Result for an index between data in a stream|Comment|
|---|---|---|
|Numeric Types|Interpolated\*|Rounding is done as needed for integer types|
|Time related Types|Interpolated|DateTime, DateTimeOffset, TimeSpan|
|Nullable Types|Returns ‘null’|Cannot reliably interpolate due to possibility of a null value
|Array and List Types|Returns ‘null’||
|String Type|Returns ‘null’||
|Boolean Type|Returns value of nearest index||
|Enumeration Types|Returns Enum value at 0|This may have a value for the enumeration|
|GUID|||
|Version|Returns ‘null’||
|IDictionary or Ienumerable|Returns ‘null’||

*When extreme values are involved in an interpolation (e.g. Decimal.MaxValue) the call may result in a ‘BadRequest’ exception if the interpolation cannot be completed successfully.

## Qi Stream Behavior Methods

### DeleteBehavior
*_Qi Client Library_*
```
void DeleteBehavior(string behaviorId);
Task DeleteBehaviorAsync(string behaviorId);
```

*_Http_*
```
DELETE Qi/Behaviors/{behaviorId}
```

**Parameters**
`behaviorId` -- id of the behavior to delete; the behavior must not be associated with any streams

**Security**
Allowed by Administrator account

**Operation**
Deletes the specified behavior
Stream Behaviors objects that are still associated with a stream cannot be deleted

### GetBehavior
*_Qi Client Library_*
```
QiStreamBehavior GetBehavior(string behaviorId);
Task<QiStreamBehavior> GetBehaviorAsync(string behaviorId);
```
*_Http_*
```
GET Qi/Behaviors/{behaviorId}
```

**Parameters**
`behaviorId` -- id of the behavior definition to retrieve

**Security**
Allowed by Administrator and User accounts

**Operation**
Gets a QiStreamBehavior object from service

### GetBehaviors
*_Qi Client Library_*
```
IEnumerable<QiStreamBehavior> GetBehaviors();
Task<IEnumerable<QiStreamBehavior>> GetBehaviorsAsync();
```

*_Http_*
```
GET Qi/Behaviors
```

**Parameters**
None

**Security**
Allowed by Administrator and User accounts

**Operation**
Returns IEnumerable of all behavior objects

### GetOrCreateBehavior
*_Qi Client Library_*
```
QiStreamBehavior GetOrCreateBehavior(QiStreamBehavior entity);
Task<QiStreamBehavior> GetOrCreateBehaviorAsync(QiStreamBehavior entity);
```

*_Http_*
```
POSTQi/Behaviors
```
Content is serialized `QiStreamBehavior` entity

**Parameters**
`entity` -- a QiStreamBehavior object to add to the Qi Service

**Security**
Allowed by Administrator account

**Operation**
Creates a QiStreamBehavior (or returns it if it already exists)
If `entity` already exists on the server by `Id`, that existing behavior is returned to the caller unchanged

### UpdateBehavior
*_Qi Client Library_*
```
void UpdateBehavior(string behaviorId, QiStreamBehavior entity);
Task UpdateBehaviorAsync(string behaviorId, QiStreamBehavior entity);
```
*_Http_*
```
PUT Qi/Behaviors/{behaviorId}
```
Content is a serialization of the behavior to update

**Parameters**
`behaviorId` -- identifier of the stream behavior to update
`entity` -- updated stream behavior 

**Security**
Allowed by Administrator account

**Operation**
This method replaces the stream’s existing behavior with those defined in the ‘entity’. If certain aspects of the existing behavior are meant to remain, they must be included in entity.

An override list can be included in the ‘entity’ of this call to cause the addition, removal or change to this list. 

The Stream Behavior Id cannot be changed.
