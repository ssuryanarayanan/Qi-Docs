Qi Stream Behavior
==================

.. role:: raw-latex(raw)
   :format: latex
..

QiStreamBehavior dictates how data read operations will be performed by
defining whether or not interpolation and:raw-latex:`\or `extrapolation
will be applied when the index of a read operation falls between, before
or after stream data. QiStreamBehavior is an object that the user
defines and includes in the definition of a stream (similar to how a
QiType is defined for a QiStream). Note that if you do not assign a
specific Stream Behavior object to a stream, it will assume the default
behavior which is *Mode* = *Continuous* (with no overrides) and a
*QiStreamExtrapolation* = *All*.

The following table defines QiStreamBehavior objects:

+-----+-----+-----+
| Obj | Typ | Det |
| ect | e   | ail |
|     |     | s   |
+=====+=====+=====+
| Id  | Str | Uni |
|     | ing | que |
|     |     | ide |
|     |     | nti |
|     |     | fie |
|     |     | r   |
|     |     | use |
|     |     | d   |
|     |     | to  |
|     |     | ref |
|     |     | ere |
|     |     | nce |
|     |     | thi |
|     |     | s   |
|     |     | beh |
|     |     | avi |
|     |     | or  |
+-----+-----+-----+
| Nam | Str | Opt |
| e   | ing | ion |
|     |     | al  |
|     |     | des |
|     |     | cri |
|     |     | pto |
|     |     | r   |
+-----+-----+-----+
| Mod | QiS | Int |
| e   | tre | erp |
|     | amM | ola |
|     | ode | tio |
|     |     | n   |
|     |     | beh |
|     |     | avi |
|     |     | or  |
|     |     | set |
|     |     | tin |
|     |     | g   |
|     |     | to  |
|     |     | all |
|     |     | val |
|     |     | ue  |
|     |     | pro |
|     |     | per |
|     |     | tie |
|     |     | s   |
|     |     | (un |
|     |     | les |
|     |     | s   |
|     |     | ove |
|     |     | rri |
|     |     | dde |
|     |     | n   |
|     |     | by  |
|     |     | Ove |
|     |     | rri |
|     |     | de  |
|     |     | lis |
|     |     | t)  |
+-----+-----+-----+
| Ove | ILi | A   |
| rri | st< | lis |
| des | QiS | t   |
|     | tre | of  |
|     | amB | QiS |
|     | eha | tre |
|     | vio | amB |
|     | rOv | eha |
|     | err | vio |
|     | ide | rOv |
|     | >   | err |
|     |     | ide |
|     |     | ite |
|     |     | ms  |
|     |     | use |
|     |     | d   |
|     |     | to  |
|     |     | set |
|     |     | a   |
|     |     | dif |
|     |     | fer |
|     |     | ent |
|     |     | int |
|     |     | erp |
|     |     | ola |
|     |     | tio |
|     |     | n   |
|     |     | beh |
|     |     | avi |
|     |     | or  |
|     |     | tha |
|     |     | n   |
|     |     | the |
|     |     | mod |
|     |     | e   |
|     |     | to  |
|     |     | a   |
|     |     | typ |
|     |     | e   |
|     |     | pro |
|     |     | per |
|     |     | ty  |
+-----+-----+-----+
| Ext | QiS | Con |
| rap | tre | tro |
| ola | amE | ls  |
| tio | xtr | ext |
| nMo | apo | rap |
| de  | lat | ola |
|     | ion | tio |
|     |     | n   |
|     |     | beh |
|     |     | avi |
|     |     | or  |
|     |     | for |
|     |     | the |
|     |     | str |
|     |     | eam |
+-----+-----+-----+

**Rules for QiStreamBehavior *Id***

1. Case Insensitive
2. Allows spaces
3. Cannot start with two underscores ("\_\_")
4. Cannot contain slash characters ("/" and "")
5. Maximum of 260 characters

Stream behavior objects are always referenced by the *Id* property. A
stream can be changed to use a different stream behavior or the stream
behavior itself can be changed after it is created/configured.

**Methods effected by QiStreamBehavior**

+--------------------------------------------------------------------------------------------------------+---------------------------------------------+
| Method                                                                                                 | Details                                     |
+========================================================================================================+=============================================+
| `*GetValue( )* <https://qi-docs.readthedocs.org/en/latest/Reading%20data/#getvalue>`__                 |                                             |
+--------------------------------------------------------------------------------------------------------+---------------------------------------------+
| `*GetValues( )* <https://qi-docs.readthedocs.org/en/latest/Reading%20data/#getvalues>`__               |                                             |
+--------------------------------------------------------------------------------------------------------+---------------------------------------------+
| `*GetWindowValues( )* <https://qi-docs.readthedocs.org/en/latest/Reading%20data/#getwindowvalues>`__   | When Boundary is set to ExactOrCalculated   |
+--------------------------------------------------------------------------------------------------------+---------------------------------------------+
| `*GetRangeValues( )* <https://qi-docs.readthedocs.org/en/latest/Reading%20data/#getrangevalues>`__     | When Boundary is set to ExactOrCalculated   |
+--------------------------------------------------------------------------------------------------------+---------------------------------------------+

The code here shows the definition and creation of a simple
QiStreamBehavior:

::

    String behaviorid = “myFirstBehavior”;
    QiStreamBehavior simpleBehavior = new QiStreamBehavior()
    {
        Id = behaviorId,
            Mode = QiStreamMode.StepwiseContinuousLeading,
    };
    _service.GetOrCreateBehavior(createdBehavior);

Once the stream behavior is defined, the behavior can be applied to a
stream as shown here:

::

    string streamId = "MyFirstStream";
    string streamType = " mySimpleType ";
    QiStream stream1 = new QiStream()
    {
        Id = streamId,
        TypeId = streamType,
        BehaviorId = “MyFirstBehavior”
    }
    _service.GetOrCreateStream(stream1);

Interpolation
------------

When read methods effected by QiStreamBehavior (as shown above) are
given an index that lands between two values in a stream, it is the
*Mode* object that will determine what values will be retrieved. The
table below indicates how a stream will behave for the mode values
listed:

\|Mode\|Enumeration value\|Operation\| \|---\|---\|---\|
\|Default\|0\|Continuous\| \|Continuous\|0\|Interpolates the data using
previous and next index values\*\|
\|StepwiseContinuousLeading\|1\|Returns the data from the previous
index\| \|StepwiseContinuousTrailing\|2\|Returns the data from the next
index\| \|Discrete\|3\|Returns ‘null’\| \*Certain value types cannot be
interpolated

When *Mode* is set to continuous (or left at default) calls to read the
value of the QiStreamBehavior will return "0=Default”. Stream behavior
can also be used to give different mode settings to different data
properties within the stream’s type using overrides. For example, this
allows for a Discrete mode setting for one property and a Continuous
mode setting for another.

When the Stream Behavior is set to Continuous or Default read methods
attempt return an interpolated value for indexes that land between two
existing data events in a stream. This cannot always be done, for
example when the type is not numeric.

The table below describes how the Continuous or Default *Mode* effects
indexes between data in a stream:

***Mode* = Continuous or Default**

+-----+-----+-----+
| *Ty | Res | Com |
| pe* | ult | men |
|     | for | t   |
|     | an  |     |
|     | ind |     |
|     | ex  |     |
|     | bet |     |
|     | wee |     |
|     | n   |     |
|     | dat |     |
|     | a   |     |
|     | in  |     |
|     | a   |     |
|     | str |     |
|     | eam |     |
+=====+=====+=====+
| Num | Int | Rou |
| eri | erp | ndi |
| c   | ola | ng  |
| Typ | ted | is  |
| es  | \*  | don |
|     |     | e   |
|     |     | as  |
|     |     | nee |
|     |     | ded |
|     |     | for |
|     |     | int |
|     |     | ege |
|     |     | r   |
|     |     | typ |
|     |     | es  |
+-----+-----+-----+
| Tim | Int | Dat |
| e   | erp | eTi |
| rel | ola | me, |
| ate | ted | Dat |
| d   |     | eTi |
| Typ |     | meO |
| es  |     | ffs |
|     |     | et, |
|     |     | Tim |
|     |     | eSp |
|     |     | an  |
+-----+-----+-----+
| Nul | Ret | Can |
| lab | urn | not |
| le  | s   | rel |
| Typ | ‘nu | iab |
| es  | ll’ | ly  |
|     |     | int |
|     |     | erp |
|     |     | ola |
|     |     | te  |
|     |     | due |
|     |     | to  |
|     |     | pos |
|     |     | sib |
|     |     | ili |
|     |     | ty  |
|     |     | of  |
|     |     | a   |
|     |     | nul |
|     |     | l   |
|     |     | val |
|     |     | ue  |
+-----+-----+-----+
| Arr | Ret |     |
| ay  | urn |     |
| and | s   |     |
| Lis | ‘nu |     |
| t   | ll’ |     |
| Typ |     |     |
| es  |     |     |
+-----+-----+-----+
| Str | Ret |     |
| ing | urn |     |
| Typ | s   |     |
| e   | ‘nu |     |
|     | ll’ |     |
+-----+-----+-----+
| Boo | Ret |     |
| lea | urn |     |
| n   | s   |     |
| Typ | val |     |
| e   | ue  |     |
|     | of  |     |
|     | nea |     |
|     | res |     |
|     | t   |     |
|     | ind |     |
|     | ex  |     |
+-----+-----+-----+
| Enu | Ret | Thi |
| mer | urn | s   |
| ati | s   | may |
| on  | Enu | hav |
| Typ | m   | e   |
| es  | val | a   |
|     | ue  | val |
|     | at  | ue  |
|     | 0   | for |
|     |     | the |
|     |     | enu |
|     |     | mer |
|     |     | ati |
|     |     | on  |
+-----+-----+-----+
| GUI |     |     |
| D   |     |     |
+-----+-----+-----+
| Ver | Ret |     |
| sio | urn |     |
| n   | s   |     |
|     | ‘nu |     |
|     | ll’ |     |
+-----+-----+-----+
| IDi | Ret |     |
| cti | urn |     |
| ona | s   |     |
| ry  | ‘nu |     |
| or  | ll’ |     |
| Ien |     |     |
| ume |     |     |
| rab |     |     |
| le  |     |     |
+-----+-----+-----+

\*When extreme values are involved in an interpolation (e.g.
Decimal.MaxValue) the call may result in a BadRequest exception if the
interpolation cannot be completed successfully.

Extrapolation
------------

In addition to interpolations settings, stream behavior is also used to
define how the stream will extrapolate data. *ExtrapolationMode* acts as
a master switch to determine whether extrapolation will occur and at
which end of the data. When defined, *ExtrapolationMode* works with the
*Mode* to determine how a stream will respond to requests for an index
that precedes or follows all of the data in the stream.

The following tables show how *ExtrapolationMode* effects returned
values for each *Mode* value:

***ExtrapolationMode* with *Mode*\ =Default or Continuous**

+---------------------+---------------------+----------------------------+---------------------------+
| ExtrapolationMode   | Enumeration value   | Index before data          | Index after data          |
+=====================+=====================+============================+===========================+
| All                 | 0                   | Returns first data value   | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+
| None                | 1                   | Return null                | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Forward             | 2                   | Returns first data value   | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Backward            | 3                   | Return null                | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+

***ExtrapolationMode* with *Mode*\ =Discrete**

+---------------------+---------------------+---------------------+--------------------+
| ExtrapolationMode   | Enumeration value   | Index before data   | Index after data   |
+=====================+=====================+=====================+====================+
| All                 | 0                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+
| None                | 1                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+
| Forward             | 2                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+
| Backward            | 3                   | Return null         | Return null        |
+---------------------+---------------------+---------------------+--------------------+

***ExtrapolationMode* with *Mode*\ =StepwiseContinuousLeading**

+---------------------+---------------------+----------------------------+---------------------------+
| ExtrapolationMode   | Enumeration value   | Index before data          | Index after data          |
+=====================+=====================+============================+===========================+
| All                 | 0                   | Returns first data value   | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+
| None                | 1                   | Return null                | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Forward             | 2                   | Returns first data value   | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Backward            | 3                   | Return null                | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+

***ExtrapolationMode* with *Mode*\ =StepwiseContinuousTrailing**

+---------------------+---------------------+----------------------------+---------------------------+
| ExtrapolationMode   | Enumeration value   | Index before data          | Index after data          |
+=====================+=====================+============================+===========================+
| All                 | 0                   | Returns first data value   | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+
| None                | 1                   | Return null                | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Forward             | 2                   | Returns first data value   | Return null               |
+---------------------+---------------------+----------------------------+---------------------------+
| Backward            | 3                   | Return null                | Returns last data value   |
+---------------------+---------------------+----------------------------+---------------------------+

For additonal information on the effect of stream behaviors, see the
documentation on the `read
method <https://qi-docs.readthedocs.org/en/latest/Reading%20data/>`__
you are using.

Overrides
------------

As described above, the interpolation behavior for the values in a
stream is determined by the stream behavior *Mode*, however individual
data types can be overridden to conform to another behavior by setting
the *Overrides* property. In this way the user can have different
interpolation behaviors for different types within the stream. Without
the overrides, properties will get the interpolation behavior defined by
the *Mode* object of the stream behavior.

The *Override* object has the following structure:

::

    string QiTypePropertyId
    QiStreamMode Mode

Note that when using the override list the *Mode* setting of Discrete
cannot be overridden. If the *Mode* is set to Discrete a null value is
returned for the entire event. If a Discrete setting is desired for one
of the types within a stream and a different setting (e.g.
StepwiseContinuousLeading) is desired for other properties within the
stream, set the *Mode* to StepwiseContinuousLeading and use the override
list to set the desired property to Discrete.

DeleteBehavior( )
------------

**Qi Client Library**

::

    void DeleteBehavior(string behaviorId);
    Task DeleteBehaviorAsync(string behaviorId);

**Http**

::

    DELETE Qi/Behaviors/{behaviorId}

**Parameters**

*behaviorId*: Id of the behavior to delete; the behavior must not be
associated with any streams

**Security** Allowed by administrator account

**Operation** Deletes the specified behavior Stream Behaviors objects
that are still associated with a stream cannot be deleted

GetBehavior( )
------------

**Qi Client Library**

::

    QiStreamBehavior GetBehavior(string behaviorId);
    Task<QiStreamBehavior> GetBehaviorAsync(string behaviorId);

**Http**

::

    GET Qi/Behaviors/{behaviorId}

**Parameters**

*behaviorId*: Id of the behavior definition to retrieve

**Security** Allowed by administrator and user accounts

**Operation** Gets a QiStreamBehavior object from service

GetBehaviors( )
------------

**Qi Client Library**

::

    IEnumerable<QiStreamBehavior> GetBehaviors();
    Task<IEnumerable<QiStreamBehavior>> GetBehaviorsAsync();

**Http**

::

    GET Qi/Behaviors

**Parameters**

None

**Security** Allowed by administrator and user accounts

**Operation** Returns IEnumerable of all behavior objects

GetOrCreateBehavior( )
------------

**Qi Client Library**

::

    QiStreamBehavior GetOrCreateBehavior(QiStreamBehavior entity);
    Task<QiStreamBehavior> GetOrCreateBehaviorAsync(QiStreamBehavior entity);

**Http**

::

    POSTQi/Behaviors

Content is serialized ``QiStreamBehavior`` entity

**Parameters**

*entity*: A QiStreamBehavior object to add to Qi

**Security** Allowed by administrator account

**Operation** Creates a QiStreamBehavior (or returns it if it already
exists) If *entity* already exists on the server by *Id*, that existing
behavior is returned to the caller unchanged

UpdateBehavior( )
------------

**Qi Client Library**

::

    void UpdateBehavior(string behaviorId, QiStreamBehavior entity);
    Task UpdateBehaviorAsync(string behaviorId, QiStreamBehavior entity);

**Http**

::

    PUT Qi/Behaviors/{behaviorId}

Content is a serialization of the behavior to update

**Parameters**

*entity*: Updated stream behavior

*behaviorId*: Identifier of the stream behavior to update

**Security** Allowed by Administrator account

**Operation** This method replaces the stream’s existing behavior with
those defined in the ‘entity’. If certain aspects of the existing
behavior are meant to remain, they must be included in entity.

An override list can be included in the ‘entity’ of this call to cause
the addition, removal or change to this list.

The Stream Behavior Id cannot be changed.
