Overview
########

You can find code sameples for Python, .NET, Node.js, and Java in the
Qi-Samples repository on GitHub. You should obtain Qi REST API access keys from
`qi.osisoft.com <https://qi.osisoft.com>`__ before running the sample code.

A Qi tenant is a self-contained entity within Qi that can be used to
define three different objects in which to store and manage data:

-  **Type**: A user-defined structure that denotes a single measured event or
   object for storage.
-  **Stream**: A basic unit of storage consisting of an ordered series of
   objects that conform to a type definition.
-  **Stream Behavior**: Defines how Qi interpolates or extrapolates
   data during event retrieval when requests occur before, after, or between
   existing data events.

Creating a type
---------------

A QiType is made of one or more index properties and one more or more
data properties. Index properties are used to put data into a sequence.
DateTime is a common index property, but any native type can be used as
an index as long as it allows for ordering of values. For information on
compound indexes refer to
`QiTypes <https://qi-docs.readthedocs.org/en/latest/QiTypes/#compound-indexes>`__.

Creating one or more types which define the structure of data is done by
defining a QiType object and sending it to Qi via the `*GetOrCreateType(
)* <https://qi-docs.readthedocs.org/en/latest/QiTypes/#getorcreatetype>`__
method.

There are a wide variety of QiType data properties including lists,
arrays and enumerations. For additional information including a full
list of supported data types refer to
`QiTypes <https://qi-docs.readthedocs.org/en/latest/QiTypes/>`__.

This example creates a simple type:

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

Creating a stream
-----------------

Streams are used to hold data of a predefined type. To create a QiStream
the Id and TypeId of the stream must be defined. Optionally a Name,
Description, and BehaviorID can be defined.

This example creates a QiStream with an Id of ‘MyFistStream’ of type
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

The above stream can now be used to hold data values of the structure
defined in mySimpleType. The stream's Name, Description, and BehaviorId
field can be modified, however the Id and TypeId cannot be changed once
the stream has been created.

Additonal information on streams can be found in
`QiStreams <https://qi-docs.readthedocs.org/en/latest/QiStreams/>`__.

Writing data
------------

Qi has several methods that can be used to write data. For example,
`*InsertValue()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalue>`__
is used to write a single event of data to a stream. If the data event
written includes an index at which there is already an event this method
will throw an exception. However
`*UpdateValue()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#updatevalue>`__
can also be used to write a single event to a stream, and will overwrite
the existing event with the new event.

Each of these methods has a counterpart which acts upon a list of data
events instead of just a single event. For example,
`*InsertValues()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalues>`__
writes multiple events and similarly
`*UpdateValues()* <https://qi-docs.readthedocs.org/en/latest/Writing%20data/#updatevalues>`__
can be used to update multiple events.

This example will write a single data event to the ‘MyFirstStream’
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

This example will write multiple values to the stream:

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

Additonal information on writing data can be found in `Writing
data <https://qi-docs.readthedocs.org/en/latest/Writing%20data/>`__.

Reading data
------------

Qi includes different read methods and overloads that can be used to
retrieve data from streams for a large assortment of circumstances.

Several things that all of the read methods share is that each acts
against a specified stream and each requires one or more indexes upon
which to act.

All of the read operations include a streamId property to indicate the
stream from which to retrieve the data. With the read methods, indexes
are supplied to the method in ‘string’ format. So for example if you
wanted to send the index for ‘now’ to read from a stream that has data
indexed with a DateTime type as its index, the string could be defined
in a line something like this:

::

    string start = DateTime.UtcNow.ToString("o");

Notice that Utc format is used (time indexes in Qi use UTC format) and
the ‘(”o”)’ formatting is used to insure that the precision of the
DateTime value is included on the string value.

Additional information on reading data can be found in `Reading
data <https://qi-docs.readthedocs.org/en/latest/Reading%20data/>`__

Stream behaviors
----------------

Qi Stream Behaviors are applied to streams to affect how certain data
read operations will be performed. The Stream Behavior object affects whether
interpolation and/or extrapolation will be done when the
index of a read operation falls between, before or after stream data.

Additonal information on stream behaviors can be found in
`QiStreamBehaviors <https://qi-docs.readthedocs.org/en/latest/QiStreamBehaviors/>`__.

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

