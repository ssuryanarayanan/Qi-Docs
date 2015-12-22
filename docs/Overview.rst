After obtaining Qi REST API access keys from `qi.osisoft.com`_, samples
of clients using Qi can be found on the Qi-Samples repository on GitHub.

A Qi tenant is a self-contained entity within Qi and can be utilized to
define 3 different objects to store and manage data:

-  **Type**: user defined structure denoting a single measured event or
   object for storage
-  **Stream**: basic unit of storage consisting of an ordered series of
   objects conforming to a type definition
-  **Stream Behavior**: defines how Qi will interpolate or extrapolate
   during event retrieval when requests land before, after or in-between
   existing data events

Creating a type
---------------

A QiType is made of one or more index properties and one more or more
data properties. Index properties are used to put data into a sequence.
DateTime is a common index property, but any native type can be used as
an index as long as it allows for ordering of values. For information on
compound indexes refer to `QiTypes`_.

Creating one or more types which define the structure of data is done by
defining a QiType object and sending it to Qi via the `*GetOrCreateType(
)*`_ method.

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
defined in mySimpleType. The stream’s Name, Description, and BehaviorId
field can be modified, however the Id and TypeId cannot be changed once
the stream has been created.

Additonal information on streams can be found in `QiStreams`_.

Writing data
------------

Qi has several methods that can be used to write data. For example,
`*InsertValue()*`_ is used to write a single event of data to a stream

.. _qi.osisoft.com: https://qi.osisoft.com
.. _QiTypes: https://qi-docs.readthedocs.org/en/latest/QiTypes/#compound-indexes
.. _*GetOrCreateType( )*: https://qi-docs.readthedocs.org/en/latest/QiTypes/#getorcreatetype
.. _QiStreams: https://qi-docs.readthedocs.org/en/latest/QiStreams/
.. _*InsertValue()*: https://qi-docs.readthedocs.org/en/latest/Writing%20data/#insertvalue

