QiType API calls
==================

The API calls in this section are all used to create and manipulate QiTypes. See `QiTypes <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Types.html>`__ for general QiType information, working with compound indexes, and supported QiTypes,


GetStreamType()
----------------

Returns the type definition that is associated with a given stream from the specified namespace.

**Syntax**

::

    QiType GetStreamType(string tenantId, string namespaceId, string streamId);
    Task<QiType> GetStreamTypeAsync(string tenantId, string namespaceId, string streamId);

*Http*
::

    GET Qi/{tenantId}/{namespaceId}/Streams/{streamId}/Type


**Parameters**

``string tenantID``
  The tenant identifier for the request
string ``namespaceId``
  The namespace identifier for the request
string ``streamId``
  The ID of the stream to search for the specified type definition


**Returns**

  Qitype type definition


Security
  Allowed by administrator and user accounts


GetType()
----------------

Returns the type of the specified ``typeId`` from the specified namespace. 

**Syntax**

::

    QiType GetType(string tenantId, string namespaceId, string typeId);
    Task<QiType> GetTypeAsync(string tenantId, string namespaceId, string typeId);

*Http*

::

    GET Qi/{tenantId}/{namespaceId}/Types/{typeId}

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``string typeId``
  The ID of the type to retrieve


**Returns**
  A QiType specified by the typeId

Security
  Allowed by administrator and user accounts


GetTypes()
----------------

Returns a list of all types within a given namespace. 

**Syntax**

::

    IEnumerable<QiType> GetTypes(string tenantId, string namespaceId);
    Task<IEnumerable<QiType>> GetTypesAsync(string tenantId, string namespaceId);


*Http*

::

    GET Qi/Types


**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request

**Returns**

  IEnumerable QiType of all types in the namespace


Security
  Allowed by administrator and user accounts


GetOrCreateType()
----------------

Returns the type of the specified ``typeId`` within a namespace, or creates the type if the ``typeId`` does not already exist. If the typeId exists, it is returned to the caller unchanged. 


**Syntax**

::

    QiType GetOrCreateType(string tenantId, string namespaceId, QiType qitype);
    Task<QiType> GetOrCreateTypeAsync(string tenantId, string namespaceId, QiType qitype);

*Http*

::

    POST Qi/{tenantId}/{namespaceId}/Types



**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``QiType qitype``
  The type of the stream for which the type request is made


**Returns**

  Qitype


Security
  Allowed by administrator account


DeleteType()
----------------

Deletes a type from the specified namespace. Note that a type cannot be deleted if there are streams associated with it.

**Syntax**

::

    void DeleteType(string tenantId, string namespaceId, string typeId);
    Task DeleteTypeAsync(string tenantId, string namespaceId, string typeId);

*Http*

::

    DELETE Qi/{tenantId}/{namespaceId}/Types/{typeId}



**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``string typeId``
  The ID of the type to delete

**Returns**

  Qitype


Security
  Allowed by administrator account


UpdateType()
----------------

Updates the definition of a type. Note that a type cannot be updated if there are streams associated with it. Also, certain parameters cannot be changed after they are defined.

**Syntax**

::

    void UpdateType(string tenantId, string namespaceId, string typeId, QiType qitype);
    Task UpdateTypeAsync(string tenantId, string namespaceId, string typeId, QiType qitype);

*Http*

::

    PUT Qi/{tenantId}/{namespaceId}/Types/{typeId}


**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``string qitype``
  The qitype of the type to update


**Returns**

  Qitype

Security
  Allowed by Administrator account
