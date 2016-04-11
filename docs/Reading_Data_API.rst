API Calls for reading data
===========================

Reading and writing data with the Qi Client Libraries is performed through the ``IQiDataService`` interface, which can be accessed with the ``QiService.GetDataService( )`` helper.

``FindDistinct``
----------------

Searches for data in a stream using the specified search mode.


**Syntax**

::
 
    Task<T> FindDistinctValueAsync<T>(string streamId, string index, QiSearchMode mode);
    Task<T> FindDistinctValueAsync<T, T1>(string streamId, T1 index, QiSearchMode mode);
    Task<T> FindDistinctValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index, QiSearchMode mode);

**Http**

``GET Qi/{tenantId}/{tenantId}{namespaceId}/Streams/{streamId}/Data/FindDistinctValue?index={index}&mode={mode}``

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``string streamId``
  The stream identifier for the request.
``string index``
  String representation of the index value at which to search.
``QiSearchMode mode``
  Search mode (see **Notes** below).
  

**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  ``FindDistinctValue`` returns null if the data is not found.
  The mode parameter determines how the search for data is executed from the specified index. See the following table:

+-------------------+------------+-------------------------------------------------------------------+
|Search Mode        |Enumeration |Action                                                             |
|                   |Value       |                                                                   |
+===================+============+===================================================================+
|Exact              |1           |Returns data if found at the index, otherwise returns null         |      
+-------------------+------------+-------------------------------------------------------------------+
|ExactOrNext        |2           |Returns data if found at the index or searches forward for the     |
|                   |            |next index with data                                               |
+-------------------+------------+-------------------------------------------------------------------+
|ExactOrPrevious    |3           |Returns data if found at the index or searches for the first       |
|                   |            |previous index with data                                           |
+-------------------+------------+-------------------------------------------------------------------+
|Next               |4           |Searches forward (immediately after the index given) for the next  |
|                   |            |index with data                                                    |
+-------------------+------------+-------------------------------------------------------------------+
|Previous           |5           |Searches for the first previous index with data starting           |
|                   |            |immediately behind the index given                                 |
+-------------------+------------+-------------------------------------------------------------------+

**Examples**

Assume a type named ``TestType`` has been created (with a DateTime index
parameter named ``TimeId`` and a double data parameter called ``Value``).
Assume that the stream identified by streamId was created with
``TestType``.

The following example obtains the most recent event in the stream by
starting at the index ``Now`` and searching backwards until a value is
found. Note that if the stream is empty a null is returned in ``readEvent``:

::

    searchMode = QiSearchMode.ExactOrPrevious;
    string index = DateTime.Now.ToString(“o”);
    var readEvent = _dataService.FindDistinctValueAsync<TestType>(streamId, index, searchMode).Result;

The next example does the same thing as the previous example, while illustrating the use of the
generic overload allowing ``DateTime`` to be used directly as the index
instead of a string:

::

    searchMode = QiSearchMode.ExactOrPrevious;
    DateTime indexDT = DateTime.Now;
    var readEvent = _dataService.FindDistinctValueAsync<TestType, DateTime>(streamId, indexDT, searchMode).Result;

The next example uses tuples to indicate the index. Using tuples is useful for
stream types with a compound index, such as a DateTime and an Integer.

::

    searchMode = QiSearchMode.ExactOrPrevious;
    var tupleId = new Tuple<DateTime, int>(DateTime.Now, 0);
    var readEvent = _dataService.FindDistinctValueAsync<TestType, DateTime, int>(streamId, tupleId, searchMode).Result;


``GetDistinctValue``
----------------

Returns an event from the specified stream at the specified index.


**Syntax**

::

    Task<T> GetDistinctValueAsync<T>(string streamId, string index);
    Task<T> GetDistinctValueAsync<T, T1>(string streamId, T1 index);
    Task<T> GetDistinctValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetDistinctValue?index={index}

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.
``index``
  String representation of the index value at which to search.


**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  ``GetDistinctValue`` returns an event from the specified stream at
  the specified index. An exception is thrown if no event exists at index.

**Examples** 
  The following example obtains the event in the stream
  at the index defined by ``Now``. An exception is thrown if there is no event 
  at that index:

::

    string index = DateTime.Now.ToString(“o”);
    try
    {
        var readEvent = _dataService.GetDistinctValueAsync<TestType>(streamId, index).Result;
    }
    Catch (exception e)
    {
        //handle exception
    }

**Overloads**

**Task<T> GetDistinctValueAsync(string streamId, T1 index);**

Can be used to supply the index of the call as a different type.

**Task<T> GetDistinctValueAsync(string streamId, Tuple index);**

Can be used to supply the index of the call as a tuple (for compound
indexes).

See the `FindDistinctValue <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html#finddistinctvalue>`__
examples for an illustration of these.


``GetFirstValue``
----------------

Retrieves the first data event in a stream.


**Syntax**

::

    Task<T> GetFirstValueAsync<T>(string streamId);

**Http**

::

    GET Qi/{tenantId}/{tenantId}{namespaceId}/Streams/{streamId}/Data/GetFirstValue

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.


**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  ``GetFirstValue`` returns null if the stream has no data (no exception is thrown).


``GetLastValue``
----------------

Retrieves the last data event in a stream.


**Syntax**

::

    Task<T> GetLastValueAsync<T>(string streamId);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetLastValue

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.


**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  ``GetLastValue`` Returns null if the stream has no data (no exception is thrown).


``GetRangeValues``
----------------

Retrieves events from a stream based on a starting index and a requested number of events.


**Syntax**

::

    IEnumerable<T> GetRangeValuesAsync<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType, string filterExpression);
    Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int count);
    Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int count, bool reversed);
    Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int count, QiBoundaryType boundaryType);
    Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType);
    Task<IEnumerable<T>> GetRangeValuesAsync<T>(string streamId, string startIndex, int skip, int count, bool reversed, QiBoundaryType boundaryType, string filterExpression);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&reversed={reversed}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&boundaryType={boundaryType}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boun GET daryType={boundaryType}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boun GET daryType={boundaryType}&filterExpression={filterExpression}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&reversed={reversed}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&count={count}&boundaryType={boundaryType}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boun GET daryType={boundaryType}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetRangeValues?startIndex={startIndex}&skip={skip}&count={count}&reversed={reversed}&boundaryType={boundaryType}&filterExpression={filterExpression}

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.
``startIndex``
  String represntation of the starting index value.
``count``
  Maximum number of events to return.
``reversed``
  Order of event retrieval; true to retrieve events in reverse order.
``skip``
  Number of events to skip; skipped events are not returned or
  counted. (Applied after filterExpression. )
``boundaryType``
  Enumeration indicating how to handle boundary events.
``filterExpression``
  String containing an OData filter expression (see *Notes* section below).
  

**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  ``GetRangeValues`` is used to obtain events from a stream based on
a starting index and a requested number of events. Optionally, overloads allow
the client to specify search direction, number of events to
skip over, special boundary handling for **startIndex**, and an event
filter. Events returned by ``GetRangeValues`` are stored events, not
calculated events, with the exception of the starting event if
ExactOrCalculated is specified for ``boundaryType``.

``GetRangeValues`` searches FORWARD if the ``reverse`` parameter is
false and reverse if the ``reverse`` parameter is true. For overloads that
do not include the ``reverse`` parameter, the default is forward.

The ``skip`` parameter indicates the number of events that the call 
skips over before it collects events for the response.

BoundaryType has the following possible values: • Exact •
ExactOrCalculated • Inside • Outside

The BoundaryType determines how to specify the first value in from the
stream starting at the start index. This is also affected by the
direction of the method. The table below indicates how the first value
is determined for ``GetRangeValues`` for a FORWARD search of the
BoundaryTypes shown:

+--------------------------+-------------------------------------------------------------------------------+
| Boundary Type            | First value obtained                                                          |
+==========================+===============================================================================+
|Exact                     |The first value at or after the startIndex                                     |
+--------------------------+-------------------------------------------------------------------------------+
|ExactOrCalculated         |If a value exists at the startIndex it is used, otherwise a value is           |
|                          |‘calculated’ according to the Stream Behavior setting                          |
+--------------------------+-------------------------------------------------------------------------------+
|Inside                    |The first value after the startIndex                                           |
+--------------------------+-------------------------------------------------------------------------------+
|Outside                   |The first value before the startIndex                                         |
+--------------------------+-------------------------------------------------------------------------------+

The table below indicates how the first value is determined for
``GetRangeValues( )`` for a reverse search of the BoundaryTypes shown:

+--------------------------+-------------------------------------------------------------------------------+
| Boundary Type            | First value obtained                                                          |
+==========================+===============================================================================+
|Exact                     |The first value at or before the startIndex                                    |
+--------------------------+-------------------------------------------------------------------------------+
|ExactOrCalculated         |If a value exists at the startIndex it is used, otherwise a value is           |
|                          |‘calculated’ according to the Stream Behavior setting. See the                 |
|                          |*Calculated startIndex* topic below.                                           | 
+--------------------------+-------------------------------------------------------------------------------+
|Inside                    |The first value before the startIndex                                          |
+--------------------------+-------------------------------------------------------------------------------+
|Outside                   |The first value after the startIndex                                          |
+--------------------------+-------------------------------------------------------------------------------+

The order of execution first determines the direction of the method and
the starting event using the ``BoundaryType``. After the starting event is
determined, the filterExpression is applied in the direction requested
to determine potential return values. Then, ``skip`` is applied to pass
over the specified number of events, including any calculated events.
Finally, events up to the number specified by count are returned.

The filter expression uses OData query language. Most of the query
language is supported. More information about OData Filter Expressions can
be found in `Filter
expressions <http://qi-docs-rst.readthedocs.org/en/latest/Filter%20Expressions.html>`__

**Calculated startIndex** When the startIndex for ``GetRangeValues`` 
lands before, after, or in-between data in the stream, and the
ExactOrCalculated boundaryType is used, the stream behavior determines
whether an additional calculated event is created and returned in the
response.

The table below indicates when an event will be calculated and included
in the ``GetRangeValues`` response for a **startIndex** before or after
all data in the stream. (This data is for FORWARD search modes):

+--------------------------+--------------------------+------------------------------+------------------------------+
|Stream Behavior           |Stream Behavior           |When start index is           |When start index is           |
|Mode                      |QiStreamExtrapolation     |before all data               |after all data                |
+==========================+==========================+==============================+==============================+
|Continuous                |All                       |Event is calculated*          |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |Event is calculated*          |No event calculated           |
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
|                          |Forward                   |No event calculated           |Event is calculated*          |
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
|ContinuousLeading      |Event is calculated using the index and previous event values            |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousTrailing     |Event is calculated using the index and next event values                 |
+-----------------------+--------------------------------------------------------------------------+


``GetValue``
----------------

Retrieves a specified data event from a stream.


**Syntax**

::

    Task<T> GetValueAsync<T>(string streamId, string index);
    Task<T> GetValueAsync<T, T1>(string streamId, T1 index);
    Task<T> GetValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetValue?index={index}

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.
``index``
  String representation of the index value for GetValue or IEnumerable of index
  values requested for GetValues.
  

**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  If there is a value at the index, the call returns that event.

If the specified index is before or after all events, the value returned
with that index is determined by the stream behavior (specifically, the
stream behavior extrapolation setting).

If the specified index is between events, the event returned is
determined by the stream behavior and any behavior overrides.

If the stream contains no data, null is returned regardless of the
stream behavior.

**Examples** The following example obtains the event in the stream
at the index defined by ``Now``. If no event exists at that index the
result is determined by the stream behavior.

::

    string index = DateTime.Now.ToString(“o”);
    try
    {
        var  readEvent = _dataService.GetValue<TestType>(string tenandId, namespaceId, streamId, index);
    }
    Catch (exception e)
    {
        //handle exception
    }

**Overloads**

**Task<T> GetValueAsync<T, T1>(string streamId, T1 index);**

Can be used to supply the index of the call as a different type

**    Task<T> GetValueAsync<T, T1, T2>(string streamId, Tuple<T1, T2> index);**

Can be used to supply the index of the call as a tuple (for compound indexes)

See the `FindDistinctValue <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html#finddistinctvalue>`__
examples for an illustration of these.


``GetValues``
----------------

Retrieves calculated events at the requested index values in **index**, or **count** number of evenly spaced calculated events between **startIndex** and **endIndex**.


**Syntax**

::

    Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, IEnumerable<string> index);
    Task<IEnumerable<T>> GetValuesAsync<T, T1>(string streamId, IEnumerable<T1> index);
    Task<IEnumerable<T>> GetValuesAsync<T, T1, T2>(string streamId, IEnumerable<Tuple<T1, T2>> index);
    Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, string filterExpression);
    Task<IEnumerable<T>> GetValuesAsync<T>(string streamId, string startIndex, string endIndex, int count);
    Task<IEnumerable<T>> GetValuesAsync<T, T1>(string streamId, T1 startIndex, T1 endIndex, int count);
    Task<IEnumerable<T>> GetValuesAsync<T, T1, T2>(string streamId, Tuple<T1, T2> startIndex, Tuple<T1, T2> endIndex, int count);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetValues?startIndex={startIndex}&endIndex={endIndex}&count={count}

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.
``index``
  IEnumerable of index values at which to return calculated events.
``startIndex``
  String representation of the starting index value.
``endIndex``
  String representation of the ending index value.
``count``
  Number of equally-spaced calculated events to return within the *startIndex* and *endIndex* boundaries.

  

**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**
  ``GetValues`` returns calculated events at the requested
  index values in **index**, or **count** number of evenly spaced calculated
  events between **startIndex** and **endIndex**. For ``GetValues`` overloads
  that include a streamId and IEnumberable **index**, the call behaves like
  multiple ``GetValue`` calls. For the ``GetValues`` overloads that
  include **startIndex**, **endIndex** and **count**, these parameters are used
  to generate a list of indexes for which to obtain values. Events
  returned for each index are determined according to the QiStreamBehavior
  assigned to the stream being read.

For ``GetValues`` overloads that include the filterExpression
parameters are used to create a list of indexes that match the OData
filter text used. More information on OData Filter Expressions can be
found in `Filter
expressions <http://qi-docs-rst.readthedocs.org/en/latest/Filter%20Expressions.html>`__


``GetWindowValues``
----------------

Retrieves values between the specified start and end indexes.


**Syntax**

::

    Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex);
    Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType);
    Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, string filterExpression);
    Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, QiBoundaryType startBoundaryType, string endIndex, QiBoundaryType endBoundaryType, string filterExpression);
    Task<QiResultPage<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, int count, string continuationToken);
    Task<IEnumerable<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, QiBoundaryType startBoundaryType, string endIndex, QiBoundaryType endBoundaryType, string filterExpression, string selectExpression);
    Task<QiResultPage<T>> GetWindowValuesAsync<T>(string streamId, string startIndex, string endIndex, QiBoundaryType boundaryType, string filterExpression, int count, string continuationToken);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&endIndex={endIndex}&boundaryType={boundaryType}&filterExpression={filterExpression}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&startBoundaryType={startBoundaryType}&endIndex={endIndex}&endBoundaryType={endBoundaryType}&filterExpression={filterExpression}&selectExpression={selectExpression}
    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Data/GetWindowValues?startIndex={startIndex}&&endIndex={endIndex}&boundaryType={boundaryType}&count={count}&continuationToken={continuationToken}

	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The stream identifier for the request.
``startIndex``
  String representation of the starting index value, must be less than **endIndex**.
``endIndex``
  String representation of the ending index value.
``boundaryType``
  Enumeration describing how to handle boundary events.
``filterExpression``
  OData filter expression.
``count``
  Maximum of events to return within the specified index range. For paging through data.
``continuationToken``
  Continuation token for handling multiple return data sets.
``startBoundaryType``
  How to handle startIndex boundary events.
``endBoundaryType``
  How to handle endIndex boundary events.
``selectExpression``
  Expression designating which fields of the stream's type should make up the return events.

  
**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts
  
**Notes**

``GetWindowValues`` returns stored events within a
specified index range. If **count** and **continuationToken** are used, up
to **count** events are returned within the specified index range along
with a continuation token that may be passed into a subsequent
``GetWindowValues`` call to obtain the next **count** events. Note that
**count** need not stay the same through multiple ``GetWindowValues( )``
calls with **continuationToken**.

Boundary events at or near **startIndex** and **endIndex** are handled
according to **boundaryType** or **startBoundaryType** and
**endBoundaryType**, which have the following possible values: • Exact •
ExactOrCalculated • Inside • Outside

The table below indicates how the first value is determined for
``GetWindowValues`` for the **startBoundaryType** shown:


+----------------------+-----------------------------------------------------------------------------+
|*startBoundaryType*   |First value obtained                                                         |
+======================+=============================================================================+
|Exact                 |The first value at or after the startIndex                                   |
+----------------------+-----------------------------------------------------------------------------+
|ExactOrCalculated     |If a value exists at the startIndex it is used, else a value is calculated   |
|                      |according to the stream's behavior setting                                   |
+----------------------+-----------------------------------------------------------------------------+
|Inside                |The first value after the startIndex                                        |
+----------------------+-----------------------------------------------------------------------------+
|Outside               |The first value before the startIndex                                       |
+----------------------+-----------------------------------------------------------------------------+

This chart indicates how the last value is determined for
``GetWindowValues`` for the **endBoundaryType** shown:

+----------------------+-----------------------------------------------------------------------------+
|*endBoundaryType*     |First value obtained                                                         |
+======================+=============================================================================+
|Exact                 |The first value at or before the endIndex                                    |
+----------------------+-----------------------------------------------------------------------------+
|ExactOrCalculated     |If a value exists at the endIndex it is used, else a value is calculated     |
|                      |according to the stream's behavior setting                                   |
+----------------------+-----------------------------------------------------------------------------+
|Inside                |The first value before the endIndex                                         |
+----------------------+-----------------------------------------------------------------------------+
|Outside               |The first value after the endIndex                                          |
+----------------------+-----------------------------------------------------------------------------+

Calls against an empty stream always return a single null
regardless of boundary type used.

The filter expression uses OData syntax. More information on OData
Filter Expressions can be found in `Filter
expressions <http://qi-docs-rst.readthedocs.org/en/latest/Filter%20Expressions.html>`__

The select expression is a CSV list of strings that indicate which fields
of the stream type are being requested. By default all type fields are
included in the response. Select may improve the performance of the call
by avoiding management of the unneeded fields. Note that the index is
always included in the returned results.

Selection is applied before filtering; therefore, any fields that are used in the filter
expression must be included by the select statement.

**Calculated startIndex and endIndex** When the startIndex or endIndex
of ``GetWindowValues`` does not fall on an event in the stream, and the
**boundaryType** of ExactOrCalculated is used, an event may be created and
returned in the GetWindowValues call response.

The table below indicates when a calculated event is created for
indexes before or after stream data:

+--------------------------+--------------------------+------------------------------+------------------------------+
|QiStreamBehavior          |QiStreamBehavior          |When start index is           |When start index is           |
|*Mode*                    |*ExtrapolationMode*       |before all data               |after all data                |
+==========================+==========================+==============================+==============================+
|Continuous                |All                       |Event is calculated*          |Event is calculated*          |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |None                      |No event calculated           |No event calculated           |
+--------------------------+--------------------------+------------------------------+------------------------------+
|                          |Backward                  |Event is calculated*          |No event calculated           |
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
|                          |Forward                   |No event calculated           |Event is calculated*          |
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
returned along with the result of the *GetWindowValues* call.

If an index (startIndex or endIndex) in ``GetWindowValues`` lands
between data in the stream, and the BoundaryT Type is set to
ExactOrCalculated, and event is created according to the following
table:

+-----------------------+--------------------------------------------------------------------------+
|Stream Behavior        |Calculated Event                                                          |
|Mode                   |                                                                          |
+=======================+==========================================================================+
|Continuous             |The event is calculated using the index and a value that is interpolated  |
|                       |from the surrounding index values.                                        |
+-----------------------+--------------------------------------------------------------------------+
|Discrete               |No event is calculated.                                                   |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousLeading      |The event is calculated using the index and the previous event values.    |
+-----------------------+--------------------------------------------------------------------------+
|ContinuousTrailing     |Event is calculated using the index and next event values                 |
+-----------------------+--------------------------------------------------------------------------+


