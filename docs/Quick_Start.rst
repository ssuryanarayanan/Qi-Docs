Quick start
###########

Step 1: Create a Namespace
--------------------------

You start by creating a Namespace so that you have a place in which to create types, 
streams, and behaviors.

::

   // create a Namespace ‘Namespace1’
   QiNamespace qinamespace = new QiNamespace(“Namespace1”);
   await _adminService.GetOrCreateNamespaceAsync(qinamespace);


Step 2: Create data types
------------------------

A QiType consists of one or more index properties and one or more
data properties. You use index properties to arrange data into a sequence.
For example, DateTime is a common index property, but any native type can be used as
an index as long as it allows for ordering of values. For information about
compound indexes refer to:
`QiTypes <https://qi-docs.readthedocs.org/en/latest/QiTypes/#compound-indexes>`__.

You define the structure of your data by defining a QiType object and then
sending it to Qi using `GetOrCreateType <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Types_API.html#getorcreatetype>`__
method.

A wide variety of QiType data properties are available, 
including lists, arrays and enumerations. For additional information,
including a detailed list of supported data types, refer to
`QiTypes <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Types.html>`__.

The following example creates a simple type:

::

    public class SimpleTypeClass 
    {
      public DateTime TimeId
      {
        get;
        set;
      }
      public double Value
      {
        get;
        set;
      }
    }

    // create type
    QiType simpleType = QiTypeBuilder.CreateQiType<SimpleTypeClass>();
    var mySimpleType = await _metadataService.GetOrCreateTypeAsync(simpleType);

Step 3: Create a stream
-----------------------

You use streams to hold data of a predefined type. To create a QiStream
the Id and TypeId of the stream must be defined. Optionally, you can also
define a Name, Description, and BehaviorID.

The following example creates a QiStream with an Id of ‘MyFistStream’ of type
‘mySimpleType’:

::

    string streamId = "MyFirstStream";
    string streamType = " mySimpleType ";
    QiStream stream1 = new QiStream()
    {
        Id = streamId,
        TypeId = streamType
    }
    
    await _metadataService.GetOrCreateStreamAsync(stream1);

The stream in the previous example can now be used to hold data values of 
the structure that is defined in mySimpleType. The stream's Name, 
Description, and BehaviorId fields can be modified; however, the Id 
and TypeId cannot be changed after the stream has been created.

Additional information about streams can be found in
`QiStreams <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Streams.html>`__.

**To define Stream behaviors**

Qi Stream Behaviors are applied to streams to affect how certain data
read operations are performed. The Stream Behavior object affects whether
interpolation or extrapolation will be done when the
index of a read operation falls between, before, or after stream data.

Additonal information about stream behaviors can be found in
`QiStreamBehaviors <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Stream_Behavior.html>`__.


Step 4: Write data
------------------

Qi has several methods that can be used to write data. For example,
`InsertValue <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#insertvalue>`__
is used to write a single data event to a stream. If the data event
includes an index at that is the same as a previous event, 
this method will throw an exception. However
`UpdateValue <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#updatevalue>`__
can also be used to write a single event to a stream, and will overwrite
the existing event with the new event.

Each of these methods has a counterpart that acts upon a list of data
events instead of only a single event. For example,
`InsertValues <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#insertvalues>`__
writes multiple events. Similarly,
`UpdateValues <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#updatevalues>`__
is used to update multiple events.

The following example writes a single data event to the ‘MyFirstStream’
stream. The event has a time index of ‘Now’ and a double ‘Value’ of 1.1:

::

    string streamId = "MyFirstStream";
    DateTime startWrites = DateTime.UtcNow;
    SimpleTypeClass data1 = new SimpleTypeClass()
    {
      TimeId = startWrites,
      Value = (double)1.1
    };
    
    await _dataService.InsertValueAsync(streamId, data1);

The following example writes multiple values to the stream:

::

    List< SimpleTypeClass > writeEvents = new List< SimpleTypeClass >();
    for (int i = 0; i < eventCountToWrite; i++)
    {
        SimpleTypeClass dataEvent = new SimpleTypeClass ()
      {
        TimeId = startWrites.AddSeconds(i),
        Value = (double)i
      };
      writeEvents.Add(dataEvent);
    }
    
    await _dataService.UpdateValuesAsync(streamId, writeEvents);

Additonal information about writing data can be found in `Writing
data <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data.html>`__.

Step 5: Read data
-----------------

Qi includes several different read methods and overloads that can be used to
retrieve data from streams. These methods can be used in a large 
number of circumstances.

Several things that all of the read methods share is that each acts
against a specified stream and each requires one or more indexes upon
which to act.

All of the read operations include a streamId property to indicate the
stream from which to retrieve the data. With the read methods, indexes
are supplied to the method in ‘string’ format. For example, 
to send the index for ‘now’ to read from a stream that has data
indexed with a DateTime type as its index, the string could be defined
as in the following example:

::

    string start = DateTime.UtcNow.ToString("o");

Notice that UTC format is used (time indexes in Qi use UTC format) and
the ‘(”o”)’ formatting ensures that the precision of the
DateTime value is included on the string value.

Additional information about reading data can be found in `Reading
data <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data.html>`__.

Handling transient service interruptions
----------------------------------------

Temporary service interruptions are a fact of life in real-world cloud applications. 
While network connections and external services are basically reliable, your 
application should be able to handle temporary service interruptions and network 
glitches. Often, simply retrying a failed call is all that is required after 
a transient service interruption.

If you access Qi using the Qi client .NET SDKs, error handling for certain 
transient faults is built in; that is, the Qi client .NET SDK automatically 
retries calls that return 503 (Service Unavailable) instead of throwing an 
exception and displaying an error condition.

However, if you access the Qi HTTP API directly, you should consider creating 
your own retry logic to automatically retry the operation that resulted in the error.

Errors that are typically transient have error codes in the 500 range and 
your code should have logic to retry calls that return errors in this range. 
Calls that result in a status in the 400s indicate that there is something 
intrinsically wrong with the request. In this case you should refactor your 
code to correct the error.

Of course, you should limit the number of times you retry a call. For example, 
there might be many users retrying the same failed request, resulting in so 
many queued requests that the service cannot recover. 

A good example retry strategy for HTTP API users might be to retry five times, 
waiting increasingly longer intervals from 1 to 60 seconds between retries.

