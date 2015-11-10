Streams are the fundamental unit of storage in the Qi Service. Each stream represents an ordered series of events or observations for a particular item of interest, e.g., the temperature of a given room. Each stream must have a Qi Type associated with it, and each event sent to the stream must conform to this type or it will be rejected. Streams may optionally have a Stream Behavior to control how data is returned.

## Qi Stream Object

```
  string Id           (required Id for referencing the type)
  string Name         (optional name)
  string Description  (optional description text)
  string TypeId       (required type to be used for this stream)
  string BehaviorId   (optional Stream Behavior for this stream)
```

A Qi Stream object must include an ‘Id’ field (unique) and a ‘TypeId’ field with the Id of an existing Type. 
‘Name’ and ‘Description’ fields are optional text fields.
‘BehaviorId’ is set with the Id of an existing Stream Behavior but is optional.

When BehaviorId is not included the stream will have default behavior which is equivalent to Mode = ‘Continuous’ and Extrapolation = ‘All’. See Stream Behavior for more information.

A stream is always referenced with its Id property.

## Naming Rules for Stream Identifiers
1.	Case Insensitive
2.	Allows spaces
3.	Cannot start or end with two underscores ("__")
4.	Cannot start or end with period (".")
4.	Maximum of 260 characters
5.	Cannot use the following characters: (\ / : ? # [ ] @ ! $ & ' ( ) * + , ; =)
6.	No more than 250 periods (".") allowed  

## Stream Methods
### GetStream
*_Qi Client Library_*
```
QiStream GetStream(string streamId);
Task<QiStream> GetStreamAsync (string streamId);
```

*_Http_*
```
GET Qi/Streams/{streamId}
```

**Parameters**
`streamId` -- string identifying the stream

**Security**
Allowed by Administrator and User accounts

**Operation**
Returns a QiStream object

### GetStreams
*_Qi Client Library_*
```
IEnumerable<QiStream> GetStreams ();
Task<IEnumerable<QiStream>> GetStreamsAsync ();
```

*_Http_*
```
GET Qi/Streams
```

**Parameters**
None

**Security**
Allowed by Administrator and User accounts

**Operation**
Returns IEnumerable of all streams

### GetOrCreateStream
*_Qi Client Library_*
```
QiStream GetOrCreateStream (QiStream entity);
Task<QiStream> GetOrCreateStreamAsync (QiStream entity);
```

*_Http_*
```
POST Qi/Streams
```
Content is serialized QiStream entity

**Parameters**
`entity` -- Qi Stream object

**Security**
Allowed by Administrator account

**Operation**
If entity already exists on the service by `Id`, that existing stream is returned to the caller unchanged. Otherwise the new stream is created.

### UpdateStream
*_Qi Client Library_*
```
void UpdateStream(string streamId, QiStream entity);
Task UpdateStreamAsync(string streamId, QiStream entity);
```

*_Http_*
```
PUT Qi/Streams/{streamId}
```
Content is serialized QiStream entity

**Parameters**
`streamId` -- identifier of the stream to modify
`entity` -- updated stream object

**Security**
Allowed by Administrator account

**Operation**
Changes the stream to hold the properties in the QiStream entity given.
Permitted changes:

•	Name
•	BehaviorId
•	Description

An exception is thrown on unpermitted change attempt (and the stream is left unchanged). 

The `UpdateStream` method applies the entire entity. Optional fields left out of the entity will remove the field from the stream if they had been set previously. 

### DeleteStream
*_Qi Client Library_*
```
void DeleteStream(string streamId);
Task DeleteStreamAsync(string streamId);
```

*_Http_*
```
DELETE Qi/Streams/{streamId}
```

**Parameters**
`streamId` -- identifier of the stream to delete

**Security**
Allowed by Administrator account

**Operation**
Delete stream using its stream id

