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
