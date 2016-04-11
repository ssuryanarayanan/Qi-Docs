Reading Data
============

The QI API includes an assortment of methods for reading of data from streams.  

- To read data between two indexes, use GetWindowValues.
- To read defined elements from a start index, use GetRangeValues
- To filter the data, use GetRangeValues
- To use multiple calls with ‘continuation’, use GetWindowValues , GetRangeValues
- To read at individual indexes, use GetFirstValue, GetLastValue, GetValue, FindDiscreteValue, etc….
- To search in reverse, use GetRangeValues

Reading and writing data with the Qi Client Libraries is performed through the ``IQiDataService`` interface, which can be accessed with the ``QiService.GetDataService( )`` helper.

Generics and tuples
------------

Many of the read methods in the Qi Client Library have additional
overloads that assist with indexing into a stream when it is
inconvenient or difficult to convert the index into a string. Using
generics, the overloads allow the caller to indicate the type of the
index and then use the type for the index parameter(s) in the call.
Similarly the read methods have overloads that use tuples that are also
accepted for the indexing instead of a string.

The following example uses ``GetRangeValuesAsync( )`` to return a list of up to 100 events
from streamId starting 30 minutes ago:

::

    List< SimpleTypeClass > readEvents;
    String startindex = DateTime.UtcNow.AddMinutes(-30).ToString("o");
    readEvents = _dataService.GetRangeValuesAsync< SimpleTypeClass >(streamId, startindex, 100).Result.ToList();

``GetRangeValuesAsync( )`` also has overloads defined that include a *skip* parameter
which makes multiple calls and retrievals from different sets of data after a
specified time stamp.
