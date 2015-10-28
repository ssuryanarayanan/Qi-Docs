AFter obtaining Qi API access keys by visiting <qi.osisoft.com>, samples of clients using the Qi service can be found by visiting the <a href="https://github.com/osisoft/Qi-Samples" target="_blank">Qi-Samples Repository</a> on GitHub.

Your tenant is a self-contained entity within the Qi Service and can be utilized to define 3 different Qi objects that are used to store and manage data:

* Type -- user defined structure denoting a single measured event or object for storage
* Stream -- basic unit of storage consisting of an ordered series of objects conforming to a type definition
* Stream Behavior -- defines how Qi will interpolate or extrapolate during event retrieval when requests land before, after or in-between existing data events.

###Creating a type

The first step is to create one or more Types which define the structure of the data. This is done by defining a QiType entity and sending it to QI via a *‘GetOrCreateType( )’* call.

A Type in Qi is made of one or more index properties and one more or more data properties. The index fields are used to put data into a sequence. A common index field is a DateTime, but any native type can be used as an index long as it allows for ordering of values. Multiple or compound indexes can also be used (see Advanced Topics).

For the data properties of a QiType, Qi allows a wide variety of types including the ability to use structures such as lists, arrays and enumerations. For a full list of supported data types see the Appendix.

The example here creates a Simple Type:
```
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
```

###Creating a stream
A Stream is used to hold data of a predefined type.
To create a QiStream the Id and TypeId of the stream must be defined. Additionally the QiStream can be given a value for the Name and Description fields.

This example creates a QiStream with an Id of ‘MyFistStream’ of type ‘mySimpleType’:

```
string streamId = "MyFirstStream";
string streamType = " mySimpleType ";
QiStream stream1 = new QiStream()
{
    Id = streamId,
    TypeId = streamType
}
_service.GetOrCreateStream(stream1);
```

This stream (with stream Id ‘MyFirstStream’ can now be used to hold data values of the structure defined in mySimpleType.

The streams Name and Description field can be modified, but the Id or TypeId cannot be changed once the stream has been created.  

