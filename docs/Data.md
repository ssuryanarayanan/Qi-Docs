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

- `streamId` -- stream against which to perform retrieval
- `index` -- value of the index at which to retrieve a value (e.g., a DateTime if the stream's Type is indexed by a DateTime property)
- `mode` -- search mode enumeration indicating how to find the event

Search modes

1.	Exact 
2.	ExactOrNext 
3.	ExactOrPrevious 
4.	Next 
5.	Previous

This method is used in situations where the client software needs to query a stream without getting any exceptions when the indexes queried do not have data.

Returns null values for calls that do not find a value (e.g. Search of ‘Next’ from an index after all existing data.)

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

- `streamId` -- id of the stream to search
- `index` -- index value on which to search


This method is used by a client when data is specifically expected to reside at the index used. 
Returns event from the specified stream at the specified index. Throws exception if no event exists at index, or if the stream has no data.

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

`streamId` -- identifier of the stream to search

Gets the first data event in the stream. If the stream has no data – a ‘null’ is returned (no exception thrown)

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

`streamId` -- stream identifier

Gets the last data event in the stream. If the stream has no data – a ‘null’ is returned (no exception thrown)

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

- `streamId` -- identifier of the stream to search
- `startIndex` -- string representation of the index starting value
- `endIndex` -- string representation of the index ending value
- `count` -- number of intervals

The call accepts a start and end value and the count of intervals. Intervals are created by dividing the time range into equal parts. The start and end index values will use an ‘ExactOrCalculated’ search so every intervals will typically have 2 (or more) values with which to work.

A QiInterval is made up of a start and end event and a List of Summaries.
IDictionary<string, IDictionary<QiSummaryType, object>> Summaries
T End
T Start

A Summaries object corresponds to the fields within the the type for which calculations were made. For example, if a Type was created with a DateTime TimeId property as an index, and two double values and a string value, then a GetIntervals call would include 2 Summaries (one for each of the double elements).

Summaries are made up of the following list of calculations:

Facets show the following 13 calculations for the field for the Interval.

1.	Minimum		(also shows Index where the first instance if this occurred)	
2.	Maximum		(also shows Index where this first instance if this occurred)
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
- `streamId` -- identifier of the stream to search
- `startIndex` -- string representation of the start value of the stream's index property
- `count` -- number of events to return
- `reversed` -- true to return events in reverse order
- `skip` --  number of values to skip before starting the count to be returned. The filter expression is applied first.
- `boundaryType` -- enumeration indicating how to handle events on the boundaries
- `filterExpression` -- string containing an ODATA filter expression (see below)

This call is used to obtain events from a stream where a start point is provided and the number of events desired. The many overloads allow the client to indicate where to start, which direction to search, whether to skip any values and also allows a special filter to be applied to the events found.

*QiBoundaryType Behavior*
For `FORWARD` (default) calls:
- `Exact` will find the first event at or after the index  
- `ExactOrCalculated` if the index is on an event, that event is used. Otherwise the behavior and `ExtrapolationMode` value determine whether a value is calculated for the index used. The result will either be no event or an event with the index used in the call and the value of the next event in the stream. 
- `Inside` will find the first event after the index  
- `Outside` will find the first event before the index  

For `REVERSE` calls.
- `Exact` will find the first event at or before the index  
- `ExactOrCalculated` if the index is on an event, that event is used. Otherwise the behavior and `ExtrapolationMode` value determine whether a value is calculated for the index used. The result will either be no event or an event with the index used in the call and the value of the previous event in the stream. 
- `Inside` will find the first event before the index  
- `Outside` will find the first event after the index  

After the starting event is determined, any filter provided is applied to determine possible values (in the direction requested) that could be returned. Next the Skip parameter is applied, and finally the number of events (up to count) is returned.

Filter uses the OData query language. Most of the query language is supported.
	
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

- `streamId` -- id denoting the stream to search
- `index` -- value of an index into the stream type's index property.
- `startIndex` -- start index value
- `endIndex` -- end index value
- `count` -- number of events to return

If the specified index is before any events, the value returned is determined by stream Behavior Mode and Extrapolation setting. By default , the first event value is returned with the index of the call.

If the specified index is after all events, the value returned is determined by stream Behavior Mode and Extrapolation setting. By default (Continous Mode and Both Extrapolation), the last event value is returned with the index of the call. 

If the specified index is between events, the value returned is determined by stream Behavior Mode (and any overrides).

If no data in stream – returns NULL (regardless of stream behavior setting)

`GetValues` can generally be thought of as multiple `GetValue` calls.
If there is no data in a stream an array of NULLs (one for each member in requested list) is returned.

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

- `streamId` -- id of stream to search
- `startIndex` -- string representation of the start index value of the range
- `endIndex` -- string representation of the end index value of the range
- `boundaryType` -- enumeration describing how to handle events near boundaries
- `filterExpression` -- ODATA filter expression
- `count` -- number of events to return
- `continuationToken` -- continuation token for handling multiple return data sets
- `startBoundaryType` -- how to handle events near the start of the range
- `endBoundaryType` -- how to handle events near the end of the range
- `selectExpression` -- expression designating which proeprties of the event  should be included in the response. 

These methods are used to obtain data between 2 indices.

The starting index value must be less than the ending index value. `BoundaryCondition` is `Exact` unless otherwise set 

BoundaryConditions:

- `Exact`: return values exactly on the start or end index value
- `Inside`: any value inside the range but not including the boundaries
- `Outside`: includes 1 value outside the boundary on both sides and any values at or inside the range
- `ExactOrCalculated`: Will create values for the endpoints given if value at Exact index not found. Behavior for given value is used in calculation (Continuous is default)

Calls against an empty stream will always return a single null regardless of boundary type used. 

Filter uses OData queries. Please see the section on Filter Text at the end of this document.

The select expression is an ODATA syntax expression designating which properties of the event type to return. The index property is always included. Separate multiple fields with a comma. If the select expression is blank, then all fields are included in response. By only selecting fields of interest the call can be made more efficiently. Selection is applied before filtering, so any fields excluded by select expression cannot be used in the filter expression.

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

- `streamId` -- identifier of the stream into which to insert a value
- `item` -- event to insert, where T is the type of the event and the stream
Inserts an item into the specified stream. Will throw an exception if the index of item already has an event. 

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

- `streamId` -- identifier of the stream into which to insert values
- `items` -- list of items of type T

Inserts items into the specified stream. Will throw an exception if any index in items already has an event. If any individual index has a problem, the entire list of events is rolled back and no inserts at all are done. The index that caused the issue can be determined in the error response.

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

- `streamId` -- identifier of the stream to update
- `selectExpression` -- expression selecting events for patching
- `item` -- object of the same type, T, as the property to patch

Patches a value at the index noted by T item using the value denoted by selectExpression and T item from the specified stream, e.g. 
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

- `streamId` -- identifier of the stream on which to operate
- `selectExpression` -- ODATA expression for selecting values to patch
- `items` -- list of properties to patch

This call is used to modify properties of specific events in a stream.  Only the properties indicated in `selectExpression` are modified. Property names in `selectExpression` are separated by a comma. The events to be modified are indicated by the index value in each member of the `items` collection. The individual events in `items` also holds the new values.  
                
PatchValues is basically a series of PatchValue calls. If there are any problems completing the entire list of ‘patches’ the entire operation is rolled back. 


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

- `streamId` -- identifier of the stream on which to operate
- `index` -- index of the value to remove

Removes the value at index from the specified stream. Precision can matter when finding a value. If index is a DateTime, use the round-trip format given by `.ToString(“o”)`.

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

- `streamId` -- identifier of the stream from which to remove values
- `index` -- list of indices at which to remove values

Removes the value at each index from the specified stream. If any individual index has a problem, the enter list of attempted RemoveValues is rolled back and no removes are done.  The index that caused the issue can be determined in the error response.

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

- `streamId` -- identifier of the stream from which to remove values
- `startIndex` -- string representation of the starting index of the range
- `endIndex` -- string representation of the ending index of the range

Removes a range of values at and between the indices given.

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

- `streamId` -- identifier of the stream in which to replace values
- `items` -- list of new items to replace existing items in the stream

Writes items over existing values in the specified stream. Throws an exception if any index does not have a value to be replaced.

If any individual index has a problem doing replace, the enter list of attempted replacements is rolled back and no replaces at all are done. The index that caused the issue can be determined in the error response.

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

- `streamId` -- identifier of the stream in which to update a value
- `item` -- new value to replace an existing value

Writes item to specified stream.  Will insert at any index that does not have a value and will replace if the index already has a value. 

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

- `streamId` -- identifier of the stream in which to perform updates
- `items` -- list of new items to replace existing items

Writes items to specified stream.   Will insert or replace. If any individual index has a problem, the enter set of attempted UpdateValues is rolled back (no updates at all are done). The index that caused the issue can also be determined in the error response.
