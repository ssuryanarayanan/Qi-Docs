Qi is capable of storing any data type you care to define.  A Qi Type consists of one or more properties.  Properties are either simple atomic types (e.g., integer) or previously-defined Qi Types.  The latter permits the construction of complex, nested types. The only requirement is that your data type have one or more properties that constitute an ordered key.  While a timestamp is a very common type of key, any ordered value is permitted. 

## Naming Rules for typeId
1.	Case sensitive
2.	Allows spaces

## Type Methods

### GetStreamType
```
QiType GetStreamType(string streamId);
Task<QiType> GetStreamTypeAsync(string streamId);
```

*REST*
```
Qi/Streams/{streamId}/Type
```
HTTP GET

*Parameters*

`streamId` -- id of the stream associated with a type

Returns the type definition associated with a stream


### GetType
```
QiType GetType(string typeId);
Task<QiType> GetTypeAsync(string typeId);
```
*REST*
```
Qi/Types/{typeId}
```

HTTP GET

*Parameters*

```typeId``` -- id of the type to retrieve

Returns type searched for by typeId

### GetTypes
```
IEnumerable<QiType> GetTypes();
Task<IEnumerable<QiType>> GetTypesAsync();
```

*REST*
```
Qi/Types
```

HTTP GET

Returns IEnumerable of all types for the tenant. 


### GetOrCreateType
```
QiType GetOrCreateType(QiType entity);
Task<QiType> GetOrCreateTypeAsync(QiType entity);
```

*REST*
```
Qi/Types
```

HTTP POST
Body is serialized QiType entity

*Parameters*

`entity` -- Qi Type object

Returns a Qi Type object. If entity already exists on the server by Id, that existing type is returned to the caller unchanged.  Otherwise, a new type definition is added to the Qi Service for use by that tenant.

### UpdateType
```
void UpdateType(string typeId, QiType entity);
Task UpdateTypeAsync(string typeId, QiType entity);
```
This call is not allowed at this time.

### DeleteType
```
void DeleteType(string typeId);
Task DeleteTypeAsync(string typeId);
```

*REST*
```
Qi/Types/{typeId}
```

HTTP DELETE

*Parameters*

`typeId` -- string type id of the type to delete

Deletes type from server.
