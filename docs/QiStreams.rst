Qi Streams
==========

A QiStream is the fundamental unit of storage in Qi. Each stream
represents an ordered series of events or observations for a particular
item of interest.

QiSTream management via the Qi Client Libraries is performed through the ``IQiMetadataService interface``, which may be accessed via the ``QiService.GetMetadataService( )`` helper.

The following table shows the required and optional QiStream properties:

+---------------+----------+-------------+--------------------------------------------+
| Property      | Type     | Optionality |Details                                     |
+===============+==========+=============+============================================+
| ID            | String   | Required    | An Identifier for referencing the stream.  |
+---------------+----------+-------------+--------------------------------------------+
| Name          | String   | Optional    | The name of the stream.                    |
+---------------+----------+-------------+--------------------------------------------+
| Description   | String   | Optional    | Text that describes the stream.            |
+---------------+----------+-------------+--------------------------------------------+
| TypeId        | String   | Required    | The type to be used for this stream.       |
+---------------+----------+-------------+--------------------------------------------+
| BehaviorId    | Sting    | Optional    | The stream behavior for this stream.       |
+---------------+----------+-------------+--------------------------------------------+
| Tag           | Sting    | Optional    | A collection of strings that permit        |
|               |          |             | classifying and identifying individual     |
|               |          |             | streams.                                   |
+---------------+----------+-------------+--------------------------------------------+

A stream is always referenced by its ID property. As shown in the preceeding table,
a QiStream must include a unique *ID* as well as a *TypeId* with the ID of
an existing QiType. The optional *BehaviorId* is set with the ID of an
existing stream behavior. When BehaviorId is omitted, the stream
will have a default behavior mode set to *continuous* and *extrapolation*
set to *all*. See
`QiStreamBehaviors <https://qi-docs.readthedocs.org/en/latest/QiStreamBehaviors/>`__
for more information.

**Rules for QiStream *ID*:**

1. Is not case sensitive.
2. Can contain spaces.
3. Cannot start with two underscores ("\_\_").
4. Cannot start or end with a period (".").
5. Can contain a maximum of 260 characters.
6. Cannot use the following characters: ( / : ? # [ ] @ ! $ & ' ( ) \* +
   , ; = %)
7. Cannot contain more than 250 periods (".").

GetStream
------------

**Qi Client Library**

::

    Task<QiStream> GetStreamAsync (string streamId);

**Http**

::

    GET Qi/{namespaceId}/Streams/{streamId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: String identifying the stream

**Security** Allowed by administrator and user accounts

**Operation** Returns a QiStream object

GetStreams
------------

**Qi Client Library**

::

    Task<IEnumerable<QiStream>> GetStreamsAsync ( );

**Http**

::

    GET Qi/{namespaceId}/Streams

**Parameters**

*namespaceId*: The namespace identifier for the request

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all streams

``GetStreamsAsync()`` is an overloaded method that is also used to search for and return QiStreams. See `Searching for QiStreams <https://github.com/osisoft/Qi-Docs/blob/Qi_Edits/docs/Searching.rst>`__ for more information.

::

   Task<IEnumerable<QiStream>> GetStreamsAsync (string searchText, int skip, int count);
  

**Http**

::

    GET Qi/{namespaceId}/Streams  

**Parameters**

*searchText*: The text you want to search for.

*skip*: The number of matched stream names to skip over before returning the matching streams.

*count*: The maximum number of streams to return. 

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all streams



GetOrCreateStream
------------

**Qi Client Library**

::

    Task<QiStream> GetOrCreateStreamAsync (QiStream entity);

**Http**

::

    POST Qi/{namespaceId}/Streams

Content is serialized QiStream entity

**Parameters**

*namespaceId*: The namespace identifier for the request

*entity*: Qi Stream object

**Security** Allowed by Administrator account

**Operation** If an entity with the same *Id* already exists on the service, then the
existing stream is returned to the caller unchanged. Otherwise the new
stream is created.

UpdateStream
------------

**Qi Client Library**

::

    Task UpdateStreamAsync(string streamId, QiStream entity);

**Http**

::

    PUT Qi/{namespaceId}/Streams/{streamId}

Content is serialized QiStream entity

**Parameters**

*namespaceId*: The namespace identifier for the request

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

The *UpdateStreamAsync* method applies to the entire entity. Optional fields
that are omitted from the entity will remove the field from the stream if the fields had
been set previously.

DeleteStream
------------

**Qi Client Library**

::

    Task DeleteStreamAsync(string streamId);

**Http**

::

    DELETE Qi/{namespaceId}/Streams/{streamId}

**Parameters**

*namespaceId*: The namespace identifier for the request

*streamId*: Identifier of the stream to delete

**Security** Allowed by Administrator account

**Operation** Delete stream using its stream id
