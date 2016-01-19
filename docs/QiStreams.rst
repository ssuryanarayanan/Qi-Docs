Qi Streams
==========

A QiStream is the fundamental unit of storage in Qi. Each stream
represents an ordered series of events or observations for a particular
item of interest.

The following table shows the required and optional QiStream objects:

+---------------+----------+-------------+--------------------------------------------+
| Object        | Type     | Optionality |Details                                     |
+===============+==========+=============+============================================+
| Id            | String   | Required    | Identifier for referencing the stream      |
+---------------+----------+-------------+--------------------------------------------+
| Name          | String   | Optional    | Name of the stream                         |
+---------------+----------+-------------+--------------------------------------------+
| Description   | String   | Optional    | Text that describes the stream             |
+---------------+----------+-------------+--------------------------------------------+
| TypeId        | String   | Required    | The type to be used for this stream        |
+---------------+----------+-------------+--------------------------------------------+
| BehaviorId    | Sting    | Optional    | The stream behavior for this stream        |
+---------------+----------+-------------+--------------------------------------------+

A stream is always referenced by its Id property. As shown in the preceeding table,
a QiStream must include a unique *Id* as well as a *TypeId* with the Id of
an existing QiType. The optional *BehaviorId* is set with the Id of an
existing stream behavior. When BehaviorId is omitted, the stream
will have a default behavior mode set to continuous and extrapolation
set to all. See
`QiStreamBehaviors <https://qi-docs.readthedocs.org/en/latest/QiStreamBehaviors/>`__
for more information.

**Rules for QiStream *Id*:**

1. Not case sensitive
2. Spaces are allowed
3. Cannot start with two underscores ("\_\_")
4. Cannot start or end with a period (".")
5. Maximum of 260 characters
6. Cannot use the following characters: ( / : ? # [ ] @ ! $ & ' ( ) \* +
   , ; = %)
7. No more than 250 periods (".") allowed

GetStream( )
------------

**Qi Client Library**

::

    QiStream GetStream(string streamId);
    Task<QiStream> GetStreamAsync (string streamId);

**Http**

::

    GET Qi/Streams/{streamId}

**Parameters**

*streamId*: String identifying the stream

**Security** Allowed by administrator and user accounts

**Operation** Returns a QiStream object

GetStreams( )
------------

**Qi Client Library**

::

    IEnumerable<QiStream> GetStreams ();
    Task<IEnumerable<QiStream>> GetStreamsAsync ();

**Http**

::

    GET Qi/Streams

**Parameters**

None

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all streams

GetOrCreateStream( )
------------

**Qi Client Library**

::

    QiStream GetOrCreateStream (QiStream entity);
    Task<QiStream> GetOrCreateStreamAsync (QiStream entity);

**Http**

::

    POST Qi/Streams

Content is serialized QiStream entity

**Parameters**

*entity*: Qi Stream object

**Security** Allowed by Administrator account

**Operation** If an entity with the same *Id* already exists on the service, then the
existing stream is returned to the caller unchanged. Otherwise the new
stream is created.

UpdateStream( )
------------

**Qi Client Library**

::

    void UpdateStream(string streamId, QiStream entity);
    Task UpdateStreamAsync(string streamId, QiStream entity);

**Http**

::

    PUT Qi/Streams/{streamId}

Content is serialized QiStream entity

**Parameters**

*streamId*: Identifier of the stream to modify

*entity*: Updated stream object

**Security** Allowed by Administrator account

**Operation** Changes the stream to hold the properties in the QiStream
entity given. Permitted changes:

• Name

• BehaviorId

• Description

An exception is thrown on unpermitted change attempt (and the stream is
left unchanged)

The *UpdateStream()* method applies to the entire entity. Optional fields
that are omitted from the entity will remove the field from the stream if the fields had
been set previously.

DeleteStream( )
------------

**Qi Client Library**

::

    void DeleteStream(string streamId);
    Task DeleteStreamAsync(string streamId);

**Http**

::

    DELETE Qi/Streams/{streamId}

**Parameters**

*streamId*: Identifier of the stream to delete

**Security** Allowed by Administrator account

**Operation** Delete stream using its stream id
