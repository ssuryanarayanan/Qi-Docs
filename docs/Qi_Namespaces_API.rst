Qi namespace API calls
======================

The API calls in this section are all used to create and manipulate Qi namespaces. You can create, delete, or obtain information about your namespaces using the methods outlined in this topic. Namespace management via the Qi Client Libraries is performed through the ``IQiAdministrationService`` interface, which may be accessed via the ``QiService.GetAdministrationService( )`` helper.

``GetNamespaceAsync()``
-------------------

Retrieves an existing namespace.

**Syntax**

.. highlight:: none

::

    Task<QiNamespace> GetNamespaceAsync(string namespaceId);

**Http**

::

    GET "Qi/{tenantId}/Namespaces/{namespaceId}”


**Parameters**

``string tenantId``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request


Security
  Allowed by administrator and user accounts.

**Returns** 
  Returns a namespace.

**********************


``GetNamespacesAsync()``
----------------

Retrieves a list of existing namespaces.

**Syntax**

::

    Task<IEnumerable<QiNamespace>> GetNamespacesAsync();


*Http*

::

    GET "Qi/{tenantId}/Namespaces"


**Parameters**

``string tenantId``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
  
Security
  Allowed by administrator and user accounts.

**Returns**
  Returns a list of namespaces.


``GetOrCreateNamespaceAsync()``
----------------

Returns the namespace with the specified namespaceId, or creates the namespace if the namespace does not already exist. 
If the namespace exists, it is returned to the caller unchanged. For security, this method is 
executable only by an administrator account.

::

    Task<QiNamespace> GetOrCreateNamespaceAsync(QiNamespace qinamespace);

**Http**

::

    POST "Qi/{tenantId}/Namespaces/"


**Parameters**

``string tenantId``
  The tenant identifier for the request.
``QiNamespace qinamespaceId``
  The QiNamespace object for which the request is being made.

**Security**
  Allowed by administrator account.

**Returns** 
  Returns a namespace.


``DeleteNamespaceAsync()``
----------------

Deletes the namespace with the specified namespaceId from the tenant specified by the tenantId.

**Syntax**

::

    Task DeleteNamespaceAsync(string namespaceId);

**Http**

::

    DELETE "Qi/{tenantId}/Namespaces/{namespaceId}”

**Parameters**

``string tenantId``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
  

**Security** 
  Allowed by administrator account.

**Returns** 
  void
  
**Notes**
  You must have at least one namespace in a tenant. If a tenant contains only one namespace, the namespace cannot be deleted. 
  Deleting a namespace does not change the maximum number of allowed namespaces within a tenant. 

