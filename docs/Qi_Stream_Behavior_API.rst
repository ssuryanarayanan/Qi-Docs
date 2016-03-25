QiStreamBehavior API calls
==================

The API calls in this section are all used to create and manipulate QiStreamBehaviors. See `QiStream behavior <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Stream_Behavior.html>`__ for a list of a list of properties, interpolation, extrapolation, and overrides.


``DeleteBehavior()``
----------------

Deletes a QiStream behavior from the specified namespace that matches the specified behaviorId. You cannot delete a stream behavior that is associated with a stream.


**Syntax**

::

    void DeleteBehavior(string tenantId, string namespaceId, string behaviorId);
    Task DeleteBehaviorAsync(string tenantId, string namespaceId, string behaviorId);

**Http**

::

    DELETE Qi/{tenantid}/{namespaceId}/Behaviors/{behaviorId}

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
 
``behaviorId``
  The ID of the behavior to delete.


**Returns**
  A QiStream object for the specified typeId and namespace.

Security
  Allowed by administrator accounts




``GetBehavior()``
----------------

Retrieves a QiStreamBehavior from the specified namespace that matches the specified behaviorId

**Syntax**

::

    QiStreamBehavior GetBehavior(string tenantId, string namespaceId);
    Task<QiStreamBehavior> GetBehaviorAsync(string tenantId, string namespaceId);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Behaviors/{behaviorId}

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request


**Returns**
  A QiStream object

Security
  Allowed by administrator and user accounts



``GetBehaviors()``
----------------

Retrieves all QiStream behaviors from a namespace.


**Syntax**

::

    IEnumerable<QiStreamBehavior> GetBehaviors(string tenantId, string namespaceId);
    Task<IEnumerable<QiStreamBehavior>> GetBehaviorsAsync(string tenantId, string namespaceId);

**Http**

::

    GET Qi/{tenantId}/{namespaceId}/Behaviors

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``behaviorId``
  The ID of the behavior to retrieve.


**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator and user accounts

  
**********

``GetOrCreateBehavior()``
----------------

Retrieves the QiStream behavior from a namespace, or creates the behavior if the behavior does not already exist. If the behavior exists, it is returned to the caller unchanged.

**Syntax**

::

    QiStreamBehavior GetOrCreateBehavior(string tenantId, string namespaceId, QiStreamBehavior qibehavior);
    Task<QiStreamBehavior> GetOrCreateBehaviorAsync(string tenantId, string namespaceId, QiStreamBehavior qibehavior);

**Http**

::

    POST  Qi/{tenantId}/{namespaceId}/Behaviors
	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``qibehavior``
  A QiStreamBehavior object to add to Qi.


**Returns**
  An IEnumerable of all behavior objects.

Security
  Allowed by administrator accounts

**********

``UpdateBehavior()``
----------------

Replaces the stream’s existing behavior with those defined in the ‘qibehavior’. If certain aspects of the existing behavior are meant to remain, they must be included in qibehavior.

An override list can be included in the ‘qibehavior’ to cause
the addition, removal, or change to this list.

**Syntax**

::

    void UpdateBehavior(string tenantId, string namespaceId, string behaviorId, QiStreamBehavior qibehavior);
    Task UpdateBehaviorAsync(string tenantId, string namespaceId, string behaviorId, QiStreamBehavior qibehavior);

**Http**

::

    PUT Qi/{tenantId}/{namespaceId}/Behaviors/{behaviorId}	
**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
``qibehavior``
  The updated stream behavior


**Returns**
  An IEnumerable of all behavior objects

Security
  Allowed by administrator accounts

