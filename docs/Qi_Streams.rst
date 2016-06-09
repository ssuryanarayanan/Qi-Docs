QiStream Information
====================

A QiStream is the fundamental unit of storage in Qi. Each stream
represents an ordered series of events or observations for a particular
item of interest.

QiStream management via the Qi Client Libraries is performed through the ``IQiMetadataService`` interface, which may be accessed via the ``QiService.GetMetadataService( )`` helper.

The following table shows the required and optional QiStream properties:

+---------------+------------------------------+-------------+--------------------------------------------+
| Property      | Type                         | Optionality |Details                                     |
+===============+==============================+=============+============================================+
| ID            | String                       | Required    | An Identifier for referencing the stream.  |
+---------------+------------------------------+-------------+--------------------------------------------+
| TypeId        | String                       | Required    | The type to be used for this stream.       |
+---------------+------------------------------+-------------+--------------------------------------------+
| Name          | String                       | Optional    | The name of the stream.                    |
+---------------+------------------------------+-------------+--------------------------------------------+
| Description   | String                       | Optional    | Text that describes the stream.            |
+---------------+------------------------------+-------------+--------------------------------------------+
| BehaviorId    | String                       | Optional    | The stream behavior for this stream.       |
+---------------+------------------------------+-------------+--------------------------------------------+
| Tag           | String                       | Optional    | A collection of strings that permit        |
|               |                              |             | classifying and identifying individual     |
|               |                              |             | streams.                                   |
+---------------+------------------------------+-------------+--------------------------------------------+
| Metadata      | iDictionary<string, object>  | Optional    | A dictionary for users to store information|
|               |                              |             | that is relevant to the stream             |
+---------------+------------------------------+-------------+--------------------------------------------+
| Indexes       | IList<QiStreamIndex>         | Optional    | Used to define secondary indexes for stream|
+---------------+------------------------------+-------------+--------------------------------------------+

A stream is always referenced by its Id property. As shown in the preceding table,
a QiStream must include a unique *ID* as well as a *TypeId* with the ID of
an existing QiType. The optional *BehaviorId* is set with the ID of an
existing stream behavior. When BehaviorId is omitted, the stream
will have a default behavior mode set to *continuous* and *extrapolation*
set to *all*. See
`QiStreamBehaviors <https://qi-docs-rst.readthedocs.org/en/latest/Qi_Stream_Behavior.html>`__
for more information.

**Restrictions and Limitations**

*QiStream ID*

1. Is not case sensitive.
2. Can contain spaces.
3. Cannot start with two underscores ("\_\_").
4. Can contain a maximum of 260 characters.
5. Cannot use the following characters: ( / : ? # [ ] @ ! $ & ' ( ) \* +
   , ; = %)
6. Cannot start or end with a period.
7. Cannot contain consecutive periods.
8. Cannot consist of only periods. 

*QiStream Metadata*

Use of TimeSpan objects in the QiStream Metadata dictionary is not supported.  Any numeric types stored in QiStream Metadata may be returned as ``long``.
