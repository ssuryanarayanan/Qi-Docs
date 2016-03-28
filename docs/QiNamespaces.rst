Qi Namespaces
#############

When you create a tenant in Qi, you define a Type (which defines the structure of your data), 
a Stream (which creates an area in which to store your data), and you define a Stream Behavior 
(which defines rules for how data is read). 

Tenant information is stored within one or more *namespaces*. A *namespace* in this context 
stores information for a given tenant and can be thought of as a self-contained partition 
that you use to store the entirety of the data and metadata for your tenant.

You use namespaces to separate tenants into logical entities. For example, you might 
want one namespace for production, one for development, and perhaps one or more 
namespaces for QA or to serve as a pre-production staging area for certification testing.

After OSIsoft assigns your tenant ID, you must create at least one namespace. You may create 
up to five namespaces within a tenant. Contact OSIsoft if you require more than five 
namespaces within a tenant.

You can create, delete, or obtain information about your namespaces using the Qi methods outlined in this topic.

The following table shows the required and optional Qi namespace objects:

+---------------+-------------------------+----------------------------------------+
| Object        | Type                    | Details                                |
+===============+=========================+========================================+
| Id            | String                  | Required ID for referencing the        |
|               |                         | namespace                              | 
+---------------+-------------------------+----------------------------------------+

**Rules for namespaceId**

1. Not case sensitive
2. Spaces are allowed
3. Cannot start with two underscores ("\_\_")
4. Cannot contain forward slash or backslash characters ("/" or "\\")
5. Maximum length of 260 characters


``GetNamespace( )``
-------------------

Retrieves an existing namespace.

**Syntax**

::

    QiNamespace GetNamespace(string tenantId, string namespaceId);
    Task<QiNamespace> GetNamespaceAsync(string tenantId, string namespaceId);

**Http**

::

    GET "Qi/{tenantId}/Namespaces/{namespaceId}”


**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request


Security
  Allowed by administrator and user accounts.

**Returns** 
  Returns a namespace.


``GetNamespaces( )``
----------------

Retrieves a list of existing namespaces.

**Syntax**

::


    IEnumerable<QiNamespace> GetNamespaces();
    Task<IEnumerable<QiNamespace>> GetNamespacesAsync();


*Http*

::

    GET "Qi/{tenantId}/Namespaces"


**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
  
Security
  Allowed by administrator and user accounts.

**Returns**
  Returns a list of namespaces.


``GetOrCreateNamespace( )``
----------------

Returns the namespace with the specified typeId and namespace, or creates the namespace if the namespace does not already exist. If the namespace exists, it is returned to the caller unchanged.

::

    QiNamespace GetOrCreateNamespace(string tenantId, QiNamespace qinamespace);
    Task<QiNamespace> GetOrCreateNamespaceAsync(string tenantId, QiNamespace qinamespace);

**Http**

::

    POST "Qi/{tenantId}/Namespaces/"


**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request

**Security**
  Allowed by administrator account.

**Returns** 
  Creates or returns a namespace.


``DeleteNamespace( )``
----------------

**Syntax**

::

    void DeleteNamespace(string tenantId, string namespaceId);
    Task DeleteNamespaceAsync(string tenantId, string namespaceId);

**Http**

::

    DELETE "Qi/{tenantId}/Namespaces/{namespaceId}”

**Parameters**

**Parameters**

``string tenantID``
  The tenant identifier for the request
``string namespaceId``
  The namespace identifier for the request
  

**Security** 
  Allowed by administrator account.

**Returns** 
  Deletes the namespace.


