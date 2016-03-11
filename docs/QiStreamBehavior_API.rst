QiStream Behavior API calls
==================

The API calls in this section are all used to create and manipulate QiStream behaviors. See .. _Qi Types: https://github.com/osisoft/Qi-Docs/blob/Qi_Edits/docs/Qi_Types.rst for a list of supported QiTypes, a discussion of compound indexes, and general information about QiTypes. 


DeleteBehavior( )
------------

**Qi Client Library**

::

    void DeleteBehavior(string namespaceId, string behaviorId);
    Task DeleteBehaviorAsync(string namespaceId, string behaviorId);

**Http**

::

    DELETE Qi/{namespaceId}/Behaviors/{behaviorId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*behaviorId*: Id of the behavior to delete; the behavior must not be
associated with any streams

**Security** Allowed by administrator account

**Operation** Deletes the specified behavior Stream Behaviors objects
that are still associated with a stream cannot be deleted

GetBehavior( )
------------

**Qi Client Library**

::

    QiStreamBehavior GetBehavior(string namespaceId, string behaviorId);
    Task<QiStreamBehavior> GetBehaviorAsync(string namespaceId, string behaviorId);

**Http**

::

    GET Qi/{namespaceId}/Behaviors/{behaviorId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*behaviorId*: Id of the behavior definition to retrieve

**Security** Allowed by administrator and user accounts

**Operation** Gets a QiStreamBehavior object from service

GetBehaviors( )
------------

**Qi Client Library**

::

    IEnumerable<QiStreamBehavior> GetBehaviors(string namespaceId);
    Task<IEnumerable<QiStreamBehavior>> GetBehaviorsAsync(string namespaceId);

**Http**

::

    GET Qi/{namespaceId}/Behaviors

**Parameters**

*namespaceId*: The namespace identifier for the request

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all behavior objects

GetOrCreateBehavior( )
------------

**Qi Client Library**

::

    QiStreamBehavior GetOrCreateBehavior(string namespaceId, QiStreamBehavior entity);
    Task<QiStreamBehavior> GetOrCreateBehaviorAsync(string namespaceId, QiStreamBehavior entity);

**Http**

::

    POST  Qi/{namespaceId}/Behaviors

Content is serialized ``QiStreamBehavior`` entity

**Parameters**

*namespaceId*: The namespace identifier for the request

*entity*: A QiStreamBehavior object to add to Qi

**Security** Allowed by administrator account

**Operation** Creates a QiStreamBehavior (or returns it if it already
exists) If *entity* already exists on the server by *Id*, that existing
behavior is returned to the caller unchanged

UpdateBehavior( )
------------

**Qi Client Library**

::

    void UpdateBehavior(string namespaceId, string behaviorId, QiStreamBehavior entity);
    Task UpdateBehaviorAsync(string namespaceId, string behaviorId, QiStreamBehavior entity);

**Http**

::

    PUT Qi/{namespaceId}/Behaviors/{behaviorId}

Content is a serialization of the behavior to update

**Parameters**

*namespaceId*: The namespace identifier for the request

*entity*: Updated stream behavior

*behaviorId*: Identifier of the stream behavior to update

**Security** Allowed by Administrator account

**Operation** This method replaces the stream’s existing behavior with
those defined in the ‘entity’. If certain aspects of the existing
behavior are meant to remain, they must be included in entity.

An override list can be included in the ‘entity’ of this call to cause
the addition, removal or change to this list.

The Stream Behavior Id cannot be changed.
