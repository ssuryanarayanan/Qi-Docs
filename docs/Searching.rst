Searching for QiStreams
=======================

``GetStreams`` is an overloaded method that is also used to search for and return QiStreams (also see `QiStreams <http://qi-docs-rst.readthedocs.org/en/latest/Qi_Streams.html>`__ for information about using ``GetStream`` to return streams). When you call an overloaded method, the software determines the most appropriate method to use by comparing the argument types specified in the call to the method definition.

The syntax of the client libraries method is as follows:

::

  _metadataService.GetStreamsAsync(string searchText, int skip, int count);


The ``GetStreams`` overload returns QiStreams that match specific search criteria within a given tenant. 
You use the ``searchText`` parameter to specify a search string. The ``GetStreams`` method then returns any QiStreams that match the search string. The QiStreams returned are those in which the ``searchText`` is found in the stream ``name``, the stream ``description``, or in the ``tag`` list. 

For example, assume that a tenant contains the following Streams:

============    =========       ================     =========================
**streamId**    **Name**        **Description**      **Tags**
------------    ---------       ----------------     -------------------------
stream1         tempA           The temperature      “temperature”, “DeviceA”
                                from DeviceA                
stream2         pressureA       The pressure         “pressure”, “DeviceA”
                                from DeviceA     
stream3         calcA           calculation from     “temperature”, 
                                DeviceA values       “pressure”, “DeviceA”
============    =========       ================     =========================


Using the stream data above, the following table shows the results of a ``GetStreams`` call with different ``SearchText`` values:

==============     ========================================
**SearchText**     **Streams returned**
--------------     ----------------------------------------
“temp*”            stream1 and stream3 returned.
“calc*”            Only stream3 returned.
“DeviceA*”         All three streams returned.
“humidity*”        No streams returned.
==============     ========================================

The ``skip`` and ``count`` parameters determine which streams are returned when a large number of streams match the ``searchText`` criteria. 

The asterisk (*) character is a wildcard which matches zero or more characters (see Search operators_).  

``count`` indicates the maximum number of streams returned by the ``GetStreams()`` call. The maximum value of the ``count`` parameter is 1000. 

``skip`` indicates the number of matched stream names to skip over before returning matching streams. You use the skip parameter when more streams match the search criteria than can be returned in a single call. 

For example, assume there are 175 streams that match the search criteria: “temperature*”. 
The following call returns the first 100 matches:

::
 
   _metadataService.GetStreamsAsync(“temperature*”, 0, 100)

After the previous call, you can use the following call to return the remaining 75 matches, skipping over the first 100 matches because of the skip parameter set at 100):

::

   _metadataService.GetStreamsAsync(“temperature*”, 100, 100) 


Search operators
----------------

You can specify search operators in the ``searchText`` string to return more specific search results. 

.. _operators: 

=======  ============================================================
``+``    AND operator. For example, ``"cat+dog"`` searches for streams
         containing both "cat" and "dog".
``|``    OR operator. For example, "cat|dog" searches for documents
         containing either "cat" or "dog" or both.
``-``    NOT operator. For example, "cat –dog" searches for streams 
         that have the "cat" term or do not have "dog" 
``*``    Suffix operator. For example, "cat*" searches for streams 
         that have a term that starts with "cat", ignoring case.
``"``    Phrase search operator. For example, while Roach Motel 
         (without quotes) would search for documents containing 
         Roach Motel anywhere in any order, "Roach Motel" 
         (with quotes) will only match documents that contain the 
         whole phrase together and in that order.
``( )``  Precedence operator. For example, motel+(wifi|luxury) 
         searches for documents containing the motel term and 
         either wifi or luxury (or both).
=======  ============================================================

Qi Stream tags
--------------

QiStream objects support a collection of strings called *Tags*. Tags allow you to augment QiStreams with additional information, making it easier to classify, identify, and search for individual streams. The example below shows a stream being created that has the tags “Depth”, “Temperature”, and “operations” defined. 

