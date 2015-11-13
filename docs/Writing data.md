##InsertValue( )

*_Qi Client Library_*
```
void InsertValue<T>(string streamId, T item);
Task InsertValueAsync<T>(string streamId, T item);
```

*Http*
```
POST Qi/Streams/{streamId}/Data/InsertValue
```
Content is serialized event of type T

**Parameters**

*streamId*: Stream identifier for the request

*item*: Event to insert, where T is the type of the event and the stream

**Security**
Allowed by administrator account

**Operation**
Inserts data into the specified stream
Will throw an exception if an event already exists at the index of the item

##InsertValues( )

*_Qi Client Library_*
```
void InsertValues(IDictionary<string, IQiValues> items);
void InsertValues<T>(string streamId, IList<T> items);
Task InsertValuesAsync(IDictionary<string, IQiValues > items);
Task InsertValuesAsync<T>(string streamId, IList<T> items);
```

*_Http_*
```
POST Qi/Streams/{streamId}/Data/InsertValues
```
Content is serialized list of events of type T

**Parameters**

*streamId*: Stream identifier for the request

*items*: List of events to insert, where T is the type of the stream and events

**Security**
Allowed by administrator account

**Operation**
Inserts the items into the specified stream. Will throw an exception if any index in items already has an event. If any individual index has a problem, the entire operation is rolled back and no insertions are made. The streamId and index that caused the issue is included in the error response.

There are also overloads of the InsertValues method that allow the user to put a ‘batch’ of writes together and send data to multiple streams in the same operation. For more information see the Advanced Topics: ‘Methods that act upon Multiple Streams’ section.

##PatchValue( )

*_Qi Client Library_*
```
void PatchValue(string streamId, string selectExpression, T item);
Task PatchValueAsync(string streamId, string selectExpression, T item);
```

*_Http_*
```
PATCH Qi/Streams/{streamId}/Data/PatchValue?selectExpression={selectExpression}
```
Content is serialized patch property

**Parameters**

*streamId*: Stream identifier for the request

*selectExpression*: CSV list strings that indicates the event fields will be changed in stream events.

*item*: Object with index and new values to patch in the stream

**Security **
Allowed by administrator account

**Operation**
This call is used to modify the stream events. The values for each SelectExpression field are taken from the item and replaced (patched) in the stream using the item index.

**Example**
```
var obj = new { TimeId = DateTime.UtcNow(), Value = 10 };
PatchValue(“someStreamId”, “Value”, obj);
```

##PatchValues( )

*_Qi Client Library_*
```
void PatchValues(string streamId, string selectExpression, IList<T> items);
Task PatchValuesAsync(string streamId, string selectExpression, IList<T> items);
```

*_Http_*
```
PATCH Qi/Streams/{streamId}/Data/PatchValues?selectExpression={selectExpression}
```
Content is serialized list of patch property values

**Parameters**

*streamId*: Stream identifier for the request

*selectExpression*: CSV list strings that indicates the event fields will be changed in stream events

*items*: List which contain indexes and new values to patch in the stream

**Security** 
Allowed by administrator account

**Operation**
This call is used to patch the values of the selected fields for multiple events in the stream. Only the fields indicated in selectExpression are modified. The events to be modified are indicated by the index value of each member of the items collection. The individual events in items also hold the new values. 

PatchValues may be thought of as a series of PatchValue calls. If there is a problem patching any individual event, the entire operation is rolled back and the error will indicate the streamID and index of the problem. 

##RemoveValue( )

*_Qi Client Library_*
```
void RemoveValue(string streamId, string index);
void RemoveValue<T1>(string streamId, T1 index);
void RemoveValue<T1, T2>(string streamId, Tuple<T1, T2> index);
Task RemoveValueAsync(string streamId, string index);
Task RemoveValueAsync<T1>(string streamId, T1 index);
Task RemoveValueAsync<T1, T2>(string streamId, Tuple<T1, T2> index);
```

*_Http_*
```
DELETE Qi/Streams/{streamId}/Data/RemoveValue?index={index}
```

**Parameters**

*streamId*: Stream identifier for the request

*index*: String representation of the index in the stream to be deleted

**Security**
Allowed by administrator account

**Operation**
Removes the event at index from the specified stream. Precision can matter when finding a value. If the index is a DateTime, use the round-trip format specifier: DateTime.ToString(“o”).


##RemoveValues( )

*_Qi Client Library_*
```
void RemoveValues(string streamId, IEnumerable<string> index);
void RemoveValues<T1>(string streamId, IEnumerable<T1> index);
void RemoveValues<T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
Task RemoveValuesAsync(string streamId, IEnumerable<string> index);
Task RemoveValuesAsync<T1>(string streamId, IEnumerable<T1> index);
Task RemoveValuesAsync<T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
```

*_Http_*
```
DELETE Qi/Streams/{streamId}/Data/RemoveValues?index={index}
```

**Parameters**

*streamId*: Stream identifier for the request

*index*: List of indices at which to remove events in the stream

**Security**
Allowed by administrator account

**Operation**
Removes the event at each index from the specified stream 

If any individual event fails to be removed, the entire RemoveValues operation is rolled back and no removes are done. The streamId and index that caused the issue is included in the error response.


##RemoveWindowValues( )
*_Qi Client Library_*
```
void RemoveValues(string streamId, IEnumerable<string> index);
void RemoveValues<T1>(string streamId, IEnumerable<T1> index);
void RemoveValues<T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
Task RemoveValuesAsync(string streamId, IEnumerable<string> index);
Task RemoveValuesAsync<T1>(string streamId, IEnumerable<T1> index);
Task RemoveValuesAsync<T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
```

*_Http_*
```
DELETE Qi/Streams/{streamId}/Data/RemoveWindowValues?startIndex={startIndex}&endIndex={endIndex}
```

**Parameters**

*streamId*: Stream identifier for the request

*startIndex*: String representation of the starting index value

*endIndex*: String representation of the ending index value

**Security**
Allowed by administrator account.

**Operation**
Removes a range of values at and between the indices given. 

If any individual event fails to be removed, the entire operation is rolled back and no removes are done. 

##ReplaceValue( )

*_Qi Client Library_*
```
void ReplaceValue<T>(string streamId, T item);
Task ReplaceValueAsync<T>(string streamId, T item);
```

*_Http_*
```
PUT Qi/Streams/{streamId}/Data/ReplaceValue
```
Content is serialzied replacement object

*Parameters*

*streamId*: Identifier of the stream in which to replace value

*item*: Item to replace existing stream event

**Security** 
Allowed by administrator account

**Operation**
Writes an item over an existing event in the specified stream.
Throws an exception if the stream does not have an event at the index to be replaced

##ReplaceValues( )

*_Qi Client Library_*
```
void ReplaceValues(IDictionary<string, IQiValues> items);
void ReplaceValues<T>(string streamId, IList<T> items);
Task ReplaceValuesAsync(IDictionary<string, IQiValues > items);
Task ReplaceValuesAsync<T>(string streamId, IList<T> items);
```

*_Http_*
```
PUT Qi/Streams/{streamId}/Data/ReplaceValues
```
Content is serialized list of replacement values

**Parameters*

*streamId*: Stream identifier for the request

*items*: List of new items to replace existing items in the stream

**Security**
Allowed by administrator account.

**Operation**
Writes `items` over existing events in the specified stream. Throws an exception if any index does not have a value to be replaced.
If any individual event fails to be replaced, the entire operation is rolled back and no replaces are performed. The index that caused the issue and the streamId are included in the error response.

There are also overloads of the *ReplaceValues( )* method that allow the user to put a ‘batch’ of writes together and send data to multiple streams in the same operation. For more information see the Advanced Topics: ‘Methods that act upon Multiple Streams’ section.

##UpdateValue( )

*_Qi Client Library_*
```
void UpdateValue<T>(string streamId, T item);
Task UpdateValueAsync<T>(string streamId, T item);
```

*_Http_*
```
PUT Qi/Streams/{streamId}/Data/UpdateValue
```
Content is serialized updated value

**Parameters**

*streamId*: Stream identifier for the request

*item*: Event to write to the stream

**Security**
Allowed by administrator account

**Operation**
Writes item to specified stream
Performs an insert or a replace, depending on whether an event already exists at the index in the stream

##UpdateValues( )

*_Qi Client Library_*
```
void UpdateValues(IDictionary<string, IQiValues > items);
void UpdateValues<T>(string streamId, IList<T> items);
Task UpdateValuesAsync(IDictionary<string, IQiValues > items);
Task UpdateValuesAsync<T>(string streamId, IList<T> items);
```

*_Http_*
```
PUT Qi/Streams/{streamId}/Data/UpdateValues
```
Content is serialized list of updated values

**Parameters**

*streamId*: Stream identifier for the request

*items*: Events to write to the stream

**Security**
Allowed by administrator account

**Operation**
Writes items to the specified stream. Performs an insert or a replace, depending on whether an events already exists at the items indexes. 
If any item fails to write, entire operation is rolled back and no events are written to the stream. The index that caused the issue is included in the error response.

Overloads for several of the QiValue methods can be used to act upon multiple streams with a single call. For more information on these operations refer to [Advanced topics](https://qi-docs.readthedocs.org/en/latest/Advanced%20Topics/#methods-that-act-upon-multiple-streams).


##Write execption handling
If a method that acts upon multiple data events has a problem carrying out the operation an exception is thrown and none of the list of elements is acted upon. For example [*InsertValues( )*](https://qi-docs.readthedocs.org/en/latest/Data/#insertvalues) is called with a list of 100 events and one of the events uses an index at which there is already data present. An exception will be thrown and all of the events will be rolled back resulting in no inserts for the 100 events. The event at which the error occurred cwill be identified in the exception.

For example:

```
{
  _service.InsertValues(streamId, writeEvents);
}
catch (QiHttpClientException e)
{
 	:
  //  e.Errors.Values[0] indicates the streamId of the exception
  //  e.Errors.Values[1] indicates the TimeId of the exception
	:
}
```

