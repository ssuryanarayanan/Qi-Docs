======================
Qi Types API Reference
======================

A QiType is used by Qi to store definable data types. A QiType
consists of one or more properties that are either simple atomic types
(such as an integer) or previously-defined QiTypes. A type is always
referenced with its Id property. Using a QiType as a property
permits the construction of complex, nested data types. A QiType must
have have one or more properties that constitute an ordered key to be
used as an index. While a timestamp (DateTime) is a very common type of
key, any ordered value is permitted.

When a QiType is created it cannot be changed and it can only be deleted if
there are no streams associated with it.

The following table shows the required and optional QiType properties:

+---------------+-------------------------+----------------------------------------+
| Property      | Type                    | Details                                |
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

1. Is not case sensitive
2. Can contain spaces
3. Cannot begin with two underscores ("\_\_")
4. Cannot contain forward slash or backslash characters ("/" or "\\")
5. Can contain a maximum of 260 characters

``GetStreamType()``
----------------

The ``GetStreamType()`` method returns the type definition that is associated with a stream.

**Parameters**

string namespaceId
  The namespace identifier for the request
string streamId
  The ID of the stream for which the type request is made

**Optional parameters**

  None

**Syntax**

::

    QiType GetStreamType(string namespaceId, string streamId);
    Task<QiType> GetStreamTypeAsync(string namespaceId, string streamId);

**Http**

::

    GET Qi/{namespaceId}/Streams/{streamId}/Type

Security
  Allowed by administrator and user accounts
Operation
  Returns the type definition that is associated with a stream


GetType( )
----------------

**Qi Client Library**

::

    QiType GetType(string namespaceId, string typeId);
    Task<QiType> GetTypeAsync(string namespaceId, string typeId);

**Http**

::

    GET Qi/{namespaceId}/Types/{typeId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*typeId*: Id of the type to retrieve

**Security** Allowed by administrator and user accounts

**Operation** Returns the type that is searched for by *typeId*

GetTypes( )
----------------

**Qi Client Library**

::

    IEnumerable<QiType> GetTypes(string namespaceId);
    Task<IEnumerable<QiType>> GetTypesAsync(string namespaceId);

**Http**

::

    GET Qi/{namespaceId}/Types

**Parameters**

*namespaceId*: The namespace identifier for the request

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all types

GetOrCreateType( )
----------------

**Qi Client Library**

::

    QiType GetOrCreateType(string namespaceId, QiType entity);
    Task<QiType> GetOrCreateTypeAsync(string namespaceId, QiType entity);

**Http**

::

    POST Qi/{namespaceId}/Types

Content is serialized QiType entity

**Parameters**

*namespaceId*: The namespace identifier for the request

*entity*: Qi Type object

**Security** Allowed by administrator account

**Operation** Returns a Qi Type object. If the entity Id already exists on the
service, the existing type is returned to the caller unchanged.
Otherwise, a new type definition is added to the Qi Service

DeleteType( )
----------------

**Qi Client Library**

::

    void DeleteType(string namespaceId, string typeId);
    Task DeleteTypeAsync(string namespaceId, string typeId);

**Http**

::

    DELETE Qi/{namespaceId}/Types/{typeId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*typeId*: String typeId of the type to delete

**Security** Allowed by administrator account

**Operation** Deletes the type from service A. The type cannot be deleted from
the service if existing streams are associated with it.

UpdateType( )
----------------

**Qi Client Library**

::

    void UpdateType(string namespaceId, string typeId, QiType entity);
    Task UpdateTypeAsync(string namespaceId, string typeId, QiType entity);

**Http**

::

    PUT Qi/{namespaceId}/Types/{typeId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*typeId*: String typeId of the type to update

**Security** Allowed by Administrator account

**Operation** Updates a type’s definition. A type cannot be updated if
existing streams are associated with it.

Compound Indexes
----------------

When defining a QiType, the index property you use to sequence the
data must be defined in the type definition. Often a single
index, such as DateTime, is used but for more complex scenarios Qi
allows multiple indexes to be defined in a type. Multiple indexes are
concatenated to form a compound index. The Qi REST API methods
that use tuples were created to assist you to use compound
indexes.

Supported QiTypes
----------------

The following types are supported when
creating a QiType:

* Array
* Boolean
* BooleanArray
* Byte
* ByteArray
* ByteEnum
* Char
* CharArray
* DateTime
* DateTimeArray
* DateTimeOffset
* DateTimeOffsetArray
* DBNull
* Decimal
* DecimalArray
* Double
* DoubleArray
* Empty
* Guid
* GuidArray
* IDictionary
* IEnumerable
* IList
* Int16
* Int16Array
* Int16Enum
* Int32
* Int32Array
* Int32Enum
* Int64
* Int64Array
* Int64Enum
* NullableBoolean
* NullableByte
* NullableChar
* NullableDateTime
* NullableDateTimeOffset
* NullableDecimal
* NullableDouble
* NullableGuid
* NullableInt16
* NullableInt32
* NullableInt64
* NullableSByte
* NullableSingle
* NullableTimeSpan
* NullableUInt16
* NullableUInt32
* NullableUInt64
* Object
* SByte
* SByteArray
* SByteEnum
* Single
* SingleArray
* String
* StringArray
* TimeSpan
* TimeSpanArray
* UInt16
* UInt16Array
* UInt16Enum
* UInt32
* UInt32Array
* UInt32Enum
* UInt64
* UInt64Array
* UInt64Enum
* Version
* VersionArray
