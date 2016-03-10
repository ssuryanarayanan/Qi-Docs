Qi Types API calls
==================

The API calls in this section are all related to creating and manipulating Qi Types. See Qi Types for general information about
Qi Types.


``GetStreamType()``
----------------

Returns the type definition that is associated with a stream.

**Parameters**

string ``namespaceId``
  The namespace identifier for the request
string ``streamId``
  The ID of the stream for which the type request is made

**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    QiType GetStreamType(string namespaceId, string streamId);
    Task<QiType> GetStreamTypeAsync(string namespaceId, string streamId);

**Http**

::

    GET Qi/{namespaceId}/Streams/{streamId}/Type

Security
  Allowed by administrator and user accounts


``GetType()``
----------------

The ``GetType()`` method returns the type that is searched for by the ``typeId``. Use the ``getype()`` method when you know the type ID and want to get the Qitype object

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``typeId``
  The ID of the type to retrieve

**Optional parameters**

  None
  
**Returns**
  QiType 

**Syntax**

::

    QiType GetType(string namespaceId, string typeId);
    Task<QiType> GetTypeAsync(string namespaceId, string typeId);

**Http**

::

    GET Qi/{namespaceId}/Types/{typeId}
    
Security
  Allowed by administrator and user accounts


``GetTypes()``
----------------

The ``GetTypes`` method returns a Qi Type object. If the entity ID already exists
on the service, the existing type is returned to the caller unchanged.
Otherwise, a new type definition is added to the Qi Service

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``QiType entity``
  The object

**Optional parameters**

  None

**Returns**

  IEnumerable of all types


**Syntax**

::

    QiType GetOrCreateType(string namespaceId, QiType entity);
    Task<QiType> GetOrCreateTypeAsync(string namespaceId, QiType entity);

**Http**

::

    POST Qi/{namespaceId}/Types


Security
  Allowed by administrator account


``GetOrCreateType()``
----------------

The ``GetOrCreateType()`` method Returns a Qi Type object. If the entity ID already exists on the service, the existing type is returned to the caller unchanged. Otherwise, a new type definition is added to the Qi Service.
Content is serialized QiType entity

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``QiType entity``
  The ID of the stream for which the type request is made

**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    QiType GetOrCreateType(string namespaceId, QiType entity);
    Task<QiType> GetOrCreateTypeAsync(string namespaceId, QiType entity);

**Http**

::

    POST Qi/{namespaceId}/Types


Security
  Allowed by administrator account


``DeleteType()``
----------------

The ``DeleteType()`` method deletes the type from service A. The type cannot be deleted from the service if existing streams are associated with it.

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``string typeId``
  The ID of the type to delete

**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    void DeleteType(string namespaceId, string typeId);
    Task DeleteTypeAsync(string namespaceId, string typeId);

**Http**

::

    DELETE Qi/{namespaceId}/Types/{typeId}


Security
  Allowed by administrator account


``UpdateType()``
----------------

The ``UpdateType()`` method updates a type’s definition. A type cannot be updated if
existing streams are associated with it.

**Parameters**

``string namespaceId``
  The namespace identifier for the request
``string typeId``
  The typeId of the type to update

**Optional parameters**

  None

**Returns**

  Qitype


**Syntax**

::

    void UpdateType(string namespaceId, string typeId, QiType entity);
    Task UpdateTypeAsync(string namespaceId, string typeId, QiType entity);

**Http**

::

    PUT Qi/{namespaceId}/Types/{typeId}

Security
  Allowed by Administrator account
