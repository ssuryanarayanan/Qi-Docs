After obtaining Qi REST API access keys from [qi.osisoft.com](https://qi.osisoft.com), samples of clients using Qi can be found on the <a href="https://github.com/osisoft/Qi-Samples" target="_blank">Qi-Samples</a> repository on GitHub.

Your tenant is a self-contained entity within Qi and can be utilized to define 3 different objects to store and manage data:

* __Type__: user defined structure denoting a single measured event or object for storage
* __Stream__: basic unit of storage consisting of an ordered series of objects conforming to a type definition
* __Stream Behavior__: defines how Qi will interpolate or extrapolate during event retrieval when requests land before, after or in-between existing data events

##Creating a type

A QiType is made of one or more index properties and one more or more data properties. Index properties are used to put data into a sequence. DateTime is a common index property, but any native type can be used as an index as long as it allows for ordering of values. For information on compound indexes refer to [Advanced topics](https://qi-docs.readthedocs.org/en/latest/Advanced%20Topics/).

Creating one or more types which define the structure of data is done by defining a QiType object and sending it to Qi via the [*GetOrCreateType( )*](https://qi-docs.readthedocs.org/en/latest/QiTypes/#getorcreatetype) method.

There are a wide variety of QiType data properties including lists, arrays and enumerations. For a full list of supported data types refer to [QiTypes](https://qi-docs.readthedocs.org/en/latest/QiTypes/).

This example creates a simple type:
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

##Creating a stream
Streams are used to hold data of a predefined type.
To create a QiStream the Id and TypeId of the stream must be defined. Optionally a Name, Description, and BehaviorID can be defined.

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

The above stream can now be used to hold data values of the structure defined in mySimpleType. The stream's Name, Description, and BehaviorId field can be modified, however the Id and TypeId cannot be changed once the stream has been created.  

##Writing data

Qi has several methods that can be used to write data. For example, [*InsertValue()*](https://qi-docs.readthedocs.org/en/latest/Data/#insertvalue) is used to write a single event of data to a stream. If the data event written includes an index at which there is already an event this method will throw an exception.  However [*UpdateValue()*](https://qi-docs.readthedocs.org/en/latest/Data/#updatevalue) can also be used to write a single event to a stream, and will overwrite the existing event with the new event.  

Each of these methods has a counterpart which acts upon a list of data events instead of just a single event. For example, [*InsertValues()*](https://qi-docs.readthedocs.org/en/latest/Data/#insertvalues) writes multiple events and similarly [*UpdateValues()*](https://qi-docs.readthedocs.org/en/latest/Data/#updatevalues) can be used to update multiple events.

This example will write a single data event to the ‘MyFirstStream’ stream. The event has a time index of ‘Now’ and a double ‘Value’ of 1.1:

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

This example will write multiple values to the stream:

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

###Write execption handling
If a method that acts upon multiple data events has a problem carrying out the operation an exception is thrown and none of the list of elements is acted upon. For example [*InsertValues*](https://qi-docs.readthedocs.org/en/latest/Data/#insertvalues) is called with a list of 100 events and one of the events uses an index at which there is already data present. An exception will be thrown and all of the events will be rolled back resulting in no inserts for the 100 events. The event at which the error occurred cwill be identified in the exception.

For example:

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

###Summary of write methods
The following table shows the methods that are available for writing and changing Qi data. For further detail refer to [QiValues](https://qi-docs.readthedocs.org/en/latest/Data/):

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

##Reading data
Qi includes different read methods and overloads that can be used to retrieve data from streams for a large assortment of circumstances.  

Several things that all of the read methods share is that each acts against a specified stream and each requires one or more indexes upon which to act. 

All of the read operations include a streamId property to indicate the stream from which to retrieve the data.  With the read methods, indexes are supplied to the method in ‘string’ format. So for example if you wanted to send the index for ‘now’ to read from a stream that has data indexed with a DateTime type as its index, the string could be defined in a line something like this:

	string start = DateTime.UtcNow.ToString("o");

Notice that Utc format is used (time indexes in Qi use UTC format) and the ‘(”o”)’ formatting is used to insure that the precision of the DateTime value is included on the string value.

###Generics and tuples
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

###Interpolation and extrapolation
While using methods to read data from Qi, the indexes requested may land between, after or before the events in a stream. The *FindDistinctValue( )* and *GetDistinctValue( )* methods have a predefined outcome for these cases. The *FindDistinctValue( )* will return a ‘null’ when no event exists at the defined index, while the *GetDistinctValue( )* method will throw an exception. Other read methods that will use predefined stream settings to determine how to report when indexes land between, before or after data. These predefined settings are called Stream Behaviors and they determine when and how data is interpolated and extrapolated by certain read methods. Stream Behaviors are described in a later subsection of this document. Documentation for each read method will also indicate any pertinent interpolation or extrapolation information.

###Reading through data in a stream
Several of Qi’s read methods have options which assist you reading sequentially through the data in a stream. For Example the *GetWindowValues( )* method retrieves data between two indexes, but it also includes overloads which allow you to specific the maximum number of events you would like to receive from the call. When you use this *GetWindowValues( )* overload it return a set of events of the size requested, but also gives the caller a ‘continuation token’ which can be used on a subsequent *GetWindowValues( )* call to return the next set of sequential events in the stream.

The *GetRangeValues( )* method also has overloads that include a ‘skip’ parameter which allows you to retrieve make multiple calls and retrieve different sets of data after a specified time stamp.

###Filtering data 
The *GetWindowValues( )*  and *GetRangeValues( )* include options to ‘filter’ the data retrieved during a call. These overloads allow you to specify conditions that must be met by the event or it will not be returned. It is a powerful way to quickly read through data to find specific information. See the Advanced Topics for more information on Filter Expressions.

###Selecting data
The *GetWindowValues( )* can be used to ‘select’ which properties of the data Type should be retrieved. 

For example assume you have a stream that stores type data that includes a ‘double’ value and a ‘string’ value (along with an index). A *GetWindowValues( )* call can be made which requests that only the ‘double’ value be returned in the set of information retrieved by the call. This can be very helpful when specific data is needed and such calls positively impact performance since data that is not needed does not get transferred to the client program.

##Stream behaviors
The *GetValue( )* method is used to request data from a stream at a specific indexes. The *GetValue( )* method allows for multiple indexes to be queried in a single call similar to making multiple *GetValue( )* calls. As you would expect, when there is data at the index specified by one of these calls, it is returned to the user. However when a specified index is between two events the method must determine how to respond. 

In Qi, the response depends on the stream’s Stream Behavior.

A Stream Behavior is an object that the user defines and includes in the definition of a stream (similar to how a Type is defined at set for a stream).

The code here shows the definition and creation a simple Stream Behavior object:

```
String behaviorid = “myFirstBehavior”;
QiStreamBehavior simpleBehavior = new QiStreamBehavior()
{
	Id = behaviorId,
        Mode = QiStreamMode.StepwiseContinuousLeading,
};
_service.GetOrCreateBehavior(createdBehavior);
```

When creating the stream, the user can apply the behavior as shown here:

```
string streamId = "MyFirstStream";
string streamType = " mySimpleType ";
QiStream stream1 = new QiStream()
{
	Id = streamId,
	TypeId = streamType,
	BehaviorId = “MyFirstBehavior”
}
_service.GetOrCreateStream(stream1);
```

The value of the Stream Behavior ‘Mode’ determines how the stream will interpolate data (i.e. how it will respond to requests for data in-between existing indexes). The table here indicates how a stream will behave for given Mode values:

|Mode|Operation|
|---|---|
|Default|= Continuous|
|Continuous|Interpolates the data using previous and next index values\*|
|StepwiseContinuousLeading|Returns the data from the previous index|
|StepwiseContinuousTrailing|Returns the data from the next index|
|Discrete|Returns ‘null’|
 			*Certain value types cannot be interpolated. See Stream Behavior for these special cases

The Stream Behavior object can also be used to give different Mode settings to different data properties within the stream’s type. This allows you to for example have a ‘Discrete’ mode setting for one property and a ‘Continuous’ Mode setting for another. This is done with the Overrides list.

In addition to interpolations settings, the Stream Behavior also has a property called ‘QiStreamExtrapolation’ that along with the Mode will determine how a stream will respond to requests for an index that precedes or follows all of the data in the stream. ‘QiStreamExtrapolation’ acts as a master switch to determine whether extrapolation will be done and at which end of the data. 

‘QiStreamExtrapolation’ can be set to one of these values:
* All (default)
* None
* Forward
* Backward

A QiStreamExtrapolation value setting of ‘None’ will turn ‘off’ all extrapolation (for indexes both before and after all the data in the stream). The ‘Forward’ setting will extrapolate for indexes prior to the stream data, but will not extrapolate at indexes after all the data. Conversely the ‘Backward’ setting will do extrapolation at the end of the data, but not at indexes at the front of stream data.

The following table shows stream behavior for QiStreamExtrapolation values:

|QiStreamExtrapolation|Index before data|Index after data|
|---|---|---|
|All|Extrapolation activated|Extrapolation activated|
|None|No extrapolation|No extrapolation|
|Forward|Extrapolation activated|No extrapolation|
|Backward|No extrapolation|Extrapolation activated|

When the QiStreamExtrapolation setting allows for extrapolation, then the value returned will be determined by the Stream Behavior Mode setting. This table shows what occurs when the Mode is ‘Continuous’:

|QiStreamExtrapolation|Index before data|Index after data|
|All|Returns first data value|Returns last data value|
|None|No extrapolation|No extrapolation|
|Forward|Returns first data value|No extrapolation|
|Backward|No extrapolation|Returns last data value|

For best results, see the documentation on the read method you are using regarding the effect of the Stream Behavior.

Note that if you do not assign a specific Stream Behavior object to a stream, it will assume the default behavior which is Mode = ‘Continuous’ (with no Overrides) and a QiStreamExtrapolation setting of ‘All’. 

###Methods effected by stream behavior
The Stream Behavior setting effects the *GetValue( )* and *GetValues( )* methods as described above.

The *GetWindowValues( )*  and *GetRangeValues( )* calls are also effected by the Stream Behavior when the indexes used fall before, after or between data indexes.

##Security

There are two types of security accounts for Qi users: Administrator and User:

|Account Type|Description|
|---|---|
|Administrator|Allowed to do all CRUD operations on Qi type, stream and stream behavior objects. Also allowed to read and write data to streams|
|User|Allowed read operations on Qi objects and allowed to read data from streams|
