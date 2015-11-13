A QiType is what Qi uses to store definable data types. A QiType consists of one or more properties that are either simple atomic types (e.g. integer) or previously-defined QiTypes. A type is always referenced with its Id property. Leveraging a QiType as a property permits the construction of complex, nested data types. A QiType must have have one or more properties that constitute an ordered key to be used as an index. While a timestamp (DateTime) is a very common type of key, any ordered value is permitted. 

Once a QiType is created it cannot be changed and can only be deleted if there are no streams associated with it.

The following table defines the required and optional QiType objects:

|Object|Type|Details|
|---|---|---|
|Id|String|Required Id for referencing the type|
|Name|String|Optional name|
|Description|String|Optional description text|
|QiTypeCode|QiTypeCode|Defines the type|
|QiTypeProperty|Properties|List of QiTypeProperty items|

__Rules for typeId__

1.	Case insensitive
2.	Allows spaces
3.	Cannot start with two underscores ("__")
4.	Maximum length of 260 characters

## GetStreamType( )
*_Qi Client Library_*
```
QiType GetStreamType(string streamId);
Task<QiType> GetStreamTypeAsync(string streamId);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Type
```

**Parameters**

*streamId*: Id of the stream for which type request will be made

**Security**
Allowed by administrator and user accounts

**Operation**
Returns the type definition associated with a stream

## GetType( )
*_Qi Client Library_*
```
QiType GetType(string typeId);
Task<QiType> GetTypeAsync(string typeId);
```
*_Http_*
```
GET Qi/Types/{typeId}
```

**Parameters**

*typeId*: Id of the type to retrieve

**Security**
Allowed by administrator and user accounts

**Operation**
Returns type searched for by *typeId*

## GetTypes( )
*_Qi Client Library_*
```
IEnumerable<QiType> GetTypes();
Task<IEnumerable<QiType>> GetTypesAsync();
```

*_Http_*
```
GET Qi/Types
```

**Parameters**

None

**Security**
Allowed by administrator and user accounts

**Operation**
Returns IEnumerable of all types

## GetOrCreateType( )
*_Qi Client Library_*
```
QiType GetOrCreateType(QiType entity);
Task<QiType> GetOrCreateTypeAsync(QiType entity);
```

*_Http_*
```
POST Qi/Types
```
Content is serialized QiType entity

**Parameters**

*entity*: Qi Type object

**Security**
Allowed by administrator account

**Operation**
Returns a Qi Type object
If entity already exists on the service by Id, the existing type is returned to the caller unchanged. Otherwise, a new type definition is added to the Qi Service

## DeleteType( )
*_Qi Client Library_*
```
void DeleteType(string typeId);
Task DeleteTypeAsync(string typeId);
```

*_Http_*
```
DELETE Qi/Types/{typeId}
```

**Parameters**

*typeId*: String typeId of the type to delete

**Security**
Allowed by administrator account

**Operation**
Deletes type from service
A type cannot be deleted from the service if there are existing streams associated with it

## UpdateType( )
*_Qi Client Library_*
```
void UpdateType(string typeId, QiType entity);
Task UpdateTypeAsync(string typeId, QiType entity);
```

*_Http_*
```
PUT Qi/Types/{typeId}
```

**Parameters**

*typeId*: String typeId of the type to update

**Security**
Allowed by Administrator account

**Operation**
Updates a type’s definition
A type cannot be updated if there are existing streams associated with it

##Supported QiTypes

The following are explicitely supported types that can be used when creating a QiType:

Empty

Object

DBNull

Boolean

Char

SByte

Byte

Int16

UInt16

Int32

UInt32

Int64

UInt64

Single

Double

Decimal

DateTime

String

Guid

DateTimeOffset

TimeSpan

Version

NullableBoolean

NullableChar

NullableSByte

NullableByte

NullableInt16

NullableUInt16

NullableInt32

NullableUInt32

NullableInt64

NullableUInt64

NullableSingle

NullableDouble

NullableDecimal

NullableDateTime

NullableGuid

NullableDateTimeOffset

NullableTimeSpan

BooleanArray

CharArray

SByteArray

ByteArray

Int16Array

UInt16Array

Int32Array

UInt32Array

Int64Array

UInt64Array

SingleArray

DoubleArray

DecimalArray

DateTimeArray

StringArray

GuidArray

DateTimeOffsetArray

TimeSpanArray

VersionArray

Array

IList

IDictionary

IEnumerable

SByteEnum

ByteEnum

Int16Enum

UInt16Enum

Int32Enum

UInt32Enum

Int64Enum

UInt64Enum
