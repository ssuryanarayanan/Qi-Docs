#FindDistinctValue
```
T FindDistinctValue<T>(string streamId, string index, QiSearchMode mode);
Task<T> FindDistinctValueAsync<T>(string streamId, string index, QiSearchMode mode);
```
 *REST*
 ```
 Qi/Streams/{streamId}/Data/FindDistinctValue?index={index}&mode={mode}
 ```
 
 HTTP GET
 
*Parameters*

- `streamId` -- stream identifier for the request
- `index` -- string representation of the index value at which to search
- `mode` -- search mode (see below)

Search modes:
1.	Exact 
2.	ExactOrNext 
3.	ExactOrPrevious 
4.	Next 
5.	Previous

This method is used in situations where the client software needs to query a stream without getting any exceptions when the indexes queried do not have data.

Returns null values for calls that do not find a value (e.g., searching in Next mode from an index after all existing data.)

#GetDistinctValue
```
T GetDistinctValue<T>(string streamId, string index);
Task<T> GetDistinctValueAsync<T>(string streamId, string index);
```

*REST*
```
Qi/Streams/{streamId}/Data/GetDistinctValue?index={index}
```

HTTP GET


*Parameters*

- `streamId` -- -- stream identifier for the request
- `index` -- string representation of the index value at which to search


This method is used by a client when data is expected to reside at the exact index value specified. 
Returns an event from the specified stream at the specified index. Throws an exception if no event exists at index or if the stream has no data.

#GetFirstValue
```
T GetFirstValue<T>(string streamId);
Task<T> GetFirstValueAsync<T>(string streamId);
```

*REST*
```
Qi/Streams/{streamId}/Data/GetFirstValue
```

HTTP GET

*Parameters*

`streamId` -- -- stream identifier for the request

Gets the first data event in the stream. Returns null if the stream has no data (no exception thrown).

#GetLastValue
```
T GetLastValue<T>(string streamId);
Task<T> GetLastValueAsync<T>(string streamId);
```

*REST*
```
Qi/Streams/{streamId}/Data/GetLastValue
```

HTTP GET

*Parameters*

`streamId` -- stream identifier for the request

Gets the last data event in the stream. Returns null if the stream has no data (no exception thrown).

#GetIntervals
```
IEnumerable<QiInterval<T>> GetIntervals<T>(string streamId, string startIndex, string endIndex, int count);
Task<IEnumerable<QiInterval<T>>> GetIntervalsAsync<T>(string streamId, string startIndex, string endIndex, int count);
```

*REST*
```
Qi/Streams/{streamId}/Data/GetIntervals?startIndex={startIndex}&endIndex={endIndex}&count={count}
```

HTTP GET

*Parameters*

- `streamId` -- stream identifier for the request
- `startIndex` -- string representation of the starting index value
- `endIndex` -- string representation of the ending index value
- `count` -- number of intervals

The call accepts start and end indexes and a reqeusted interval count. Intervals are created by dividing the index range into equal parts. The start and end index values will use an ‘ExactOrCalculated’ search mode, so each interval will typically have two or more values with which to work.

A QiInterval is made up of a start, an end event, and a Summaries dictionary.
IDictionary<string, IDictionary<QiSummaryType, object>> Summaries
T End
T Start

Each entry in Summaries corresponds to a field within the the type for which summary calculations were performed.  The name of the type field serves as the key while the value holds a dictionary of calculation results for that field. For example, if a type was created with a DateTime (the index), two double values, and a string value, the GetIntervals Summaries result would include two entries - one for each of the doubles.

Summary calculations include the following:
1.	Minimum	+ index of the first occurence of the minimum value
2.	Maximum	+ index of the first occurence of the maximum value
3.	Range
4.	Total
5.	Mean
6.	StandardDeviation
7.	PopulationStandardDeviation
8.	Skewness
9.	Kurtosis
10.	WeightedMean
11.	WeightedStandardDeviation
12.	WeightedPopulationStandardDeviation
13.	Integral

#GetRangeValues
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

*REST*
```
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&reversed={reversed}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&boundaryType={boundaryType}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boundaryType={boundaryType}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boundaryType={boundaryType}&filterExpression={filterExpression}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&reversed={reversed}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&boundaryType={boundaryType}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boundaryType={boundaryType}
Qi/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boundaryType={boundaryType}&filterExpression={filterExpression}
```
HTTP GET

*Parameters*
- `streamId` -- stream identifier for the request
- `startIndex` -- string representation of the starting index value
- `count` -- number of events to return
- `reversed` -- order of event retrieval; true to retrieve events in reverse order
- `skip` --  number of events to skip; skipped events are not returned or counted. Applied after filterExpression.
- `boundaryType` -- enumeration indicating how to handle boundary events
- `filterExpression` -- string containing an OData filter expression (see below)

This call is used to obtain events from a stream based on a starting index and requested number of events. The overloads allow the client to optionally specify search direction, number of events to skip, special boundary handling, and an event filter.

Boundary type meaning for forward (default, reversed = false) calls:
- `Exact` will use the first event at or after startIndex  
- `ExactOrCalculated` if an event exists at startIndex, that event is used. Otherwise the stream's Behavior determines whether a value is calculated at startIndex. The result will either be no event or an event at startIndex with the value of the next event in the stream. 
- `Inside` will find the first event after startIndex  
- `Outside` will find the first event before startIndex

Boundary type meaning for reverse (reversed = true) calls:
- `Exact` will find the first event at or before startIndex 
- `ExactOrCalculated` if an event exists at startIndex, that event is used. Otherwise the stream's Behavior determines whether a value is calculated at startIndex. The result will either be no event or an event at startIndex with the value of the previous event in the stream. 
- `Inside` will find the first event before startIndex
- `Outside` will find the first event after startIndex 

Once the starting event is determined, `filterExpression` is applied in the direction requested to determine potential return values. Next, `skip` is applied to pass over the specified number of events. Finally, events up to the number specified by count are returned.

The filter expression uses OData query language. Most of the query language is supported.
	
*GetValue* and *GetValues*
```
T GetValue<T>(string streamId, string index);
Task<T> GetValueAsync<T>(string streamId, string index);
IEnumerable<T> GetValues<T>(string streamId, IEnumerable<string> index);
IEnumerable<T> GetValues<T>(string streamId, string startIndex, string endIndex, int count);
Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, IEnumerable<string> index);
```

*REST*
```
Qi/Streams/{streamId}/Data/GetValue?index={index}
Qi/Streams/{streamId}/Data/GetValues?startIndex={startIndex}&endIndex={endIndex}&count={count}
```

HTTP GET

*Parameters*

- `streamId` -- stream identifier for the request
- `index` -- string representation of the index value for GetValue or IEnumerable of index values requested for GetValues
- `startIndex` -- string representation of the starting index value for GetValues
- `endIndex` -- string representation of the ending index value for GetValues
- `count` -- number of events to return for GetValues

If the specified index is before all events, the event returned is determined by stream's behavior. By default, the first event is returned with the index of the call.

If the specified index is after all events, the event returned is determined by stream's behavior. By default, the last event is returned with the index of the call. 

If the specified index is between events, the event returned is determined by stream's behavior and any behavior overrides.

If the stream contains no data, null is returned regardless of the stream's behavior.

GetValues can generally be thought of as multiple GetValue calls.  GetValues can be requested with a list of requested indexes or by specifying a start index, end index, and count.  

If the stream contains no data, an array of nulls (one for each member in the requested list) is returned.

#GetWindowValues
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

*REST*
```
Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}
Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}
Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}&filterExpression={filterExpression}
Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}
Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&startBoundaryType={startBoundaryType}&endIndex={endIndex}&endBoundaryType={endBoundaryType}&filterExpression={filterExpression}&selectExpression={selectExpression}
Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}
```

HTTP GET

*Parameters*

- `streamId` -- stream identifier for the request
- `startIndex` -- string representation of the starting index value, must be less than endIndex
- `endIndex` string representation of the ending index value
- `boundaryType` -- enumeration describing how to handle boundary events.
- `filterExpression` -- OData filter expression
- `count` -- number of events to return
- `continuationToken` -- continuation token for handling multiple return data sets
- `startBoundaryType` -- how to handle startIndex boundary events
- `endBoundaryType` -- how to handle endIndex boundary events
- `selectExpression` -- expression designating which fields of the stream's type should make up the return events 

These methods are used to obtain data between 2 indices.

Boundary types:
-`Exact` (default): return values exactly on the start or end index value
-`Inside`: any value inside the range but not including the boundaries
-`Outside`: includes 1 value outside the boundary on both sides and any values at or inside the range
-`ExactOrCalculated`: Will create values for the endpoints given if value an event exactly at the index is not found. The calculation is performed based on the stream's behavior.

Calls against an empty stream will always return a single null regardless of boundary type used. 

The filter expression uses OData syntax. See Filter expressions help for more info.

The select expression is an OData expression that can improve efficiency by specifying the return of only required fields from the event type. The index is always included. If no select expression is specified, all fields are included in response. Selection is applied before filtering, so any fields used in the filter expression must be included by the select statement.

#InsertValue
```
void InsertValue<T>(string streamId, T item);
Task InsertValueAsync<T>(string streamId, T item);
```

*REST*
```
Qi/Streams/{streamId}/Data/InsertValue
```

HTTP POST
Body is serialized event of type T

*Parameters*

- `streamId` -- stream identifier for the request
- `item` -- event to insert, where T is the type of the event and the stream
- 
Inserts an item into the specified stream. Will throw an exception if an event already exists at the index of the item. 

#InsertValues
```
void InsertValues<T>(string streamId, IList<T> items);
Task InsertValuesAsync<T>(string streamId, IList<T> items);
```

*REST*
```
Qi/Streams/{streamId}/Data/InsertValues
```

HTTP POST
Body is serialized list of events of type T

*Parameters*

- `streamId` -- stream identifier for the request
- `items` -- list of events to insert, where T is the type of the stream and events

Inserts the items into the specified stream. Will throw an exception if any index in items already has an event. If any individual index has a problem, the entire operation is rolled back and no insertions are made. The index that caused the issue is included in the error response.

#PatchValue
```
void PatchValue(string streamId, string selectExpression, T item);
Task PatchValueAsync(string streamId, string selectExpression, T item);
```

*REST*
```
Qi/Streams/{streamId}/Data/PatchValue?selectExpression={selectExpression}
```

HTTP PATCH
Body is serialized patch property


*Parameters*

- `streamId` -- stream identifier for the request
- `selectExpression` -- OData expression selecting type fields to patch
- `item` -- object of the same type, T, as the property to patch

This call is used to patch the values of select fields of an event in the stream. The fields to be patched are specified by `selectExpression`.  Values of the selected fields at the index of `item` are replaced with the values of those fields from `item`. 

```
var obj = new { TimeId = DateTime.UtcNow(), Value = 10 };
PatchValue(“someStreamId”, “Value”, obj);
```

#PatchValues
```
void PatchValues(string streamId, string selectExpression, IList<T> items);
Task PatchValuesAsync(string streamId, string selectExpression, IList<T> items);
```

*REST*
```
Qi/Streams/{streamId}/Data/PatchValues?selectExpression={selectExpression}
```

HTTP PATCH
Body is serialized list of patch property values

*Parameters*

- `streamId` -- stream identifier for the request
- `selectExpression` -- OData expression for selecting type fields to patch
- `items` -- list of properties to patch

This call is used to patch the values of the selected fields for multiple events in the stream.  Only the fields indicated in `selectExpression` are modified. The events to be modified are indicated by the index value of each member of the `items` collection. The individual events in `items` also hold the new values.  
                
PatchValues may be thought of as a series of PatchValue calls. If there is a problem patching any individual event, the entire operation is rolled back. 


#RemoveValue
```
void RemoveValue(string streamId, string index);
Task RemoveValueAsync(string streamId, string index);
```

*REST*
```
Qi/Streams/{streamId}/Data/RemoveValue?index={index}
```

HTTP DELETE

*Parameters*

- `streamId` -- stream identifier for the request
- `index` -- string representation of the index value

Removes the event at `index` from the specified stream. Precision can matter when finding a value. If the index is a DateTime, use the round-trip format specifier: `DateTime.ToString(“o”)`.

#RemoveValues
```
void RemoveValues(string streamId, IEnumerable<string> index);
Task RemoveValuesAsync(string streamId, IEnumerable<string> index);
```

*REST*
```
Qi/Streams/{streamId}/Data/RemoveValues?index={index}
```

HTTP DELETE

*Parameters*

- `streamId` -- stream identifier for the request
- `index` -- list of indices at which to remove events

Removes the event at each index from the specified stream. If any individual event fails to be removed, the entire RemoveValues operation is rolled back and no removes are done.  The index that caused the issue is included in the error response.

#RemoveWindowValues
```
void RemoveWindowValues(string streamId, string startIndex, string endIndex);
Task RemoveWindowValuesAsync(string streamId, string startIndex, string endIndex);
```

*REST*
```
Qi/Streams/{streamId}/Data/RemoveWindowValues?startIndex={startIndex}&endIndex={endIndex}
```

HTTP DELETE

*Parameters*

- `streamId` -- stream identifier for the request
- `startIndex` -- string representation of the starting index value
- `endIndex` -- string representation of the ending index value

Removes a range of values at and between the indices given.  If any individual event fails to be removed, the entire operation is rolled back and no removes are done.

#ReplaceValue
```
void ReplaceValue<T>(string streamId, T item);
Task ReplaceValueAsync<T>(string streamId, T item);
```

*REST*
```
Qi/Streams/{streamId}/Data/ReplaceValue
```

HTTP PUT
Body is serialzied replacement object

*Parameters*

- `streamId` -- identifier of the stream in which to replace value
- `item` -- item to replace existing value

Writes an item over an existing value in the specified stream. Throws an exception if the stream does not have an event at the index to be replaced.

#ReplaceValues
```
void ReplaceValues<T>(string streamId, IList<T> items);
Task ReplaceValuesAsync<T>(string streamId, IList<T> items);
```

*REST*
```
Qi/Streams/{streamId}/Data/ReplaceValues
```

HTTP PUT
Body is serialized list of replacement values.

*Parameters*

- `streamId` -- stream identifier for the request
- `items` -- list of new items to replace existing items in the stream

Writes `items` over existing events in the specified stream. Throws an exception if any index does not have a value to be replaced.

If any individual event fails to be replaced, the entire operation is rolled back and no replaces are performed. The index that caused the issue is included in the error response.

#UpdateValue
```
void UpdateValue<T>(string streamId, T item);
Task UpdateValueAsync<T>(string streamId, T item);
```

*REST*
```
Qi/Streams/{streamId}/Data/UpdateValue
```

HTTP PUT
Body is serialized updated value

*Parameters*

- `streamId` -- stream identifier for the request
- `item` -- event to write to the stream

Writes `item` to specified stream.  Performs an insert or a replace, depending on whether an event already exists at the index of `item`.

#UpdateValues
```
void UpdateValues<T>(string streamId, IList<T> items);
Task UpdateValuesAsync<T>(string streamId, IList<T> items);
```

*REST*
```
Qi/Streams/{streamId}/Data/UpdateValues
```

HTTP PUT
Body is serialized list of updated values.

*Parameters*

- `streamId` -- stream identifier for the request
- `items` -- events to write to the stream

Writes `items` to specified stream.   Performs an insert or a replace, depending on whether an event already exists at the index of individual itesm.  If any item fails to write, entire operation is rolled back and no events are written to the stream. The index that caused the issue is included in the error response.
