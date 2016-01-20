Qi Containers
#############

When you create a tenant in Qi, you define a Type (which defines the structure of your data), 
a Stream (which creates an area in which to store your data), and you define a Stream Behavior 
(which defines rules for how data is read). 

Tenant information is stored within one or more *containers*. A *container* in this context 
stores information for a given tenant and can be thought of as an self-contained partition 
that you use to store the entirety of the data and metadata for your tenant.

You use containers to separate tenants into logical entities. For example, you might 
want to have one tenant for production, one for development, and perhaps one or more 
containers for QA or to serve as a pre-production staging area for certification testing.

You can create, delete, or obtain information about your containers using the following Qi methods:

The following table shows the required and optional Qi Container objects:

+---------------+-------------------------+----------------------------------------+
| Object        | Type                    | Details                                |
+===============+=========================+========================================+
| ContainerId   | String                  | Required Id for referencing the        |
|               |                         | container                              | 
+---------------+-------------------------+----------------------------------------+

**Rules for ContainerId**

!!! This info needs to be verified !!!
1. Not case sensitive
2. Spaces are allowed
3. Cannot start with two underscores ("\_\_")
4. Cannot contain forward slash or backslash characters ("/" or "\\")
5. Maximum length of 260 characters

GetContainers( )
----------------

**Qi Client Library**

::

    IEnumerable<string> GetContainers();
    Task<IEnumerable<string>> GetContainersAsync();

**Http**

::

    GET "Qi/Containers"

**Parameters**

*string*: 

**Security** Allowed by administrator and user accounts

**Operation** Returns ???

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

**Operation** Returns the type that is searched for by *typeId*

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

**Operation** Returns a Qi Type object. If the entity Id already exists on the
service, the existing type is returned to the caller unchanged.
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

**Operation** Deletes the type from service A. The type cannot be deleted from
the service if existing streams are associated with it.

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

The following are types are supported when
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
