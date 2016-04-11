QiStreams
==========

A QiStream is the fundamental unit of storage in Qi. Each stream
represents an ordered series of events or observations for a particular
item of interest.

QiStream management via the Qi Client Libraries is performed through the ``IQiMetadataService`` interface, which may be accessed via the ``QiService.GetMetadataService( )`` helper.

The following table shows the required and optional QiStream properties:

+---------------+----------+-------------+--------------------------------------------+
| Property      | Type     | Optionality |Details                                     |
+===============+==========+=============+============================================+
| Id            | String   | Required    | An Identifier for referencing the stream.  |
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

A stream is always referenced by its Id property. As shown in the preceeding table,
a QiStream must include a unique *ID* as well as a *TypeId* with the ID of
an existing QiType. The optional *BehaviorId* is set with the ID of an
existing stream behavior. When BehaviorId is omitted, the stream
will have a default behavior mode set to *continuous* and *extrapolation*
set to *all*. See
`QiStreamBehaviors <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Stream_Behavior.html>`__
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
