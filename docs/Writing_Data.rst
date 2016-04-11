Writing Data
============

The Qi API library includes an assortment of methods that are used to write, replace, and remove data from streams.
Methods can be used to operate on one or more data events in a single call. All methods that write or manipulate data values are  allowed only by the administrator security accounts.

Reading and writing data with the Qi Client Libraries is performed through the ``IQiDataService`` interface, which can be accessed with the ``QiService.GetDataService( )`` helper.


Write exception handling
------------

If a method that acts upon multiple data events encounters a problem while carrying
out the operation, an exception is thrown and none of the list of
elements is acted upon. For example, when `InsertValues <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#insertvalues>`__
is called with a list of events and one of the events uses an index
at which data is already present, an exception is thrown and
all of the events are rolled back. The event at which the error occurred is identified in
the exception and no inserts are performed.

For example:

::

    {
      await _dataService.InsertValuesAsync(streamId, writeEvents);
    }
    catch (QiHttpClientException e)
    {
        :
      //  e.Errors.Values[0] indicates the streamId of the exception
      //  e.Errors.Values[1] indicates the TimeId of the exception
        :
    }
