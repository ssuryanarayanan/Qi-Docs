Writing Data
============

The Qi API library includes an assortment of methods that are used to write, replace, and remove data from streams.
Methods can be used to operatte on one or more data events in a single call. All methods that write or manipulate data values are  allowed only by the administrator security accounts.


Write exception handling
------------

If a method that acts upon multiple data events encounters a problem while carrying
out the operation, an exception is thrown and none of the list of
elements is acted upon. For example, when `InsertValues( 
) <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalues>`__
is called with a list of events and one of the events uses an index
at which data is already present, an exception is thrown and
all of the events are rolled back. The event at which the error occurred is identified in
the exception and no inserts are performed.

For example:

::

    {
      _service.InsertValues(namespaceId, streamId, writeEvents);
    }
    catch (QiHttpClientException e)
    {
        :
      //  e.Errors.Values[0] indicates the streamId of the exception
      //  e.Errors.Values[1] indicates the TimeId of the exception
        :
    }
