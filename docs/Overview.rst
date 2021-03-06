Overview
########

Code samples for Python, .NET, Node.js, and Java can be found in the
Qi-Samples repository on GitHub. Obtain Qi REST API access keys from
`qi.osisoft.com <https://qi.osisoft.com>`__ before running the sample code.

The primary object of the Qi architecture is the *tenant*. Within a tenant you create one or more 
*Namespaces*, in which data types are defined and data is stored. 

.. image:: images/ContainersA.png

You use Namespaces to separate tenants into logical entities. For example, 
you might want to have one Namespace for production, one for development, and 
perhaps another to serve as a pre-production staging area where your QA 
group can run certification testing.

Within a Namespace, Qi defines three different objects in which to store and manage data:

-  **Type**: A user-defined structure that denotes a single measured event or
   object for storage.
-  **Stream**: A basic unit of storage consisting of an ordered series of
   objects that conform to a type definition.
-  **Stream Behavior**: Defines how Qi interpolates or extrapolates
   data during event retrieval when requests occur before, after, or between
   existing data events.

.. image:: images/Containers_1A.png

Each Namespace stores a separate and independent list of Type, Stream, and Stream Behavior objects.

To create a Namespace
---------------------

You must start by creating a Namespace so that you have a place in which to create types, 
streams, and behaviors.

::

   // create a Namespace ‘Namespace1’
   _service.GetOrCreateNamespaceType(“Namespace1”;);


To create a type
----------------

A QiType consists of one or more index properties and one or more
data properties. You use index properties to arrange data into a sequence.
For example, DateTime is a common index property, but any native type can be used as
an index as long as it allows for ordering of values. For information about
compound indexes refer to:
`QiTypes <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Types.html#compound-indexes>`__.

You define the structure of your data by defining a QiType object and then
sending it to Qi using `*GetOrCreateType(
)* <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Types_API.html#getorcreatetype>`__
method.

A wide variety of QiType data properties are available, 
including lists, arrays and enumerations. For additional information,
including a detailed list of supported data types, refer to
`QiTypes <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Types_API.html>`__.

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
    var mySimpleType = _service.GetOrCreateType("Namespace1", SimpleType);

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
    _service.GetOrCreateStream("Namespace1", stream1);

The stream in the previous example can now be used to hold data values of 
the structure that is defined in mySimpleType. The stream's Name, 
Description, and BehaviorId fields can be modified; however, the Id 
and TypeId cannot be changed after the stream has been created.

Information about using streams can be found here: 
`QiStreams <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Streams.html>`__, while stream API information is here: `QiStreams <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Streams_API.html>`__.

To define Stream behaviors
--------------------------

Qi Stream Behaviors are applied to streams to affect how certain data
read operations are performed. The Stream Behavior object affects whether
interpolation or extrapolation will be done when the
index of a read operation falls between, before, or after stream data.

Additonal information about stream behaviors can be found in
`QiStreamBehaviors <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Stream_Behavior.html>`__. Qi Stream behavior API calls can be found here: `QiStreamBehavior API calls <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Stream_Behavior_API.html>`__.


To write data
-------------

Qi has several methods that can be used to write data. For example,
`InsertValue() <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#insertvalue>`__
is used to write a single data event to a stream. If the data event
includes an index at that is the same as a previous event, 
this method will throw an exception. However
`UpdateValue() <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#updatevalue>`__
can also be used to write a single event to a stream, and will overwrite
the existing event with the new event.

Each of these methods has a counterpart that acts upon a list of data
events instead of only a single event. For example,
`InsertValues() <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#insertvalue>`__
writes multiple events. Similarly,
`UpdateValues() <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html#updatevalues>`__
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
    _service.InsertValue("Namespace1", streamId, data1);

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
    _service.UpdateValues("Namespace1", streamId, writeEvents);

Additonal information about writing data can be found in `Writing
data <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data.html>`__. Information about API calls related to writing data can be found here: `API calls for writing data <http://qi-docs-rst.readthedocs.org/en/latest/Writing_Data_API.html>`__.

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
data <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data.html>`__. Information about API calls related to reaading data can be found here: `API Calls for reading data: <http://qi-docs-rst.readthedocs.org/en/latest/Reading_Data_API.html>`__.


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

