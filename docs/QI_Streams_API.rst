QI Stream API calls
====================

``GetStream()``
----------------

The ``GetStream`` method returns a QiStream object

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``string streamId``
  The ID of the stream

**Optional parameters**

  None

**Returns**

  IEnumerable of all streams


**Syntax**

::

    QiStream GetStream(string conatinerId, string streamId);
    Task<QiStream> GetStreamAsync (string namespaceId, string streamId);

**Http**

::

    GET Qi/{namespaceId}/Streams/{streamId}

Security
  Allowed by administrator and user accounts
  
Note
``GetStreams()`` is an overloaded method that is also used to search for and
 return QiStreams. See `Searching for QiStreams 
<https://github.com/osisoft/Qi-Docs/blob/Qi_Edits/docs/Searching.rst>`__ for more information.  


``GetStreams()``
----------------

The ``GetStreams()`` method returns an IEnumerable of all streams.

**Parameters**

``string namespaceId``
  The namespace identifier for the request

  
**Optional parameters**

  None

**Returns**

  Returns IEnumerable of all streams


**Syntax**

::

    IEnumerable<QiStream> GetStreams (string namespaceId);
    Task<IEnumerable<QiStream>> GetStreamsAsync (string namespaceId);

**Http**

::

    GET Qi/{namespaceId}/Streams



Security
  Allowed by administrator and user accounts
  
  
``GetStreams()``
----------------

The ``GetStreams`` method is an overloaded method that is also used to search for and return QiStreams. See `Searching for QiStreams <https://github.com/osisoft/Qi-Docs/blob/Qi_Edits/docs/Searching.rst>`__ for more information.


**Parameters**

``string namespaceId``
  The namespace identifier for the request
``string searchText``
  The text you want to search for.
  ``skip``
  The number of matched stream names to skip over before returning the matching streams.
``count``
  The maximum number of streams to return. 

  
**Optional parameters**

  None

**Returns**

  Returns IEnumerable of all streams


**Syntax**

::

   IEnumerable<QiStream> GetStreams(string searchText, int skip, int count);
   Task<IEnumerable<QiStream>> GetStreamsAsync (string searchText, int skip, int count);
  

**Http**

::

    GET Qi/{namespaceId}/Streams  

	
Security
  Allowed by administrator and user accounts
  
``GetOrCreateStream()``
---------------------

The ``GetOrCreateStream`` method is used to retrieve or create a stream. If an entity with the same *Id* already exists on the service, then the existing stream is returned to the caller unchanged. Otherwise the new stream is created. Content is serialized QiStream entity

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``Qistream entity``
  The ID of the stream for which the type request is made

**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    QiStream GetOrCreateStream (string namespaceId, QiStream entity);
    Task<QiStream> GetOrCreateStreamAsync (string namespaceId, QiStream entity);

**Http**

::

    POST Qi/{namespaceId}/Streams

Security
  Allowed by Administrator account


``UpdateStream()``
----------------

The ``UpdateStream()`` method changes the stream to hold the properties in the QiStream entity given. Permitted changes include:

- Name
- BehaviorId
- Description

Content is serialized QiStream entity

An exception is thrown on unpermitted change attempt (and the stream is
left unchanged)

The *UpdateStream()* method applies to the entire entity. Optional fields
that are omitted from the entity will remove the field from the stream if the fields had
been set previously.

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``string streamId``
  The ID of the stream for which the type request is made
``Qistream entity``  
  Updated stream object
  
  
**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    void UpdateStream(string namespaceId, string streamId, QiStream entity);
    Task UpdateStreamAsync(string namespaceId, string streamId, QiStream entity);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}

Security
  Allowed by Administrator account

``DeleteStream()``
----------------

The ``DeleteStream()`` method is used to delete a stream using its stream ID.

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``string streamId``
  The ID of the stream for which the type request is made

**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    void DeleteStream(string namespaceId, string streamId);
    Task DeleteStreamAsync(string namespaceId, string streamId);

**Http**

::

    DELETE Qi/{namespaceId}/Streams/{streamId}


Security
  Allowed by Administrator account
