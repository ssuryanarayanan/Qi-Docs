What's New
==========

What's new as of May 18th, 2016
-------------------------------

Some important changes were made to the Qi .Net Library that you should be aware of. As of May 18th, 2016, the IQiService C# interface will be deprecated and replaced with several new interfaces. The IQiService formerly included calls to manipulate Qi objects and to read and write data. Calls that were previously included in the IQiService C# interface were integrated into newly designed services and the IQiService will be depricated.

Interface name	Description
IQiAdministrationService	Includes methods that you use to manage QiNamespace objects within a tenant.
IQiMetaDataService	Includes methods that you use to manage QiTypes, QiStreams, and QiStreamBehavior objects within a namespace.
IQiDataService	Includes methods that you use to read and write data to QiStreams.

In addition to the new layout of methods shown in the previous table, all of the synchronous calls are also being deprecated. Before deprecating, each of the .NET library calls included a full complement of both synchronous and asynchronous overloads. After the library change, only the asynchronous overloads will remain. 

If you are currently using the IQiService your client code will still work, but you will receive compiler warnings to let you know you are using a call that will soon become unavailable. At your earliest possible convenience you should change your code to use the new services (IQiAdministrationService, IQiMetaDataService and IQiDataService) and remove the IQiService interface from your client code.

The libraries also include new methods and objects to assist with configuring user security roles within your client. QiService and QiSecurityHandler objects are now included in the .Net libraries.  

The QiService object is used to create service objects that implement each of the new interfaces. 







If you are new to the Qi .NET library:
--------------------------------------
Proceed to the documentation and samples for guidance in setting up and programming your Qi Clients with the Qi .NET Library

If you have already been using the Qi .NET library:
---------------------------------------------------

Your code will continue to work, but you will need to replace your use of the IQiServer C# Interface as outlined above. Specifically you will need to remove your instantiation of the IQiService C# interface and replace it with code that instantiates IQiAdministrationService, IQiMetaDataService and IQiDataService.  

To assist you in instantiated these new C# interfaces, the QI .NET libraries includes a QiService class. This class includes methods which can be used to quickly instantiate the new services. They are GetAdministratioService( ), GetMetadataService( ) and GetDataService( ). These calls accept URI, tenants, namespace and security information. Anew class called the SecurityHandler is now provided for you to set and pass security information to the Services in one easy step. 

Once you have modified your code to instantiate the new interfaces, you will need to replace the specific method calls that used the old IQiServer C# Interface.

Here are some steps that you might find useful when replacing these calls:

For each of the method calls that were previously made with IQiServer:
1.	Replace the use of IQIService with one of the new IQiAdministrationService, IQiMetaDataService and IQiDataService objects.
2.	At this point you should be able to find the method you need using the objects Completion Aids. The new services use the same method names as the old IQiServer, with the exception that the synchronous method overloads are no longer provided. All of the synchronous methods have an equivalent asynchronous method in the new Services. They simply include ‘Async’ at the end of the method name. For Example GetValue(…) becomes GetValueAsync(…).
a.	Also consider adding ‘.GetAwaiter().GetResult()’ to the end of the call as needed.  
i.	GetAwaiter() gets an ‘awaiter’ to await the completion of the task.  
ii.	GetResult() returns the result of the completed task.
3.	You will also notice that you must remove the passing of a TenantId and\or NamespaceId as parameters to these methods. The new methods do not require these.

*Example method call change:*

A call such as this:

``var event1 = _QiServer.GetDistinctValue<TypeClass>(_tenant.Id, _testNamespaceId, streamId, startIndexString);``

becomes this: 

``var event1 = _QiDataService.GetDistinctValueAsync<TypeClass>(streamId, startIndexString).GetAwaiter().GetResult();``



