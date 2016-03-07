Introducing Qi
##############

The primary object of the Qi architecture is the *tenant*. Within a tenant you create one or more 
*Namespaces*, in which data types are defined and data is stored. 

.. image:: images/ContainersA.png

You use Namespaces to separate tenants into logical entities. For example, 
you might want to have one Namespace for production, one for development, and 
perhaps another to serve as a pre-production staging area where your QA 
group can run certification testing.

Within a Namespace, Qi defines three different objects in which to store and manage data:

-  **Type**: A user-defined structure that denotes a single measured event or
   object for storage.
-  **Stream**: A basic unit of storage consisting of an ordered series of
   objects that conform to a type definition.
-  **Stream Behavior**: Defines how Qi interpolates or extrapolates
   data during event retrieval when requests occur before, after, or between
   existing data events.

.. image:: images/Containers_1A.png

Each Namespace stores a separate and independent list of Type, Stream, and Stream Behavior objects.



Security
--------

There are two types of security accounts for Qi users:

+----------------+------------------------------------------------------------------+
| Account Type   | Description                                                      |
+----------------+------------------------------------------------------------------+
| Administrator  | Allowed to do all CRUD operations on Qi type, stream and stream  |
|                | behavior objects. Also allowed to read and write data to streams |
+----------------+------------------------------------------------------------------+
| User           | Allowed read operations on Qi objects and allowed to read data   | 
|                | from streams                                                     |
+----------------+------------------------------------------------------------------+

Getting started
---------------

Code samples for Python, .NET, Node.js, and Java can be found in the
Qi-Samples repository on GitHub. Obtain Qi REST API access keys from
`qi.osisoft.com <https://qi.osisoft.com>`__ before running the sample code.




