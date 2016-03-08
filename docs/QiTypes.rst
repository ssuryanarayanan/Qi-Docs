Qi Types
########

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
