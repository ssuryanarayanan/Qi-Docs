Writing Data
============

In this section:

- `InsertValue( )`_
- `InsertValues( )`_
- `PatchValue( )`_
- `PatchValues( )`_
- `RemoveValue( )`_
- `RemoveValues( )`_
- `RemoveWindowValues( )`_
- `ReplaceValue( )`_
- `ReplaceValues( )`_
- `UpdateValue( )`_
- `UpdateValues( )`_
- `Write exception handling`_


InsertValue( )
------------

**Qi Client Library**

::

    void InsertValue<T>(string namespaceId, string streamId, T item);
    Task InsertValueAsync<T>(string namespaceId, string streamId, T item);

*Http*

::

    POST Qi/{namespaceId}/Streams/{streamId}/Data/InsertValue

Content is serialized event of type T

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*item*: Event to insert, where T is the type of the event and the stream

**Security** Allowed by administrator account

**Operation** Inserts data into the specified stream Will throw an
exception if an event already exists at the index of the item

InsertValues( )
------------

**Qi Client Library**

::

    void InsertValues(string namespaceId, IDictionary<string, IQiValues> items);
    void InsertValues<T>(string namespaceId, string streamId, IList<T> items);
    Task InsertValuesAsync(string namespaceId, IDictionary<string, IQiValues > items);
    Task InsertValuesAsync<T>(string namespaceId, string streamId, IList<T> items);

**Http**

::

    POST Qi/{namespaceId}/Streams/{streamId}/Data/InsertValues

Content is serialized list of events of type T

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*items*: List of events to insert, where T is the type of the stream and
events

**Security** Allowed by administrator account

**Operation** Inserts the items into the specified stream. Will throw an
exception if any index in **items** already has an event. If any individual
index encounters a problem, the entire operation is rolled back and no
insertions are made. The streamId and index that caused the issue are
included in the error response.

PatchValue( )
------------

**Qi Client Library**

::

    void PatchValue(string namespaceId, string streamId, string selectExpression, T item);
    Task PatchValueAsync(string namespaceId, string streamId, string selectExpression, T item);

**Http**

::

    PATCH Qi/{namespaceId}/Streams/{streamId}/Data/PatchValue?selectExpression={selectExpression}

Content is serialized patch property

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*selectExpression*: CSV list strings that indicates the event fields
will be changed in stream events.

*item*: Object with index and new values to patch in the stream

**Security** Allowed by administrator account

**Operation** This call is used to modify the stream events. The values
for each **SelectExpression** field are taken from the item and replaced
(patched) in the stream using the **item** index.

**Example**

::

    var obj = new { TimeId = DateTime.UtcNow(), Value = 10 };
    PatchValue(namespaceId, streamId, “Value”, obj);

PatchValues( )
------------

**Qi Client Library**

::

    void PatchValues(string namespaceId, string streamId, string selectExpression, IList<T> items);
    Task PatchValuesAsync(string namespaceId, string streamId, string selectExpression, IList<T> items);

**Http**

::

    PATCH Qi/{namespaceId}/Streams/{streamId}/Data/PatchValues?selectExpression={selectExpression}

Content is serialized list of patch property values

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*selectExpression*: CSV list strings that indicates the event fields
that will be changed in stream events

*items*: List which contain indexes and new values to patch in the
stream

**Security** Allowed by administrator account

**Operation** This call is used to patch the values of the selected
fields for multiple events in the stream. Only the fields indicated in
**selectExpression** are modified. The events to be modified are indicated
by the index value of each member of the **items** collection. The
individual events in **items** also hold the new values.

**PatchValues** may be thought of as a series of PatchValue calls. If there
is a problem patching any individual event, the entire operation is
rolled back and the error will indicate the streamID and index of the
problem.

RemoveValue( )
------------

**Qi Client Library**

::

    void RemoveValue(string namespaceId, string streamId, string index);
    void RemoveValue<T1>(string namespaceId, string streamId, T1 index);
    void RemoveValue<T1, T2>(string namespaceId, string streamId, Tuple<T1, T2> index);
    Task RemoveValueAsync(string namespaceId, string streamId, string index);
    Task RemoveValueAsync<T1>(string namespaceId, string streamId, T1 index);
    Task RemoveValueAsync<T1, T2>(string namespaceId, string streamId, Tuple<T1, T2> index);

**Http**

::

    DELETE Qi/{namespaceId}/Streams/{streamId}/Data/RemoveValue?index={index}

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*index*: String representation of the index in the stream to be deleted

**Security** Allowed by administrator account

**Operation** Removes the event at the index from the specified stream.
Precision is taken into account when finding a value. If the index is a DateTime,
use the round-trip format specifier: ``DateTime.ToString(“o”)``.

RemoveValues( )
------------

**Qi Client Library**

::

    void RemoveValues(string namespaceId, string streamId, IEnumerable<string> index);
    void RemoveValues<T1>(string namespaceId, string streamId, IEnumerable<T1> index);
    void RemoveValues<T1, T2>(string namespaceId, string streamId, IEnumerable<Tuple<T1, T2>> index);
    Task RemoveValuesAsync(string namespaceId, string streamId, IEnumerable<string> index);
    Task RemoveValuesAsync<T1>(string namespaceId, string streamId, IEnumerable<T1> index);
    Task RemoveValuesAsync<T1, T2>(string namespaceId, string streamId, IEnumerable<Tuple<T1, T2>> index);

**Http**

::

    DELETE Qi/{namespaceId}/Streams/{streamId}/Data/RemoveValues?index={index}

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*index*: List of indices at which to remove events in the stream

**Security** Allowed by administrator account

**Operation** Removes the event at each index from the specified stream

If any individual event fails to be removed, the entire RemoveValues
operation is rolled back and no removes are done. The streamId and index
that caused the issue are included in the error response.

RemoveWindowValues( )
------------

**Qi Client Library**

::

    void RemoveValues(string namespaceId, string streamId, IEnumerable<string> index);
    void RemoveValues<T1>(string namespaceId, string streamId, IEnumerable<T1> index);
    void RemoveValues<T1, T2>(string namespaceId, string streamId, IEnumerable<Tuple<T1, T2>> index);
    Task RemoveValuesAsync(string namespaceId, string streamId, IEnumerable<string> index);
    Task RemoveValuesAsync<T1>(string namespaceId, string streamId, IEnumerable<T1> index);
    Task RemoveValuesAsync<T1, T2>(string namespaceId, string streamId, IEnumerable<Tuple<T1, T2>> index);

**Http**

::

    DELETE Qi/{namespaceId}/Streams/{streamId}/Data/RemoveWindowValues?startIndex={startIndex}&endIndex={endIndex}

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*startIndex*: String representation of the starting index value

*endIndex*: String representation of the ending index value

**Security** Allowed by administrator account.

**Operation** Removes a range of values at and between the given indices.

If any individual event fails to be removed, the entire operation is
rolled back and no removes are done.

ReplaceValue( )
------------

**Qi Client Library**

::

    void ReplaceValue<T>(string namespaceId, string streamId, T item);
    Task ReplaceValueAsync<T>(string namespaceId, string streamId, T item);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}/Data/ReplaceValue

Content is serialzied replacement object

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Identifier of the stream in which to replace value

*item*: Item to replace existing stream event

**Security** Allowed by administrator account

**Operation** Writes an item over an existing event in the specified
stream. Throws an exception if the stream does not have an event to be replaced at the
index.

ReplaceValues( )
------------

**Qi Client Library**

::

    void ReplaceValues(string namespaceId, IDictionary<string, IQiValues> items);
    void ReplaceValues<T>(string namespaceId, string streamId, IList<T> items);
    Task ReplaceValuesAsync(string namespaceId, IDictionary<string, IQiValues > items);
    Task ReplaceValuesAsync<T>(string namespaceId, string streamId, IList<T> items);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}/Data/ReplaceValues

Content is serialized list of replacement values

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*items*: List of new items to replace existing items in the stream

**Security** Allowed by administrator account.

**Operation** Writes **items** over existing events in the specified
stream. Throws an exception if any index does not have a value to be
replaced. If any individual event fails to be replaced, the entire
operation is rolled back and no replaces are performed. The index that
caused the issue and the streamId are included in the error response.

UpdateValue( )
------------

**Qi Client Library**

::

    void UpdateValue<T>(string namespaceId, string streamId, T item);
    Task UpdateValueAsync<T>(string namespaceId, string streamId, T item);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}/Data/UpdateValue

Content is serialized updated value

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*item*: Event to write to the stream

**Security** Allowed by administrator account

**Operation** Writes **item** to the specified stream. Performs an insert or a
replace depending on whether an event already exists at the index in
the stream.

UpdateValues( )
------------

**Qi Client Library**

::

    void UpdateValues(string namespaceId, IDictionary<string, IQiValues > items);
    void UpdateValues<T>(string namespaceId, string streamId, IList<T> items);
    Task UpdateValuesAsync(string namespaceId, IDictionary<string, IQiValues > items);
    Task UpdateValuesAsync<T>(string namespaceId, string streamId, IList<T> items);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}/Data/UpdateValues

Content is serialized list of updated values

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Stream identifier for the request

*items*: Events to write to the stream

**Security** Allowed by administrator account

**Operation** Writes items to the specified stream. Performs an insert
or a replace depending on whether an event already exists at the item's
indexes. If any item fails to write, the entire operation is rolled back and
no events are written to the stream. The index that caused the issue is
included in the error response.

