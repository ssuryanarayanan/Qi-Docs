API calls for writing data
==========================

**********

``InsertValue()``
----------------

Inserts data into the specified stream. 

**Syntax**


::

    void InsertValue<T>(string namespaceId, string streamId, T item);
    Task InsertValueAsync<T>(string namespaceId, string streamId, T item);

*Http*

::

    POST Qi/{namespaceId}/Streams/{streamId}/Data/InsertValue

Content is serialized event of type T
	
**Parameters**

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``T item``
  The event to insert, where T is the type of the event and the stream
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

**Notes**
  ``InsertValue()`` throws an exception if an event already exists at the specified index.

**********

``InsertValues()``
----------------

Inserts items into the specified stream. 


**Syntax**

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

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``T items``
  The list of events to insert, where T is the type of the stream and events
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

**Notes**
  ``InsertValues()`` throws an exception if any index in **items** already has an event. If any individual
  index encounters a problem, the entire operation is rolled back and no
  insertions are made. The streamId and index that caused the issue are
  included in the error response.
  
Security
  Allowed by administrator accounts

**********

``PatchValue()``
----------------

Modifies the specified stream event,


**Syntax**

::

    void PatchValue(string namespaceId, string streamId, string selectExpression, T item);
    Task PatchValueAsync(string namespaceId, string streamId, string selectExpression, T item);

**Http**

::

    PATCH Qi/{namespaceId}/Streams/{streamId}/Data/PatchValue?selectExpression={selectExpression}

	
Content is serialized patch property
	
**Parameters**

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``string selectExpression``
  CSV list of strings that indicates the event fields that will be changed in stream events.
``T item``
  Object with index and new values to patch in the stream.
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

**Notes**
  ``PatchValue()`` is used to modify the stream events. The values
  for each **SelectExpression** field are taken from the item and replaced
  (patched) in the stream using the **item** index.
  
Security
  Allowed by administrator accounts

**Example**

::

    var obj = new { TimeId = DateTime.UtcNow(), Value = 10 };
    PatchValue(namespaceId, streamId, “Value”, obj);  
  

  **********

``PatchValues()``
----------------

Patches values of the selected fields for multiple events in the stream.


**Syntax**

::

    void PatchValues(string namespaceId, string streamId, string selectExpression, IList<T> items);
    Task PatchValuesAsync(string namespaceId, string streamId, string selectExpression, IList<T> items);

**Http**

::

    PATCH Qi/{namespaceId}/Streams/{streamId}/Data/PatchValues?selectExpression={selectExpression}

Content is serialized list of patch property values

	
**Parameters**

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``string selectExpression``
  CSV list strings that indicates the event fields that will be changed in stream events.
``T items``
  List which contain indexes and new values to patch in the stream.
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

**Notes**
  ``PatchValues()`` is used to patch the values of the selected
  fields for multiple events in the stream. Only the fields indicated in
  **selectExpression** are modified. The events to be modified are indicated
  by the index value of each member of the **items** collection. The
  individual events in **items** also hold the new values.

  **PatchValues** may be thought of as a series of PatchValue calls. If there
  is a problem patching any individual event, the entire operation is
  rolled back and the error will indicate the streamID and index of the
  problem.  
  
**********

``RemoveValue()``
----------------

Removes the event at the index from the specified stream.


**Syntax**

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

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``index``
  String representation of the index in the stream to be deleted.
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

**Notes**
  Precision is taken into account when finding a value. If the index is a DateTime,
  use the round-trip format specifier: ``DateTime.ToString(“o”)``.  

**********

``RemoveValues()``
----------------

Removes the event at each index from the specified stream.


**Syntax**

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

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``index``
  List of indices at which to remove events in the stream
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

**Notes**
  If any individual event fails to be removed, the entire RemoveValues
  operation is rolled back and no events are removed. The streamId and index
  that caused the issue are included in the error response.


**********

``RemoveWindowValues()``
----------------

Removes a range of values at and between the given indices.


**Syntax**

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

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``startIndex``
  String representation of the starting index value.
``endIndex``
  String representation of the ending index value
  
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

**Notes**
  If any individual event fails to be removed, the entire operation is
  rolled back and no removes are done.

  
**********

``ReplaceValue()``
----------------

Writes an item over an existing event in the specified stream.


**Syntax**

::

    void ReplaceValue<T>(string namespaceId, string streamId, T item);
    Task ReplaceValueAsync<T>(string namespaceId, string streamId, T item);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}/Data/ReplaceValue

Content is serialzied replacement object

	
**Parameters**

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

**Notes**
  Throws an exception if the stream does not have an event to be replaced at the
  specified index.
  
  
``ReplaceValues()``
----------------

Writes **items** over existing events in the specified stream.


**Syntax**

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

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``T items``
  List of new items to replace existing items in the stream
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

  
**Notes**
  Throws an exception if any index does not have a value to be
  replaced. If any individual event fails to be replaced, the entire
  operation is rolled back and no replaces are performed. The index that
  caused the issue and the streamId are included in the error response.


``UpdateValue()``
----------------

Writes **item** to the specified stream.


**Syntax**

::

    void UpdateValue<T>(string namespaceId, string streamId, T item);
    Task UpdateValueAsync<T>(string namespaceId, string streamId, T item);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}/Data/UpdateValue

Content is serialized updated value

	
**Parameters**

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``T item``
  Event to write to the stream
  
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts
  
**Notes**
  ``UpdateValue()`` performs an insert or a replace depending on whether an event already exists at the index in the stream.
  

``UpdateValues()``
----------------

Writes items to the specified stream.


**Syntax**

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

``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``T items``
  Events to write to the stream.
  
**Optional parameters**

  None
  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts
  
 **Notes**
  ``UpdateValues()`` performs an insert
  or a replace depending on whether an event already exists at the item's
  indexes. If any item fails to write, the entire operation is rolled back and
  no events are written to the stream. The index that caused the issue is
  included in the error response.

