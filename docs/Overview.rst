Overview
########

You can find code samples for Python, .NET, Node.js, and Java in the
Qi-Samples repository on GitHub. You should obtain Qi REST API access keys from
`qi.osisoft.com <https://qi.osisoft.com>`__ before running the sample code.

A Qi tenant is a self-contained entity within Qi that is used to
define three different objects in which to store and manage data:

-  **Type**: A user-defined structure that denotes a single measured event or
   object for storage.
-  **Stream**: A basic unit of storage consisting of an ordered series of
   objects that conform to a type definition.
-  **Stream Behavior**: Defines how Qi interpolates or extrapolates
   data during event retrieval when requests occur before, after, or between
   existing data events.

To create a type
----------------

A QiType consists of one or more index properties and one or more
data properties. You use Index properties to arrange data into a sequence.
For example, DateTime is a common index property, but any native type can be used as
an index as long as it allows for ordering of values. For information about
compound indexes refer to:
`QiTypes <https://qi-docs.readthedocs.org/en/latest/QiTypes/#compound-indexes>`__.

You define the structure of your data by defining a QiType object and then
sending it to Qi using `*GetOrCreateType(
)* <https://qi-docs.readthedocs.org/en/latest/QiTypes/#getorcreatetype>`__
method.

A wide variety of QiType data properties are available, 
including lists, arrays and enumerations. For additional information,
including a detailed list of supported data types, refer to
`QiTypes <https://qi-docs.readthedocs.org/en/latest/QiTypes/>`__.

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
    var mySimpleType = _service.GetOrCreateType(SimpleType);

To create a stream
------------------

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
    _service.GetOrCreateStream(stream1);

The stream in the previous example can now be used to hold data values of 
the structure that is defined in mySimpleType. The stream's Name, 
Description, and BehaviorId fields can be modified; however, the Id 
and TypeId cannot be changed after the stream has been created.

Additonal information about streams can be found in
`QiStreams <https://qi-docs.readthedocs.org/en/latest/QiStreams/>`__.

To define Stream behaviors
--------------------------

Qi Stream Behaviors are applied to streams to affect how certain data
read operations are performed. The Stream Behavior object affects whether
interpolation or extrapolation will be done when the
index of a read operation falls between, before, or after stream data.

Additonal information about stream behaviors can be found in
`QiStreamBehaviors <https://qi-docs.readthedocs.org/en/latest/QiStreamBehaviors/>`__.

Containers
----------

As you have seen in the previous topics, when you create a tenent in Qi, you define a Type (which defines the structure of your data), a Stream (which creates an area in which to store your data), and you define a Stream Behavior (which defines rules for how data is read). Tenent information is stored within one or more *containers*. A *container* in this context stores information for a given tenent and can be thought of as an self-contained partition that you use to store the entirety of the data and metadata for your tenent. 

You use containers to separate tenents into logical entities. For example, you might want to have one tenent for production, one for development, and perhaps one or more containers for QA or to serve as a pre-production staging area for certification testing. 

You can create, delete, or obtain information about your containers using the following Qi methods:

::

   CreateContainer(string containerName)
   -need example and API call-
   
   DeleteContainer(string containerName)
   -need example and API call-
   
   GetContainers()
   -need example and API call-


To write data
-------------

Qi has several methods that can be used to write data. For example,
`*InsertValue()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalue>`__
is used to write a single data event to a stream. If the data event
includes an index at that is the same as a previous event, 
this method will throw an exception. However
`*UpdateValue()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#updatevalue>`__
can also be used to write a single event to a stream, and will overwrite
the existing event with the new event.

Each of these methods has a counterpart that acts upon a list of data
events instead of only a single event. For example,
`*InsertValues()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalues>`__
writes multiple events. Similarly,
`*UpdateValues()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#updatevalues>`__
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
    _service.InsertValue(streamId, data1);

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
    _service.UpdateValues(streamId, writeEvents);

Additonal information about writing data can be found in `Writing
data <https://qi-docs.readthedocs.org/en/latest/Writing%20data/>`__.

To read data
------------

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
in as in the following example:

::

    string start = DateTime.UtcNow.ToString("o");

Notice that UTC format is used (time indexes in Qi use UTC format) and
the ‘(”o”)’ formatting ensures that the precision of the
DateTime value is included on the string value.

Additional information about reading data can be found in `Reading
data <https://qi-docs.readthedocs.org/en/latest/Reading%20data/>`__.


Security
--------

There are two types of security accounts for Qi users:

+----------------+------------------------------------------------------------------+
| Account Type   | Description                                                      |
+----------------+------------------------------------------------------------------+
| Administrator  | Allowed to do all CRUD operations on Qi type, stream and stream  |
|                | behavior objects. Also allowed to read and write data to streams |
+----------------+------------------------------------------------------------------+
| User           | Allowed read operations on Qi objects and allowed to read data   | 
|                | from streams                                                     |
+----------------+------------------------------------------------------------------+

