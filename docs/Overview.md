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

###Writing data

Qi has several methods that can be used to write data including *InsertValue* and *UpdateValue*. 
An *InsertValue* call is used to write a single event of data to a stream. The *InsertValue* method will throw an exception if the data event written includes an index at which there is already an event.
An *UpdateValue* call can also be used to write a single event to a stream, but if an event is found at the index the *UpdateValue* call will overwrite the event with the new event.  

Data that has already been written can be modified with a *ReplaceValue* or *PatchValue* call and to delete data a *RemoveValue* call can be used. 

Each of these methods has a counterpart which acts upon a list of data elements instead of just a single element. For Example the *InsertValues* call inserts multiple events and similarly the *RemoveValues* method can be used to remove multiple events.

The example here will write a single data event to the ‘MyFirstStream’ stream. The event has a Time Index of ‘Now’ and a double ‘Value’ of 1.1:

```
string streamId = "MyFirstStream";
DateTime startWrites = DateTime.UtcNow;
SimpleTypeClass data1 = new SimpleTypeClass()
{
  TimeId = startWrites,
  Value = (double)1.1
};
_service.InsertValue(streamId, data1);
```

This example writes multiple values to the stream (streamId) using an UpdateValues call:

```
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
```

####Write execption handling
If a method call that acts upon multiple data elements (such as *InsertValues( )*, *ReplaceValues()* or *PatchValues( )*) has a problem carrying out its operation an exception is thrown and none of the list of elements is acted upon. So if for example an *InsertValues* call is called with a list 100 events and any one of the events uses an index at which there is already data present, an exception is thrown and all of the elements are rolled-back and no inserts at all are done. In addition, the event at which the error occurred can also be identified in the exception.

Try:

```
{
  _service.InsertValues(streamId, writeEvents);
}
catch (QiHttpClientException e)
{
 	:
  //  e.Errors.Values[0] indicates the streamId of the exception
  //  e.Errors.Values[1] indicates the TimeId of the exception
	:
}
```

####Operating on multiple streams with a single call
There are overloads for several of the above ‘write’ methods (*InsertValues( )*, *UpdateValues( )* and *ReplaceValues( )*) that can be used to act upon multiple streams with a single call. These operations are described in the Advanced Topics section of this document.

####Summary of write methods
The following table shows the methods that are available for writing and changing Qi data:

|Write methods    |Description                                                            |
|---              |---                                                                    |
|InsertValue( )         |Writes a single event in a stream                                |
|InsertValues( )        |Write multiple events in a stream\*                              |
|UpdateValue( )         |Writes (or replaces) a single event in a stream                  |
|UpdateValues( )	      |Writes (or replaces) multiple events in a stream\*               |
|RemoveValue( ) 	      |Deletes a single event from a stream                             |
|RemoveValues( )	      |Deletes multiple events form a stream                            |
|RemoveWindowValues( )	|Deletes all events from a stream in a defined range              |
|ReplaceValue( )	      |Replaces a single events in a stream                             |
|ReplaceValues( )	      |Replaces multiple values in a stream\*                           |
|PatchValue( )	        |Replaces specified properties in a single event in a stream      |
|PatchValues( ) 	      |Replaces specified properties from multiple events in a stream   |

	                                *Include overloads that can act upon multiple streams

###Reading data
Qi includes different read methods and overloads that can be used to retrieve data from streams for a large assortment of circumstances.  

Several things that all of the read methods share is that each acts against a specified stream and each requires one or more indexes upon which to act. 

All of the read operations include a streamId property to indicate the stream from which to retrieve the data.  With the read methods, indexes are supplied to the method in ‘string’ format. So for example if you wanted to send the index for ‘now’ to read from a stream that has data indexed with a DateTime type as its index, the string could be defined in a line something like this:

	string start = DateTime.UtcNow.ToString("o");

Notice that Utc format is used (time indexes in Qi use UTC format) and the ‘(”o”)’ formatting is used to insure that the precision of the DateTime value is included on the string value.

####Generics and tuples
All of the Read Methods have additional overloads that assist you with indexing into a stream when it is inconvenient 	or difficult to convert the index into a string.

Using Generics, the overloads allow the caller to indicate the type of the index (for example DateTime) and then use 	this type for the index parameter(s) in the call. This is done instead of requiring the index be converted into a string.
	
Similarly the Read Methods have overloads that use Tuples that are accepted for the indexing instead of a string. 		Using a Tuple for the index make managing compound index calls easier to make. 
	
See the Advanced Topics for more information on Generics, Tuples and the use of Compound Indexes.

To read a specified number of events from a stream, starting at a predefined start index, the *GetRangeValues( )* method is a good choice. 

This example returns a list of events (up to 100) from streamId starting 30 minutes ago:

```
List< SimpleTypeClass > readEvents;
String startindex = DateTime.UtcNow.AddMinutes(-30).ToString("o");
readEvents = _service.GetRangeValues< SimpleTypeClass >(streamId, startindex, 100).ToList();
```

The *GetRangesValues( )* method (like many others in the library) have an assortment of overloads that allow you to tailor your calls for maximum effectiveness. For Example the *GetRangeValues( )* method  has overloads that allow data to be filtered according to a specified expression or returned the events in in reverse order.

To read all of the events between a start and ending index, the *GetWindowValues( )* method and its overloads can be used. 

The table below summarizes the read methods that are available:

|Read Method|Description|
|---|---|
|FindDistinctValue( )|Returns the event found at a specified index or a ‘null’ if no data exists at the index|
|GetDistinctValue( )|Returns the event found at a specified index or throws an exception if no data exists at the index|
|GetValues( )\*|Returns a value from a specified index. Options allow for interpolation and extrapolation for indexes between, before or after the data in the stream|
|GetValues( )\*|Returns a set of values using a specified set of indexes|
|GetFirstValue( )|Returns the first (oldest) event from a stream|
|GetLastValue( )|Returns the last (most recent) event from a stream|
|GetRangeValues( )\*|Returns a set of events from a stream starting from a predefined start index|
|GetWindowValues( )\*|Reads a set of events from a stream using a specified start and an end index|
								*methods effected by Stream Behaviors

