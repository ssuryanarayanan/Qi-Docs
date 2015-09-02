#Welcome

The Qi Client Samples are introductory, language-specific examples of programming against PI Cloud Historian.  They are intended as instructional samples only.  The REST API will be supported over time.  While OSIsoft expects to offer langaue-specific client libraries when PI Cloud Historian is released, MVP developers should not expect that the client classes offered in the samples will not change.

#Sample Pattern

All the samples are console applications that follow the same sequence of events, allowing you to select the langauge with which you are most comfortable without missing any instructional features.  The pattern is:

1. Instantiate a Qi REST client
2. Obtain an authentication token
3. Create a Qi tenant object in the historian
4. Create a Qi type for a simple process event
5. Create a Qi stream in the historian representing a measured process
6. Create and insert events into the stream
7. Retrieve events for a specified time range
8. Retrieve and update an event
9. Delete an event
10. Close out the program

These steps illustrate the fundamental programming steps of Qi and the PI Cloud Historian.  Over time, we will extend the sample to demonstrate more advanced tasks.  Feel free to modify the samples and propose changes.

#Qi Classes

While you can program against the PI Cloud Historian using raw REST calls and JSON serialization, it is useful to have a class framework representing the basic building blocks of Qi and encapsulating the REST API.  In the future, this will probably be a standard framewrok maintained by OSIsoft.  For now, each sample includes basic classes to provide this feature:

1. QiClient -- a basic REST client object acting as the intermediary between local Qi objects and the remote historian
2. QiType -- a class for defining event types
3. QiStream -- a class representing a single stream of measurements conforming to a QiType

In addition, the sample code includes a QiEvent class that conforms to the QiType the sample will create.  This type consists of a string denoting the unit of measure, e.g., "deg C", a double value, and a timestamp.  This allows you to create new events for insertion into the a Qi stream in the historian using the Qi client object.

