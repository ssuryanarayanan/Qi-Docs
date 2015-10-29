## Compound Indexes

When defining a QiType the index to be used to sequence the data must be defined by the user in the type definition.  Often a single index (such as a DateTime field) is adequate for the user’s needs, but for more complex situations, Qi allows multiple indexes to be defined in a type. These indexes are concatenated together to form a compound index.
Also indicate that the QI API methods that use Tuples were created to assist the user in using compound indexes.  See below for help with Read Calls with Compound indexes.

## Generics and tuples

The Read methods have overloads that all the caller to define index parameters using a more convenient type than a string. 
Using Generics, the overloads allow the caller to indicate the type of the index (for example DateTime) and then use this type for the index parameter(s) in the call. This is done instead of requiring the index be converted into a string.

Similarly the Read Methods have overloads that use Tuples that are accepted for the indexing instead of a string. Using a Tuple for the index make managing compound index calls easier to make. 

## Methods that act upon multiple streams

The Qi library includes several methods that act upon multiple streams with the same call. These calls require the user to create a ‘QiValues’ list which indicates the streamId and data for each independent operation. 
This example will write an event of Type1 to streamId1 (at a TimeId of ‘now’) and an event of Type2 to streamId2 (with a timestamp 1 second after ‘now’).  

```
  // create QiValues object to hold data to write
  QiValues insertValuesItems = new QiValues();
  var dataEvent1 = new Type1()
  {
    TimeId = DateTime.Now
    Value = (double)1,1
  };
  insertValuesItems.Add<TestTypeClass>(streamId1, dataEvent1);

  var dataEvent2 = new Type2()
  {
    TimeId = DateTime.Now.AddSeconds(1),
    Value = (double)2.2
  };
  insertValuesItems.Add<TestTypeClass>(streamId2, dataEvent2);

  _service.InsertValues(insertValuesItems);
```

There are overloads that act upon multiple streams for the ReplaceValues( )  and UpdateValues( ) methods in addition to the *InsertValues( )* overload illustrated above.
