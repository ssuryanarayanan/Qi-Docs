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




