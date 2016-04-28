QiStream API calls
==================


The API calls in this section are all used to create and manipulate QiStreams. See `QiStreams <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Streams.html>`__ for a list of supported QiTypes, a discussion of compound indexes, and general information about QiTypes. 

QiStream management via the Qi Client Libraries is performed through the ``IQiMetadataService`` interface, which may be accessed via the ``QiService.GetMetadataService( )`` helper.


``GetStream()``
----------------

Returns a QiStream object from the specified namespace that matches the specified namespace and streamId.


**Syntax**


::

    Task<QiStream> GetStreamAsync (string streamId);

*Http*

::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``string streamId``
  The ID of the stream to retrieve


**Returns**
  A QiStream object for the specified streamId and namespace

Security
  Allowed by administrator and user accounts



``GetStreams()``
----------------

Returns all streams from the specified namespace or Searches for and returns streams that fit search criteria.

**Syntax**

::

    Task<IEnumerable<QiStream>> GetStreamsAsync ( );
    Task<IEnumerable<QiStream>> GetStreamsAsync (string searchText, int skip, int count);

*Http*

::

    GET Qi/{tenantId}/{namespaceId}/Streams

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``searchText``
  Optional. The text to search for.
``skip``
  Optional. The number of matched stream names to skip over before returning the matching streams.
``count``
  Optional. The maximum number of streams to return. 

**Returns**
  IEnumerable of all QiStreams meeting specified criteria in the namespace of the defined tenant.

Security
  Allowed by administrator and user accounts
  


``GetOrCreateStream()``
----------------

Returns a stream that matches the QiStream qistream within the specified namespace, or creates the stream if it does not already exist. If the stream exists, it is returned to the caller unchanged.

**Syntax**

::

    Task<QiStream> GetOrCreateStreamAsync (QiStream qistream);

*Http*

::

    POST Qi/{tenantId}/{namespaceId}/Streams

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``qistream``
  Qi Stream object
 

**Returns**
  An QiStream

Security
  Allowed by administrator accounts
  


``UpdateStream()``
----------------

Updates a specified stream in a specified namespace with the properties in the specified QiStream qistream. The following changes are permitted:

• Name

• BehaviorId

• Description

An exception is thrown on unpermitted change attempt (and the stream is
left unchanged)

The *UpdateStreamAsync()* method applies to the entire entity. Optional fields
that are omitted from the entity will remove the field from the stream if the fields had been set previously.


**Syntax**

::

    Task UpdateStreamAsync(string streamId, QiStream qistream);

*Http*

::

    PUT Qi/{tenantId}/{namespaceId}/Streams/{streamId}

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``streamId``
  Identifier of the stream to modify
``qistream``
  Updated stream object
 

**Returns**
  A QiStream

Security
  Allowed by administrator accounts
  



``DeleteStream()``
----------------

Deletes a stream that matches the QiStream entity within the specified tenantId and namespace.

**Syntax**

::

    Task DeleteStreamAsync(string streamId);

*Http*

::

    DELETE Qi/{tenantId}/{namespaceId}/Streams/{streamId}

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request.
``streamId``
  The identifier of the stream to delete.


**Returns**
  A QiStream

Security
  Allowed by administrator accounts
  
