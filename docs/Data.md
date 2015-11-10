## FindDistinctValue
*_Qi Client Library_*
```
T FindDistinctValue<T>(string streamId, string index, QiSearchMode mode);
T FindDistinctValue<T, T1>(string streamId, T1 index, QiSearchMode mode);
T FindDistinctValue<T, T1, T2>(string streamId, Tuple<T1, T2> index, QiSearchMode) 
Task<T> FindDistinctValueAsync<T>(string streamId, string index, QiSearchMode mode);
Task<T> FindDistinctValueAsync<T, T1>(string streamId, T1 index, QiSearchMode mode);
Task<T> FindDistinctValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index, QiSearchMode mode);
```
 *_Http_*
 ```
 GET Qi/Streams/{streamId}/Data/FindDistinctValue?index={index}&mode={mode}
 ```

**Parameters**
`streamId` -- stream identifier for the request
`index` -- string representation of the index value at which to search
`mode` -- search mode (see *Operation* below)

**Security**
Allowed by Administrator and User accounts

**Operation**
This method searches for data in a stream using the search mode defined. If a data is not found a null is returned.  

The (search) mode determines how the search for data is executed:

|Search Mode|Action|
|---|---|
|1=Exact|Returns a data if found at the index, else null is returned|
|2=ExactOrNext|Returns a data if found at the index or searches forward for the next index with data|
|3=ExactOrPrevious|Returns a data if found at the index or searches for the first previous index with data|
|4=Next|Searches forward (immediately after the index given) for the next index with data|
|5=Previous|Searches for the first previous index with data starting immediately behind the index given|

**Examples**

Assume a Type named *TestType* has been created (with a DateTime index field named TimeId field and a Double data field called ‘Value’). Assume that the stream identified by streamId was created with that type.

This call will obtain the most recent event in the stream by starting at the index ‘Now’ and search backwards until it finds a value. Note if the stream is empty a null will be returned in readEvent:

```
searchMode = QiSearchMode.ExactOrPrevious;
string index = DateTime.Now.ToString(“o”);
var  readEvent = _service.FindDistinctValue<TestType>(streamId, index, searchMode);
```

This call does the same thing, but illustrates the use of the generic overload allowing DateTime to be used directly as the index (instead of a string):

```
searchMode = QiSearchMode.ExactOrPrevious;
DateTime indexDT = DateTime.Now;
var  readEvent = _service.FindDistinctValue<TestType, DateTime>(streamId, indexDT, searchMode);
```

If the type of the stream has a compound index (such as a DateTime and an Integer), then this call can be used using Tuples to indicate the index.

```
searchMode = QiSearchMode.ExactOrPrevious;
var tupleId = new Tuple<DateTime, int>(DateTime.Now, 0);
var  readEvent = _service.FindDistinctValue<TestType, DateTime, int>(streamId, tupleId, searchMode);
```

##GetDistinctValue
*_Qi Client Library_*
```
T GetDistinctValue<T>(string streamId, string index);
T GetDistinctValue<T, T1>(string streamId, T1 index);
T GetDistinctValue<T, T1, T2>(string streamId, Tuple<T1, T2> index);
Task<T> GetDistinctValueAsync<T>(string streamId, string index);
Task<T> GetDistinctValueAsync<T, T1>(string streamId, T1 index);
Task<T> GetDistinctValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetDistinctValue?index={index}
```

**Parameters**
`streamId` -- stream identifier for the request
`index` -- string representation of the index value at which to search

**Security**
Allowed by Administrator and User accounts

**Operation**
This method returns an event from the specified stream at the specified index. An exception is thrown if no event exists at index.

**Examples**
This call will obtain the event in the stream at the index defined by ‘Now’. If there is no event at that index an exception will be thrown:

```
string index = DateTime.Now.ToString(“o”);
try
{
	var  readEvent = _service.GetDistinctValue<TestType>(streamId, index);
}
Catch (exception e)
{
	//handle exception
}
```

This overload:	**T GetDistinctValue<T, T1>(string streamId, T1 index);**
Can be used to supply the index of the call as a different type. 
See the *FindDistinctValue* examples for an illustration of this.

This overload:	**T GetDistinctValue<T, T1, T2>(string streamId, Tuple<T1, T2> index);**
Can be used to supply the index of the call as a tuple (for compound indexes). 
See the *FindDistinctValue* examples for an illustration of this.


## GetFirstValue
*_Qi Client Library_*
```
T GetFirstValue<T>(string streamId);
Task<T> GetFirstValueAsync<T>(string streamId);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetFirstValue
```

**Parameters**
`streamId` -- stream identifier for the request

**Security**
Allowed by Administrator and User accounts

**Operation**
Returns the first data event in the stream. Returns null if the stream has no data (no exception thrown).

##GetLastValue
*_Qi Client Library_*
```
T GetLastValue<T>(string streamId);
Task<T> GetLastValueAsync<T>(string streamId);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetLastValue
```

**Parameters**
`streamId` -- stream identifier for the request

**Security**
Allowed by Administrator and User accounts

**Operation**
Returns the last data event in the stream. Returns null if the stream has no data (no exception thrown).

##GetRangeValues
*_Qi Client Library_*
```
IEnumerable<T> GetRangeValues<T>(string streamId, string startIndex, int count);
IEnumerable<T> GetRangeValues<T>(string streamId, string startIndex, int count, bool reversed);
IEnumerable<T> GetRangeValues<T>(string streamId, string startIndex, int count, QiBoundaryType boundaryType);
IEnumerable<T> GetRangeValues<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType); 
IEnumerable<T> GetRangeValuesAsync<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType, string filterExpression);
Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int count);
Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int count, bool reversed);
Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int count, QiBoundaryType boundaryType);
Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType);
Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType, string filterExpression);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&reversed={reversed}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&boundaryType={boundaryType}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boun GET daryType={boundaryType}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boun GET daryType={boundaryType}&filterExpression={filterExpression}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&reversed={reversed}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&boundaryType={boundaryType}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boun GET daryType={boundaryType}
GET Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boundaryType={boundaryType}&filterExpression={filterExpression}
```

**Parameters**
`streamId` -- stream identifier for the request
`startIndex` -- string representation of the starting index value 
`count` -- number of events to return
`reversed` -- order of event retrieval; true to retrieve events in reverse order 
`skip` -- number of events to skip; skipped events are not returned or counted. (Applied after filterExpression. )
`boundaryType` -- enumeration indicating how to handle boundary events 
`filterExpression` -- string containing an OData filter expression (see *Operation* section below)

**Security**
Allowed by Administrator and User accounts

**Operation**
This call is used to obtain events from a stream based on a starting index and a requested number of events. The overloads allow the client to optionally specify search direction, number of events to skip over while getting collecting response, special boundary handling (for events near the startIndex) and an event filter.

The `GetRangeValue` call will search FORWARD if the ‘reverse’ parameter is false and it is REVERSE if the ‘reverse’ parameter is true. For overloads that do not include the ‘reverse’ parameter, the default is FORWARD.

The *skip* parameter indicates the number of events that the call will skip over before it collects events for the response.

BoundaryType has the following possible values:
•	Exact
•	ExactOrCalculated
•	Inside
•	Outside

The BoundaryType determines how to determine the first value in from the stream starting at the start index. This is also effected by the direction of the `GetRangeValues()` call. 

This chart indicates how the first value is determined in a `GetRangeValue( )` call for a FORWARD search and the BoundaryType shown:

|Boundary Type|First value obtained|
|---|---|
|Exact|The first value at or after the startIndex|
|ExactOrCalculated|If a value exists at the startIndex it is used, else a value is ‘calculated’ according to the Stream Behavior setting|
|Inside|The first value after the startIndex|
|Outside|The first value before the startIndex|

This chart indicates how the first value is determined in a `GetRangeValue( )` call for a REVERSE search and the BoundaryType shown:

|Boundary Type|First value obtained|
|---|---|
|Exact|The first value at or before the startIndex 
|ExactOrCalculated|If a value exists at the startIndex it is used, else a value is ‘calculated’ according to the Stream Behavior setting. See the *Calculated startIndex* topic below.|
|Inside|The first value before the startIndex|
|Outside|The first value after the startIndex|

The order of execution is to first determine the direction of the call and the starting event (using BoundaryType). Once the starting event is determined, the filterExpression is applied in the direction requested to determine potential return values. Next, `skip` is applied to pass over the specified number of events (including any calculated events). Finally, events up to the number specified by count are returned.

The filter expression uses OData query language. Most of the query language is supported.
More information on OData Filter Expressions can be found in Advanced Topics.

### Calculated startIndex
When the startIndex of a GetRangeValue call lands before, after or in-between data in the stream, and the ExactOrCalculated Boundary Type is used, the Stream Behavior determines whether an additional ‘calculated’ event is created and returned in the response. 

This chart indicates when an event will be calculated and included in the GetRangeValues response for a startIndex befroe or after all data in the stream. (This is for ‘Forward’ search modes – when the ‘reverse’ parameter is ‘False’):

|Stream Behavior Mode|Stream Behavior QiStreamExtrapolation|When start index is **before** all data|When start index is **after** all data|
|---|---|---|---|
|Continuous|All|Event is calculated\*|Event is calculated\*|
||None|No event calculated|No event calculated|
||Backward|Event is calculated\*|No event calculated|
||Forward|No event calculated|Event is calculated\*|
|Discrete|All|No event calculated|No event calculated|
||None|No event calculated|No event calculated|
||Backward|No event calculated|No event calculated|
||Forward|No event calculated|No event calculated|
|ContinuousLeading|All|No event calculated|Event is calculated\*|
||None|No event calculated|No event calculated|
||Backward|No event calculated|No event calculated|
||Forward|No event calculated|Event is calculated\*|
|ContinuousTrailing|All|Event is calculated\*|No event calculated|
||None|No event calculated|No event calculated|
||Backward|Event is calculated\*|No event calculated|
||Forward|No event calculated|No event calculated|

			*Events is calculated using startIndex and the value of the first event

When the startIndex falls between data:

|Stream Behavior Mode|Calculated Event|
|---|---|
|Continuous|Event is calculated using the index and a value interpolated form the surrounding index values|
|Discrete|No event calculated|
|ContinuousLeading|Event is calculated using the index and previous event values|
|ContinuousTrailing|Event is calculated using the index and next event values|

## GetValue
*_Qi Client Library_*
```
T GetValue<T>(string streamId, string index);
T GetValue<T, T1>(string streamId, T1 index);
T GetValue<T, T1, T2>(string streamId, Tuple<T1, T2> index);
Task<T> GetValueAsync<T>(string streamId, string index);
Task<T> GetValueAsync<T, T1>(string streamId, T1 index);
Task<T> GetValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetValue?index={index}
```

**Parameters**
`streamId` -- stream identifier for the request
`index` -- string representation of the index value for GetValue or IEnumerable of index values requested for GetValues

**Security**
Allowed by Administrator and User accounts

**Operation**
If there is a value at the index, the call will return that event.

If the specified index is before or after all events, the value returned with that index is determined by stream's behavior (specifically its extrapolation setting).

If the specified index is between events, the event returned is determined by stream's behavior and any behavior overrides.

If the stream contains no data, null is returned regardless of the stream's behavior.

**Examples**
This call will obtain the event in the stream at the index defined by ‘Now’. If there is no event at that index the result will be determined by the Stream Behavior for that stream.

```
string index = DateTime.Now.ToString(“o”);
try
{
	var  readEvent = _service.GetValue<TestType>(streamId, index);
}
Catch (exception e)
{
	//handle exception
}
```

This overload:	**T GetValue<T, T1>(string streamId, T1 index);**
Can be used to supply the index of the call as a different type. 
See the FindDistinctValue examples for an illustration of this.

This overload:	**T GetValue<T, T1, T2>(string streamId, Tuple<T1, T2> index);**
Can be used to supply the index of the call as a tuple (for compound indexes).
See the FindDistinctValue examples for an illustration of this.

##GetValues
*_Qi Client Library_*
```
IEnumerable<T> GetValues<T>(string streamId, IEnumerable<string> index);
IEnumerable<T> GetValues<T, T1>(string streamId, IEnumerable<T1> index);
IEnumerable<T> GetValues<T, T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
IEnumerable<T> GetValues<T>(string streamId, string filterExpression);
IEnumerable<T> GetValues<T>(string streamId, string startIndex, string endIndex, int count);
IEnumerable<T> GetValues<T, T1>(string streamId, T1 startIndex, T1 endIndex, int count);
IEnumerable<T> GetValues<T, T1, T2>(string streamId, Tuple<T1, T2> startIndex, Tuple<T1, T2> endIndex, int count);
Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, IEnumerable<string> index);
Task<IEnumerable<T>> GetValuesAsync<T, T1>(string streamId, IEnumerable<T1> index);
Task<IEnumerable<T>> GetValuesAsync<T, T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, string filterExpression);
Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, string startIndex, string endIndex, int count);
Task<IEnumerable<T>> GetValuesAsync<T, T1>(string streamId, T1 startIndex, T1 endIndex, int count);
Task<IEnumerable<T>> GetValuesAsync<T, T1, T2>(string streamId, Tuple<T1, T2> startIndex, Tuple<T1, T2> endIndex, int count);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetValues?startIndex={startIndex}&endIndex={endIndex}&count={count}
```

**Parameters**
`streamId` -- stream identifier for the request
`index` -- string representation of the index value for GetValue or IEnumerable of index values requested for GetValues
`startIndex` -- string representation of the starting index value for GetValues
`endIndex` -- string representation of the ending index value for GetValues
`count` -- number of events to return for GetValues

**Security **
Allowed by Administrator and User accounts

**Operation**
For the `GetValues( )` overloads that include a streamId and IEnumberable index list, the call acts like multiple GetValue calls. Each index indicated in the IEnumberable list obtains a return event according to the rules described in the `GetValue( )` call.

For the `GetValues( )` overloads that include a startIndex, endIndex and count, these parameters are used to create a list of indexes for which to obtain values. Each index in this created list a return event is determined according to the rules described in the `GetValue( )` call.

For the `GetValues( )` overloads that include the filterExpression parameters are used to create a list of indexes that match the OData filter text used.
More information on OData Filter Expressions can be found in Advanced Topics.

##GetWindowValues
*_Qi Client Library_*
```
IEnumerable<T> GetWindowValues<T>(string streamId, string startIndex, string endIndex);
IEnumerable<T> GetWindowValues<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType);
IEnumerable<T> GetWindowValues<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, string filterExpression);
IEnumerable<T> GetWindowValues<T>(string streamId, string startIndex, QiBoundaryType startBoundaryType, string endIndex, QiBoundaryType endBoundaryType, string filterExpression);
QiResultPage<T> GetWindowValues<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, int count, string continuationToken);
IEnumerable<T> GetWindowValues<T>(string streamId, string startIndex, QiBoundaryType startBoundaryType, string endIndex, QiBoundaryType endBoundaryType, string filterExpression, string selectExpression);
QiResultPage<T> GetWindowValues<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, string filterExpression, int count, string continuationToken);
Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex);
Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType);
Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, string filterExpression);
Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, QiBoundaryType startBoundaryType, string endIndex, QiBoundaryType endBoundaryType, string filterExpression);
Task<QiResultPage<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, int count, string continuationToken);
Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, QiBoundaryType startBoundaryType, string endIndex, QiBoundaryType endBoundaryType, string filterExpression, string selectExpression);
Task<QiResultPage<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, string filterExpression, int count, string continuationToken);
```

*_Http_*
```
GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}
GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}
GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}&filterExpression={filterExpression}
GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}
GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&startBoundaryType={startBoundaryType}&endIndex={endIndex}&endBoundaryType={endBoundaryType}&filterExpression={filterExpression}&selectExpression={selectExpression}
GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}
```

**Parameters**
`streamId` -- stream identifier for the request
`startIndex` -- string representation of the starting index value, must be less than endIndex
`endIndex` -- string representation of the ending index value
`boundaryType` -- enumeration describing how to handle boundary events
`filterExpression` -- OData filter expression
`count` -- number of events to return
`continuationToken` -- continuation token for handling multiple return data sets
`startBoundaryType` -- how to handle startIndex boundary events
`endBoundaryType` -- how to handle endIndex boundary events
`selectExpression` -- expression designating which fields of the stream's type should make up the return events 

**Security**
Allowed by Administrator and User accounts

**Operation**
The GetWindowValues method is used to obtain stream data between 2 indices.

BoundaryType has the following possible values:
•	Exact
•	ExactOrCalculated
•	Inside
•	Outside

The BoundaryType determines how to obtain the values at the border of the range defined by the startIndex and endIndex.  

This chart indicates how the first value is determined in a `GetWindowValues ( )` call for the startBoundaryType shown:

|`startBoundary Type`|First value obtained|
|---|---|
|Exact|The first value at or after the startIndex|
|ExactOrCalculated|If a value exists at the startIndex it is used, else a value is ‘calculated’ according to the Stream Behavior setting|
|Inside|The first value after the startIndex|
|Outside|The first value before the startIndex|

This chart indicates how the last value is determined in a `GetWindowValues( )` call for the endBoundaryType shown:

|`startBoundary Type`|First value obtained|
|---|---|
|Exact|The first value at or before the endIndex|
|ExactOrCalculated|If a value exists at the endIndex it is used, else a value is ‘calculated’ according to the Stream Behavior setting|
|Inside|The first value before the endIndex|
|Outside|The first value after the endtIndex|

Calls against an empty stream will always return a single null regardless of boundary type used. 

The filter expression uses OData syntax. See Filter expressions help for more info.

The select expression is a CSV list of strings that indicate which field of the stream Type are being requested. By default all type fields are included in the response. The ‘Select’ feature may improve the performance of the call by avoiding management of the unneeded fields. Note that the index is always included in the returned results.

Selection is applied before filtering, so any fields used in the filter expression must be included by the select statement. See The Advanced topics for more information on Filter Expressions.

###Calculated startIndex and endindex
When the startIndex or endIndex of a GetWindowValues call does not fall on an event in the stream, and the BoundaryMode of ‘ExactOrCalculated’ is used, an event may be created and returned in the GetWindowValues call response.

The following chart indicates the when a calculated event is created for indexes before or after stream data:

|Stream Behavior Mode|Stream Behavior QiStreamExtrapolation|When start index is **before** all data|When start index is **after** all data|
|---|---|---|---|
|Continuous|All|event is calculated\*|Event is calculated*|
||None|No event calculated|No event calculated|
||Backward|Event is calculated\*|No event calculated|
||Forward|No event calculated|Event is calculated\*|
|Discrete|All|No event calculated|No event calculated|
||None|No event calculated|No event calculated|
||Backward|No event calculated|No event calculated|
||Forward|No event calculated|No event calculated|
|ContinuousLeading|All|No event calculated|Event is calculated\*|
||None|No event calculated|No event calculated|
||Backward|No event calculated|No event calculated|
||Forward|No event calculated|Event is calculated\*|
|ContinuousTrailing|All|Event is calculated\*|No event calculated|
||None|No event calculated|No event calculated|
||Backward|Event is calculated\*|No event calculated
||Forward|No event calculated|No event calculated|

*When a startIndex event is calculated, the created event has the startIndex and the value of the first data event in the stream. When an endIndex is calculated, the created event uses the endIndex along with the value from the stream’s last data event. Any calculated events are returned along with the result of the `GetWindowValues( )` call.

If an index (startIndex or endIndex) in a GetWindowValues call lands between data in the stream, and the BoundaryT Type is set to ‘ExactOrCalculated’, and event will be created according to the following chart:

|Stream Behavior Mode|Calculated Event|
|---|---|
|Continuous|Event is calculated using the index and interpolated values|
|Discrete|No event calculated|
|ContinuousLeading|Event is calculated using the index and previous event values|
|ContinuousTrailing|Event is calculated using the index and next event values|

##InsertValue
*_Qi Client Library_*
```
void InsertValue<T>(string streamId, T item);
Task InsertValueAsync<T>(string streamId, T item);
```

*REST*
```
POST Qi/Streams/{streamId}/Data/InsertValue
```
Content is serialized event of type T

**Parameters**
`streamId` -- stream identifier for the request
`item` -- event to insert, where T is the type of the event and the stream

**Security**
Allowed by Administrator account

**Operation**
Inserts data into the specified stream. Will throw an exception if an event already exists at the index of the item

##InsertValues
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
`streamId` -- stream identifier for the request
`items` -- list of events to insert, where T is the type of the stream and events

**Security**
Allowed by Administrator account

**Operation**
Inserts the items into the specified stream. Will throw an exception if any index in items already has an event. If any individual index has a problem, the entire operation is rolled back and no insertions are made. The streamId and index that caused the issue is included in the error response.

There are also overloads of the InsertValues method that allow the user to put a ‘batch’ of writes together and send data to multiple streams in the same operation. For more information see the Advanced Topics: ‘Methods that act upon Multiple Streams’ section.

##PatchValue
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
`streamId` -- stream identifier for the request
`selectExpression` – CSV list strings that indicates the event fields will be changed in stream events.
`item` – object with index and new values to patch in the stream

**Security **
Allowed by Administrator account

**Operation**
This call is used to modify the stream events. The values for each SelectExpression field are taken from the item and replaced (patched) in the stream using the item index.

**Example**
```
var obj = new { TimeId = DateTime.UtcNow(), Value = 10 };
PatchValue(“someStreamId”, “Value”, obj);
```

##PatchValues
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
`streamId` -- stream identifier for the request
`selectExpression` -- CSV list strings that indicates the event fields will be changed in stream events
`items` -- list which contain indexes and new values to patch in the stream

**Security** 
Allowed by Administrator account

**Operation**
This call is used to patch the values of the selected fields for multiple events in the stream. Only the fields indicated in selectExpression are modified. The events to be modified are indicated by the index value of each member of the items collection. The individual events in items also hold the new values. 

PatchValues may be thought of as a series of PatchValue calls. If there is a problem patching any individual event, the entire operation is rolled back and the error will indicate the streamID and index of the problem. 

##RemoveValue
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
`streamId` -- stream identifier for the request
`index` -- string representation of the index in the stream to be deleted

**Security**
Allowed by Administrator account

**Operation**
Removes the event at index from the specified stream. Precision can matter when finding a value. If the index is a DateTime, use the round-trip format specifier: DateTime.ToString(“o”).


##RemoveValues
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
`streamId` -- stream identifier for the request
`index` -- list of indices at which to remove events in the stream

**Security**
Allowed by Administrator account

**Operation**
Removes the event at each index from the specified stream 

If any individual event fails to be removed, the entire RemoveValues operation is rolled back and no removes are done. The streamId and index that caused the issue is included in the error response.


##RemoveWindowValues
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
`streamId` -- stream identifier for the request
`startIndex` -- string representation of the starting index value
`endIndex` -- string representation of the ending index value

**Security**
Allowed by Administrator account.

**Operation**
Removes a range of values at and between the indices given. 

If any individual event fails to be removed, the entire operation is rolled back and no removes are done. 

##ReplaceValue
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
`streamId` -- identifier of the stream in which to replace value
`item` -- item to replace existing stream event

**Security** 
Allowed by Administrator account

**Operation**
Writes an item over an existing event in the specified stream.
Throws an exception if the stream does not have an event at the index to be replaced

##ReplaceValues
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
`streamId` -- stream identifier for the request
`items` -- list of new items to replace existing items in the stream

**Security**
Allowed by Administrator account.

**Operation**
Writes `items` over existing events in the specified stream. Throws an exception if any index does not have a value to be replaced.
If any individual event fails to be replaced, the entire operation is rolled back and no replaces are performed. The index that caused the issue and the streamId are included in the error response.

There are also overloads of the *ReplaceValues( )* method that allow the user to put a ‘batch’ of writes together and send data to multiple streams in the same operation. For more information see the Advanced Topics: ‘Methods that act upon Multiple Streams’ section.

##UpdateValue
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
`streamId` -- stream identifier for the request
`item` -- event to write to the stream

**Security**
Allowed by Administrator account

**Operation**
Writes item to specified stream. Performs an insert or a replace, depending on whether an event already exists at the index in the stream.

##UpdateValues
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
`streamId` -- stream identifier for the request
`items` -- events to write to the stream

**Security**
Allowed by Administrator account

**Operation**
Writes items to the specified stream. Performs an insert or a replace, depending on whether an events already exists at the items indexes. 
If any item fails to write, entire operation is rolled back and no events are written to the stream. The index that caused the issue is included in the error response.

Overloads for several of the QiValue methods can be used to act upon multiple streams with a single call. For more information on these operations refer to [Advanced topics](https://qi-docs.readthedocs.org/en/latest/Advanced%20Topics/#methods-that-act-upon-multiple-streams).


###Write execption handling
If a method that acts upon multiple data events has a problem carrying out the operation an exception is thrown and none of the list of elements is acted upon. For example [*InsertValues*](https://qi-docs.readthedocs.org/en/latest/Data/#insertvalues) is called with a list of 100 events and one of the events uses an index at which there is already data present. An exception will be thrown and all of the events will be rolled back resulting in no inserts for the 100 events. The event at which the error occurred cwill be identified in the exception.

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

###Summary of write methods
The following table shows the methods that are available for writing and changing Qi data. For further detail refer to [QiValues](https://qi-docs.readthedocs.org/en/latest/Data/):

|Write methods    |Description                                                            |
|---              |---                                                                    |
|InsertValue( )         |Writes a single event in a stream                                |
|InsertValues( )        |Write multiple events in a stream\*                              |
|UpdateValue( )         |Writes (or replaces) a single event in a stream                  |
|UpdateValues( )	      |Writes (or replaces) multiple events in a stream\*               |
|RemoveValue( ) 	      |Deletes a single event from a stream                             |
|RemoveValues( )	      |Deletes multiple events form a stream                            |
|RemoveWindowValues( )	|Deletes all events from a stream in a defined range              |
|ReplaceValue( )	      |Replaces a single events in a stream                             |
|ReplaceValues( )	      |Replaces multiple values in a stream\*                           |
|PatchValue( )	        |Replaces specified properties in a single event in a stream      |
|PatchValues( ) 	      |Replaces specified properties from multiple events in a stream   |

	                                *Include overloads that can act upon multiple streams
