Reading Data
============

Generics and tuples
------------

Many of the read methods in the Qi Client Library have additional
overloads that assist with indexing into a stream when it is
inconvenient or difficult to convert the index into a string. Using
generics, the overloads allow the caller to indicate the type of the
index and then use the type for the index parameter(s) in the call.
Similarly the read methods have overloads that use tuples that are also
accepted for the indexing instead of a string.

This example uses *GetRangeValues( )* to return a list of events (up to
100) from streamId starting 30 minutes ago:

::

    List< SimpleTypeClass > readEvents;
    String startindex = DateTime.UtcNow.AddMinutes(-30).ToString("o");
    readEvents = _service.GetRangeValues< SimpleTypeClass >(streamId, startindex, 100).ToList();

*GetRangeValues( )* also has overloads that include a *skip* parameter
which makes multiple calls and retrieves different sets of data after a
specified time stamp.

FindDistinctValue( )
------------

**Qi Client Library**

::

    T FindDistinctValue<T>(string streamId, string index, QiSearchMode mode);
    T FindDistinctValue<T, T1>(string streamId, T1 index, QiSearchMode mode);
    T FindDistinctValue<T, T1, T2>(string streamId, Tuple<T1, T2> index, QiSearchMode) 
    Task<T> FindDistinctValueAsync<T>(string streamId, string index, QiSearchMode mode);
    Task<T> FindDistinctValueAsync<T, T1>(string streamId, T1 index, QiSearchMode mode);
    Task<T> FindDistinctValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index, QiSearchMode mode);

**Http**
``GET Qi/Streams/{streamId}/Data/FindDistinctValue?index={index}&mode={mode}``

**Parameters**

*streamId*: Stream identifier for the request

*index*: String representation of the index value at which to search

*mode*: Search mode (see **Operation** below)

**Security** Allowed by administrator and user accounts

**Operation** This method searches for data in a stream using the search mode defined. If a data is not found a null is returned. The mode parameter determines how the search for data is executed:

+-------------------+------------+-------------------------------------------------------------------+
|Search Mode        |Enumeration |Action                                                             |
|                   |Value       |                                                                   |
+===================+============+===================================================================+
|Exact              |1           |Returns a data if found at the index, else null is returned        |      
+-------------------+------------+-------------------------------------------------------------------+
|ExactOrNext        |2           | Returns a data if found at the index or searches forward for the  |
|                   |            |next index with data                                               |
+-------------------+------------+-------------------------------------------------------------------+
|ExactOrPrevious    |3           |Returns a data if found at the index or searches for the first     |
|                   |            |previous index with data                                           |
+-------------------+------------+-------------------------------------------------------------------+
|Next               |4           |Searches forward (immediately after the index given) for the next  |
|                   |            |index with data                                                    |
+-------------------+------------+-------------------------------------------------------------------+
|Previous           |5           |Searches for the first previous index with data starting           |
|                   |            |immediately behind the index given                                 |
+-------------------+------------+-------------------------------------------------------------------+

**Examples**

Assume a type named "TestType" has been created (with a DateTime index
parameter named "TimeId" and a double data parameter called "Value").
Assume that the stream identified by streamId was created with
"TestType".

The following example will obtain the most recent event in the stream by
starting at the index ‘Now’ and searching backwards until a value is
found. Note if the stream is empty a null will be returned in readEvent:

::

    searchMode = QiSearchMode.ExactOrPrevious;
    string index = DateTime.Now.ToString(“o”);
    var  readEvent = _service.FindDistinctValue<TestType>(streamId, index, searchMode);

The next example does the same thing, while illustrating the use of the
generic overload allowing DateTime to be used directly as the index
instead of a string:

::

    searchMode = QiSearchMode.ExactOrPrevious;
    DateTime indexDT = DateTime.Now;
    var  readEvent = _service.FindDistinctValue<TestType, DateTime>(streamId, indexDT, searchMode);

The next example uses tuples to indicate the index. This is useful for
stream types with a compound index, such as a DateTime and an Integer.
then this call

::

    searchMode = QiSearchMode.ExactOrPrevious;
    var tupleId = new Tuple<DateTime, int>(DateTime.Now, 0);
    var  readEvent = _service.FindDistinctValue<TestType, DateTime, int>(streamId, tupleId, searchMode);

GetDistinctValue( )
------------

**Qi Client Library**

::

    T GetDistinctValue<T>(string streamId, string index);
    T GetDistinctValue<T, T1>(string streamId, T1 index);
    T GetDistinctValue<T, T1, T2>(string streamId, Tuple<T1, T2> index);
    Task<T> GetDistinctValueAsync<T>(string streamId, string index);
    Task<T> GetDistinctValueAsync<T, T1>(string streamId, T1 index);
    Task<T> GetDistinctValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);

**Http**

::

    GET Qi/Streams/{streamId}/Data/GetDistinctValue?index={index}

**Parameters**

*streamId*: Stream identifier for the request

*index*: String representation of the index value at which to search

**Security** Allowed by administrator and user accounts

**Operation** This method returns an event from the specified stream at
the specified index An exception is thrown if no event exists at index

**Examples** The following example will obtain the event in the stream
at the index defined by ‘Now’. If there is no event at that index an
exception will be thrown:

::

    string index = DateTime.Now.ToString(“o”);
    try
    {
        var  readEvent = _service.GetDistinctValue<TestType>(streamId, index);
    }
    Catch (exception e)
    {
        //handle exception
    }

**Overloads**

**T GetDistinctValue(string streamId, T1 index);**

Can be used to supply the index of the call as a different type.

**T GetDistinctValue(string streamId, Tuple index);**

Can be used to supply the index of the call as a tuple (for compound
indexes).

See the `*FindDistinctValue(
)* <http://qi-docs.osisoft.com/en/latest/Reading%20data/#finddistinctvalue>`__
examples for an illustration of these.

GetFirstValue( )
------------

**Qi Client Library**

::

    T GetFirstValue<T>(string streamId);
    Task<T> GetFirstValueAsync<T>(string streamId);

**Http**

::

    GET Qi/Streams/{streamId}/Data/GetFirstValue

**Parameters**

*streamId*: Stream identifier for the request

**Security** Allowed by administrator and user accounts

**Operation** Returns the first data event in the stream Returns null if
the stream has no data (no exception thrown)

GetLastValue( )
------------

**Qi Client Library**

::

    T GetLastValue<T>(string streamId);
    Task<T> GetLastValueAsync<T>(string streamId);

**Http**

::

    GET Qi/Streams/{streamId}/Data/GetLastValue

**Parameters**

*streamId*: Stream identifier for the request

**Security** Allowed by administrator and user accounts

**Operation** Returns the last data event in the stream Returns null if
the stream has no data (no exception thrown)

GetRangeValues( )
------------

**Qi Client Library**

::

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

**Http**

::

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

**Parameters**

*streamId*: Stream identifier for the request

*startIndex*: String representation of the starting index value

*count*: Maximum number of events to return

*reversed*: Order of event retrieval; true to retrieve events in reverse
order

*skip*: Number of events to skip; skipped events are not returned or
counted. (Applied after filterExpression. )

*boundaryType*: Enumeration indicating how to handle boundary events

*filterExpression*: String containing an OData filter expression (see
*Operation* section below)

**Security** Allowed by administrator and user accounts

**Operation** This call is used to obtain events from a stream based on
a starting index and a requested number of events. The overloads allow
the client to optionally specify search direction, number of events to
skip over, special boundary handling for *startIndex*, and an event
filter. Events returned by *GetRangeValues( )* are stored events, not
calculated events, with the exception of the starting event if
ExactOrCalculated is specified for *boundaryType*.

*GetRangeValues( )* will search FORWARD if the ‘reverse’ parameter is
false and REVERSE if the ‘reverse’ parameter is true. For overloads that
do not include the ‘reverse’ parameter, the default is FORWARD.

The *skip* parameter indicates the number of events that the call will
skip over before it collects events for the response.

BoundaryType has the following possible values: • Exact •
ExactOrCalculated • Inside • Outside

The BoundaryType determines how to determine the first value in from the
stream starting at the start index. This is also effected by the
direction of the method. The table below indicates how the first value
is determined for *GetRangeValues( )* for a FORWARD search of the
BoundaryTypes shown:

+--------------------------+-------------------------------------------------------------------------------+
| Boundary Type            | First value obtained                                                          |
+==========================+===============================================================================+
|Exact                     |The first value at or after the startIndex                                     |
+--------------------------+-------------------------------------------------------------------------------+
|ExactOrCalculated         |If a value exists at the startIndex it is used, else a value is ‘calculated’   |
|                          |according to the Stream Behavior setting                                       |
+--------------------------+-------------------------------------------------------------------------------+
|Inside                    |The first value after the startIndex                                           |
+--------------------------+-------------------------------------------------------------------------------+
|Outside                   | The first value before the startIndex                                         |
+--------------------------+-------------------------------------------------------------------------------+

The table below indicates how the first value is determined for
*GetRangeValues( )* for a REVERSE search of the BoundaryTypes shown:

+--------------------------+-------------------------------------------------------------------------------+
| Boundary Type            | First value obtained                                                          |
+==========================+===============================================================================+
|Exact                     |The first value at or before the startIndex                                    |
+--------------------------+-------------------------------------------------------------------------------+
|ExactOrCalculated         |If a value exists at the startIndex it is used, else a value is ‘calculated’   |
|                          |according to the Stream Behavior setting. See the Calculated startIndex topic  |
|                          |below.                                                                         | 
+--------------------------+-------------------------------------------------------------------------------+
|Inside                    |The first value before the startIndex                                          |
+--------------------------+-------------------------------------------------------------------------------+
|Outside                   | The first value after the startIndex                                          |
+--------------------------+-------------------------------------------------------------------------------+

The order of execution first determines the direction of the method and
the starting event using the *BoundaryType*. Once the starting event is
determined, the filterExpression is applied in the direction requested
to determine potential return values. Then, *skip* is applied to pass
over the specified number of events, including any calculated events.
Finally, events up to the number specified by count are returned.

The filter expression uses OData query language. Most of the query
language is supported. More information on OData Filter Expressions can
be found in `Filter
expressions <http://qi-docs.osisoft.com/en/latest/Filter%20Expressions/>`__

**Calculated startIndex** When the startIndex for *GetRangeValues( )*
lands before, after or in-between data in the stream, and the
ExactOrCalculated *boundaryType* is used the stream behavior determines
whether an additional ‘calculated’ event is created and returned in the
response.

The table below indicates when an event will be calculated and included
in the *GetRangeValues( )* response for a *startIndex* before or after
all data in the stream. (This is for FORWARD search modes):

+--------------------------+--------------------------+------------------------------+------------------------------+
|Stream Behavior           |Stream Behavior           |When start index is           |When start index is           |
|Mode                      |QiStreamExtrapolation     |before all data               |after all data                |
+==========================+==========================+==============================+==============================+
|Continuous                |All                       |Event is calculated*          |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |Event is calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|Discrete                  |All                       |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|ContinuousLeading         |All                       |No event calculated           |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|ContinuousTrailing        |All                       |Event is calculated*          |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |Event is calculated*          |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+

::

            *Events is calculated using startIndex and the value of the first event

When the startIndex falls between data:

+-----------------------+--------------------------------------------------------------------------+
|Stream Behavior        |Calculated Event                                                          |
|Mode                   |                                                                          |
+=======================+==========================================================================+
|Continuous             |Event is calculated using the index and a value interpolated from the     |
|                       |surrounding index values                                                  |
+-----------------------+--------------------------------------------------------------------------+
|Discrete               |No event calculated                                                       |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousLeading      | Event is calculated using the index and previous event values            |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousTrailing     |Event is calculated using the index and next event values                 |
+-----------------------+--------------------------------------------------------------------------+

GetValue( )
------------

**Qi Client Library**

::

    T GetValue<T>(string streamId, string index);
    T GetValue<T, T1>(string streamId, T1 index);
    T GetValue<T, T1, T2>(string streamId, Tuple<T1, T2> index);
    Task<T> GetValueAsync<T>(string streamId, string index);
    Task<T> GetValueAsync<T, T1>(string streamId, T1 index);
    Task<T> GetValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);

**Http**

::

    GET Qi/Streams/{streamId}/Data/GetValue?index={index}

**Parameters**

*streamId*: Stream identifier for the request *index*: String
representation of the index value for GetValue or IEnumerable of index
values requested for GetValues

**Security** Allowed by administrator and user accounts

**Operation** If there is a value at the index, the call will return
that event.

If the specified index is before or after all events, the value returned
with that index is determined by the stream behavior (specifically the
stream behavior extrapolation setting).

If the specified index is between events, the event returned is
determined by the stream behavior and any behavior overrides.

If the stream contains no data, null is returned regardless of the
stream behavior.

**Examples** The following example will obtain the event in the stream
at the index defined by ‘Now’. If there is no event at that index the
result will be determined by the stream behavior.

::

    string index = DateTime.Now.ToString(“o”);
    try
    {
        var  readEvent = _service.GetValue<TestType>(streamId, index);
    }
    Catch (exception e)
    {
        //handle exception
    }

**Overloads**

**T GetValue(string streamId, T1 index);**

Can be used to supply the index of the call as a different type

**T GetValue(string streamId, Tuple index);**

Can be used to supply the index of the call as a tuple (for compound
indexes)

See the `*FindDistinctValue(
)* <http://qi-docs.osisoft.com/en/latest/Reading%20data/#finddistinctvalue>`__
examples for an illustration of these.

GetValues( )
------------

**Qi Client Library**

::

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

**Http**

::

    GET Qi/Streams/{streamId}/Data/GetValues?startIndex={startIndex}&endIndex={endIndex}&count={count}

**Parameters**

*streamId*: Stream identifier for the request

*index*: IEnumerable of index values at which to return calculated
events

*startIndex*: String representation of the starting index value

*endIndex*: String representation of the ending index value

*count*: Number of equally-spaced calculated events to return within the
*startIndex* and *endIndex* boundaries

**Security ** Allowed by administrator and user accounts

**Operation** *GetValues( )* returns calculated events at the requested
index values in *index*, or *count* number of evenly spaced calculated
events between *startIndex* and *endIndex*. For *GetValues( )* overloads
that include a streamId and IEnumberable *index*, this call behaves like
multiple *GetValue( )* calls. For the *GetValues( )* overloads that
include *startIndex*, *endIndex* and *count*, these parameters are used
to generate a list of indexes for which to obtain values. Events
returned for each index are determined according to the QiStreamBehavior
assigned to the stream being read.

For *GetValues( )* overloads that include the filterExpression
parameters are used to create a list of indexes that match the OData
filter text used. More information on OData Filter Expressions can be
found in `Filter
expressions <http://qi-docs.osisoft.com/en/latest/Filter%20Expressions/>`__

GetWindowValues( )
------------

**Qi Client Library**

::

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

**Http**

::

    GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}
    GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}
    GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}&filterExpression={filterExpression}
    GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}
    GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&startBoundaryType={startBoundaryType}&endIndex={endIndex}&endBoundaryType={endBoundaryType}&filterExpression={filterExpression}&selectExpression={selectExpression}
    GET Qi/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}

**Parameters**

*streamId*: Stream identifier for the request

*startIndex*: String representation of the starting index value, must be
less than *endIndex*

*endIndex*: String representation of the ending index value

*boundaryType*: Enumeration describing how to handle boundary events

*filterExpression*: OData filter expression

*count*: Maximum of events to return within the specified index range.
For paging through data.

*continuationToken*: Continuation token for handling multiple return
data sets

*startBoundaryType*: How to handle startIndex boundary events

*endBoundaryType*: How to handle endIndex boundary events

*selectExpression*: Expression designating which fields of the stream's
type should make up the return events

**Security** Allowed by administrator and user accounts

**Operation** *GetWindowValues( )* returns stored events within a
specified index range. If *count* and *continuationToken* are used, up
to *count* events are returned within the specified index range along
with a continuation token that may be passed into a subsequent
*GetWindowValues( )* call to obtain the next *count* events. Note that
*count* need not stay the same through multiple *GetWindowValues( )*
calls with *continuationToken*.

Boundary events at or near *startIndex* and *endIndex* are handled
according to *boundaryType* or *startBoundaryType* and
*endBoundaryType*, which have the following possible values: • Exact •
ExactOrCalculated • Inside • Outside

The table below indicates how the first value is determined for
*GetWindowValues ( )* for the *startBoundaryType* shown:


+----------------------+-----------------------------------------------------------------------------+
|*startBoundaryType*   |First value obtained                                                         |
+======================+=============================================================================+
|Exact                 |The first value at or after the startIndex                                   |
+----------------------+-----------------------------------------------------------------------------+
|ExactOrCalculated     |If a value exists at the startIndex it is used, else a value is ‘calculated’ |
|                      |according to the stream's behavior setting                                   |
+----------------------+-----------------------------------------------------------------------------+
|Inside                | The first value after the startIndex                                        |
+----------------------+-----------------------------------------------------------------------------+
|Outside               | The first value before the startIndex                                       |
+----------------------+-----------------------------------------------------------------------------+

This chart indicates how the last value is determined for
*GetWindowValues( )* for the *endBoundaryType* shown:

+----------------------+-----------------------------------------------------------------------------+
|*endBoundaryType*     |First value obtained                                                         |
+======================+=============================================================================+
|Exact                 |The first value at or before the endIndex                                    |
+----------------------+-----------------------------------------------------------------------------+
|ExactOrCalculated     |If a value exists at the endIndex it is used, else a value is ‘calculated’   |
|                      |according to the stream's behavior setting                                   |
+----------------------+-----------------------------------------------------------------------------+
|Inside                | The first value after the endIndex                                          |
+----------------------+-----------------------------------------------------------------------------+
|Outside               | The first value before the endIndex                                         |
+----------------------+-----------------------------------------------------------------------------+

Calls against an empty stream will always return a single null
regardless of boundary type used.

The filter expression uses OData syntax. More information on OData
Filter Expressions can be found in `Filter
expressions <http://qi-docs.osisoft.com/en/latest/Filter%20Expressions/>`__

The select expression is a CSV list of strings that indicate which field
of the stream type are being requested. By default all type fields are
included in the response. Select may improve the performance of the call
by avoiding management of the unneeded fields. Note that the index is
always included in the returned results.

Selection is applied before filtering, so any fields used in the filter
expression must be included by the select statement.

**Calculated startIndex and endIndex** When the startIndex or endIndex
of *GetWindowValues( )* does not fall on an event in the stream, and the
*boundaryType* of ExactOrCalculated is used, an event may be created and
returned in the GetWindowValues call response.

The table below indicates the when a calculated event is created for
indexes before or after stream data:

+--------------------------+--------------------------+------------------------------+------------------------------+
|QiStreamBehavior          |QiStreamBehavior          |When start index is           |When start index is           |
|*Mode*                    |*ExtrapolationMode*       |before all data               |after all data                |
+==========================+==========================+==============================+==============================+
|Continuous                |All                       |Event is calculated*          |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |Event is calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|Discrete                  |All                       |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|ContinuousLeading         |All                       |No event calculated           |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|ContinuousTrailing        |All                       |Event is calculated*          |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |Event is calculated*          |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Forward                   |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+



\*When a startIndex event is calculated, the created event has the
startIndex and the value of the first data event in the stream. When an
endIndex is calculated, the created event uses the endIndex along with
the value from the stream’s last data event. Any calculated events are
returned along with the result of the *GetWindowValues( )* call.

If an index (startIndex or endIndex) in *GetWindowValues( )* lands
between data in the stream, and the BoundaryT Type is set to
ExactOrCalculated, and event will be created according to the following
chart:

+-----------------------+--------------------------------------------------------------------------+
|Stream Behavior        |Calculated Event                                                          |
|Mode                   |                                                                          |
+=======================+==========================================================================+
|Continuous             |Event is calculated using the index and a value interpolated from the     |
|                       |surrounding index values                                                  |
+-----------------------+--------------------------------------------------------------------------+
|Discrete               |No event calculated                                                       |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousLeading      | Event is calculated using the index and previous event values            |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousTrailing     |Event is calculated using the index and next event values                 |
+-----------------------+--------------------------------------------------------------------------+
