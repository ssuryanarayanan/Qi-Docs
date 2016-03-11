QiStream API calls
==================

The API calls in this section are all used to create and manipulate QiStreams. See .. _Qi Types: https://github.com/osisoft/Qi-Docs/blob/Qi_Edits/docs/Qi_Types.rst for a list of supported QiTypes, a discussion of compound indexes, and general information about QiTypes. 


``GetStream()``
----------------

Returns a QiStream object from the specified namespace that matches the specified typeId.


**Syntax**

**Qi Client Library**

::

    QiStream GetStream(string conatinerId, string streamId);
    Task<QiStream> GetStreamAsync (string namespaceId, string streamId);

**Http**

::

    GET Qi/{namespaceId}/Streams/{streamId}

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``typeId``
  The ID of the stream to retrieve

**Optional parameters**

  None
  
**Returns**
  A QiStream object for the specified typeId and namespace

Security
  Allowed by administrator and user accounts

**************  

``GetStreams()``
----------------

Returns all streams from the specified namespace.

**Syntax**

**Qi Client Library**

::

    IEnumerable<QiStream> GetStreams (string namespaceId);
    Task<IEnumerable<QiStream>> GetStreamsAsync (string namespaceId);

**Http**

::

    GET Qi/{namespaceId}/Streams

**Parameters**

``string namespaceId``
  The namespace identifier for the request

**Optional parameters**

  None
  
**Returns**
  An Ienumerable object for the specified namespace

Security
  Allowed by administrator and user accounts
  
*********

``GetStreams()`` (GetStreams() overload)
----------------

Searches for and returns streams that fit search criteria

**Syntax**

::

   IEnumerable<QiStream> GetStreams(string searchText, int skip, int count);
   Task<IEnumerable<QiStream>> GetStreamsAsync (string searchText, int skip, int count);
  
  Should the above have a namespace parameter also?

**Http**

::

    GET Qi/{namespaceId}/Streams  

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``searchText``
  The text to search for.
 
**Optional parameters**

``skip`
  The number of matched stream names to skip over before returning the matching streams.
``count``
  The maximum number of streams to return. 

  
**Returns**
  An Ienumerable object of streams that fit search criteria.

Security
  Allowed by administrator and user accounts
  
  
*********

``GetOrCreateStream()``
----------------

Returns a stream that matches the QiStream entity within the specified namespace, or creates the stream if it does not already exist. If the stream exists, it is returned to the caller unchanged.

**Syntax**

::

    QiStream GetOrCreateStream (string namespaceId, QiStream entity);
    Task<QiStream> GetOrCreateStreamAsync (string namespaceId, QiStream entity);

**Http**

::

    POST Qi/{namespaceId}/Streams

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``entity``
  Qi Stream object
 
**Optional parameters**

  None
  
**Returns**
  An QiStream

Security
  Allowed by administrator accounts
  
*********

``UpdateStream()``
----------------

Updates a specified stream in a specified namespace with the properties in the specified QiStream entity. The following changes are permitted:

• Name

• BehaviorId

• Description

An exception is thrown on unpermitted change attempt (and the stream is
left unchanged)

The *UpdateStream()* method applies to the entire entity. Optional fields
that are omitted from the entity will remove the field from the stream if the fields had been set previously.


**Syntax**

::

    void UpdateStream(string namespaceId, string streamId, QiStream entity);
    Task UpdateStreamAsync(string namespaceId, string streamId, QiStream entity);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``streamId``
  Identifier of the stream to modify
``entity``
  Updated stream object
 
**Optional parameters**

  None
  
**Returns**
  A QiStream

Security
  Allowed by administrator accounts
  

*********

``DeleteStream()``
----------------

Deletes a stream that matches the QiStream entity within the specified namespace.

**Syntax**

::

    void DeleteStream(string namespaceId, string streamId);
    Task DeleteStreamAsync(string namespaceId, string streamId);

**Http**

::

    DELETE Qi/{namespaceId}/Streams/{streamId}

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``streamId``
  Identifier of the stream to delete 

  **Optional parameters**

  None
  
**Returns**
  An QiStream

Security
  Allowed by administrator accounts
  
