Qi Types
========

A QiType is what Qi uses to store definable data types. A QiType
consists of one or more properties that are either simple atomic types
(e.g. integer) or previously-defined QiTypes. A type is always
referenced with its Id property. Leveraging a QiType as a property
permits the construction of complex, nested data types. A QiType must
have have one or more properties that constitute an ordered key to be
used as an index. While a timestamp (DateTime) is a very common type of
key, any ordered value is permitted.

Once a QiType is created it cannot be changed and can only be deleted if
there are no streams associated with it.

The following table defines the required and optional QiType objects:

+---------------+-------------------------+----------------------------------------+
| Object        | Type                    | Details                                |
+===============+=========================+========================================+
| Id            | String                  | Required Id for referencing the type   |
+---------------+-------------------------+----------------------------------------+
| Name          | String                  | Optional name                          |
+---------------+-------------------------+----------------------------------------+
| Description   | String                  | Optional description text              |
+---------------+-------------------------+----------------------------------------+
| QiTypeCode    | QiTypeCode              | Defines the type                       |
+---------------+-------------------------+----------------------------------------+
| Properties    | IList<QiTypeProperty>   | List of QiTypeProperty items           |
+---------------+-------------------------+----------------------------------------+

**Rules for typeId**

1. Case insensitive
2. Allows spaces
3. Cannot start with two underscores ("\_\_")
4. Cannot contain slash characters ("/" and "")
5. Maximum length of 260 characters

GetStreamType( )
----------------

**Qi Client Library**

::

    QiType GetStreamType(string streamId);
    Task<QiType> GetStreamTypeAsync(string streamId);

**Http**

::

    GET Qi/Streams/{streamId}/Type

**Parameters**

*streamId*: Id of the stream for which type request will be made

**Security** Allowed by administrator and user accounts

**Operation** Returns the type definition associated with a stream

GetType( )
----------------

**Qi Client Library**

::

    QiType GetType(string typeId);
    Task<QiType> GetTypeAsync(string typeId);

**Http**

::

    GET Qi/Types/{typeId}

**Parameters**

*typeId*: Id of the type to retrieve

**Security** Allowed by administrator and user accounts

**Operation** Returns type searched for by *typeId*

GetTypes( )
----------------

**Qi Client Library**

::

    IEnumerable<QiType> GetTypes();
    Task<IEnumerable<QiType>> GetTypesAsync();

**Http**

::

    GET Qi/Types

**Parameters**

None

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all types

GetOrCreateType( )
----------------

**Qi Client Library**

::

    QiType GetOrCreateType(QiType entity);
    Task<QiType> GetOrCreateTypeAsync(QiType entity);

**Http**

::

    POST Qi/Types

Content is serialized QiType entity

**Parameters**

*entity*: Qi Type object

**Security** Allowed by administrator account

**Operation** Returns a Qi Type object If entity already exists on the
service by Id, the existing type is returned to the caller unchanged.
Otherwise, a new type definition is added to the Qi Service

DeleteType( )
----------------

**Qi Client Library**

::

    void DeleteType(string typeId);
    Task DeleteTypeAsync(string typeId);

**Http**

::

    DELETE Qi/Types/{typeId}

**Parameters**

*typeId*: String typeId of the type to delete

**Security** Allowed by administrator account

**Operation** Deletes type from service A type cannot be deleted from
the service if there are existing streams associated with it

UpdateType( )
----------------

**Qi Client Library**

::

    void UpdateType(string typeId, QiType entity);
    Task UpdateTypeAsync(string typeId, QiType entity);

**Http**

::

    PUT Qi/Types/{typeId}

**Parameters**

*typeId*: String typeId of the type to update

**Security** Allowed by Administrator account

**Operation** Updates a type’s definition A type cannot be updated if
there are existing streams associated with it

Compound Indexes
----------------

When defining a QiType the index property to be used to sequence the
data must be defined by the user in the type definition. Often a single
index, such as DateTime, is used, but for more complex scenarios Qi
allows multiple indexes to be defined in a type. These indexes are
concatenated together to form a compound index. The Qi REST API methods
that use tuples were created to assist the user in using compound
indexes.

Supported QiTypes
----------------

The following are explicitely supported types that can be used when
creating a QiType:

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
