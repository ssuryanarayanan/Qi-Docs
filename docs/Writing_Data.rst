Writing Data
============

Write exception handling
------------

If a method that acts upon multiple data events encounters a problem while carrying
out the operation, an exception is thrown and none of the list of
elements is acted upon. For example `InsertValues( 
) <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalues>`__
is called with a list of 100 events and one of the events uses an index
at which there is already data present. An exception is thrown and
all of the events are rolled back, resulting in no inserts for the
100 events. The event at which the error occurred is identified in
the exception.

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
