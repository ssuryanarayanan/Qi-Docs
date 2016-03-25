Qi Namespaces
#############

When you create a tenant in Qi, you define a Type (which defines the structure of your data), 
a Stream (which creates an area in which to store your data), and you define a Stream Behavior 
(which defines rules for how data is read). 

Tenant information is stored within one or more *namespaces*. A *namespace* in this context 
stores information for a given tenant and can be thought of as a self-contained partition 
that you use to store the entirety of the data and metadata for your tenant.

You use namespaces to separate tenants into logical entities. For example, you might 
want to have one namespace for production, one for development, and perhaps one or more 
namespaces for QA or to serve as a pre-production staging area for certification testing.

You can create, delete, or obtain information about your namespaces using the following Qi methods:

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


GetNamespace( )
----------------

**Qi Client Library**

::

    QiNamespace GetNamespace(string namespaceId);
    Task<QiNamespace> GetNamespaceAsync(string namespaceId);

**Http**

::

    GET "Qi/Namespaces/{namespaceId}”


**Parameters**

*namespaceId*: The ID of the namespace.

**Security** Allowed by administrator and user accounts.

**Operation** Returns a namespace.


GetNamespaces( )
----------------

**Qi Client Library**

::


    IEnumerable<QiNamespace> GetNamespaces();
    Task<IEnumerable<QiNamespace>> GetNamespacesAsync();


**Http**

::

    GET "Qi/Namespaces"


**Parameters**

*namespaceId*: The ID of the namespace.

**Security** Allowed by administrator and user accounts.

**Operation** Returns a list of namespace.


GetOrCreateNamespace( )
----------------

**Qi Client Library**

::

    QiNamespace GetOrCreateNamespace(QiNamespace entity);
    Task<QiNamespace> GetOrCreateNamespaceAsync(QiNamespace entity);

**Http**

::

    POST "Qi/Namespaces/{namespaceId}"


**Parameters**

*namespaceId*: The ID of the namespace.

**Security** Allowed by administrator account.

**Operation** Creates or returns a namespace.

DeleteNamespace( )
----------------

**Qi Client Library**

::

    void DeleteNamespace(string namespaceId);
    Task DeleteNamespaceAsync(string namespaceId);

**Http**

::

    DELETE "Qi/Namespaces/{namespaceId}”

**Parameters**

*namespaceId*: The ID of the namespace.

**Security** Allowed by administrator account.

**Operation** Deletes the namespace.


