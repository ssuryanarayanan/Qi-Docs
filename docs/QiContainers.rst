Qi Containers
#############

When you create a tenant in Qi, you define a Type (which defines the structure of your data), 
a Stream (which creates an area in which to store your data), and you define a Stream Behavior 
(which defines rules for how data is read). 

Tenant information is stored within one or more *containers*. A *container* in this context 
stores information for a given tenant and can be thought of as an self-contained partition 
that you use to store the entirety of the data and metadata for your tenant.

You use containers to separate tenants into logical entities. For example, you might 
want to have one tenant for production, one for development, and perhaps one or more 
containers for QA or to serve as a pre-production staging area for certification testing.

You can create, delete, or obtain information about your containers using the following Qi methods:

The following table shows the required and optional Qi Container objects:

+---------------+-------------------------+----------------------------------------+
| Object        | Type                    | Details                                |
+===============+=========================+========================================+
| ContainerId   | String                  | Required Id for referencing the        |
|               |                         | container                              | 
+---------------+-------------------------+----------------------------------------+

**Rules for ContainerId**

!!! This info needs to be verified !!!

1. Not case sensitive
2. Spaces are allowed
3. Cannot start with two underscores ("\_\_")
4. Cannot contain forward slash or backslash characters ("/" or "\\")
5. Maximum length of 260 characters


GetContainers( )
----------------

**Qi Client Library**

::

    IEnumerable<string> GetContainers();
    Task<IEnumerable<string>> GetContainersAsync();
    
    string GetContainer(string containerId);
    Task<string> GetContainerAsync(string containerId);


**Http**

::

    GET "Qi/Containers"

    GET "Qi/Containers/{containerId}”


**Parameters**

*containerId*: The Id of the container.

**Security** Allowed by administrator and user accounts.

**Operation** Returns ???


UpdateContainer( )
----------------

**Qi Client Library**

::

    void UpdateContainer(string containerId, string newContainerId);
    Task UpdateContainerAsync(string containerId, string newContainerId);

    
**Http**

::

    HTTP PUT "Qi/Containers/{containerId}"

**Parameters**

*containerId*: The Id of the container.

**Security** Allowed by administrator account

**Operation** Updates a container

GetOrCreateType( )
----------------

**Qi Client Library**

::

    string GetOrCreateContainer(string containerId);
    Task<string> GetOrCreateContainerAsync(string containerId);

**Http**

::

    POST "Qi/Containers/{containerId}"


**Parameters**

*containerId*: The Id of the container.

**Security** Allowed by administrator account

**Operation** Returns a Qi Type object. If the entity Id already exists on the
service, the existing type is returned to the caller unchanged.
Otherwise, a new type definition is added to the Qi Service

DeleteType( )
----------------

**Qi Client Library**

::

    void DeleteContainer(string containerId);
    Task DeleteContainerAsync(string containerId);

**Http**

::

    DELETE "Qi/Containers/{containerId}”

**Parameters**

*containerId*: The Id of the container.

**Security** Allowed by administrator account

**Operation** Deletes the type from service A. The type cannot be deleted from
the service if existing streams are associated with it.


