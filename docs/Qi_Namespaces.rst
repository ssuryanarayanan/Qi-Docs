Qi Namespace information
========================

A Qi tenant is divided into one or more Namespaces. Each Namespace is a logical entity 
within a tenant and holds its own set of Qi Types, Qi Streams, and Qi Stream Behaviors.
For more information see `Qi namespaces API <https://qi-docs.readthedocs.org/en/latest/Qi_Namespaces_API.html>`__.

Within a Qi Tenant you must have at least one Namespace in which to work.
You may create up to five namespaces within a tenant. If you use all five of your namespaces 
and want to create another, you can first delete an existing namespace and then create a new one. 
Contact OSIsoft if you require more than five namespaces within a tenant.

The following table shows the required and optional Qi namespace properties:

+---------------+-------------------------+----------------------------------------+
| Property      | Type                    | Details                                |
+===============+=========================+========================================+
| Id            | String                  | Required ID for referencing the        |
|               |                         | namespace                              | 
+---------------+-------------------------+----------------------------------------+

**Rules for namespaceId**

1. Not case sensitive
2. Spaces are allowed
3. Cannot start with two underscores ("\_\_")
4. Cannot contain forward slash or backslash characters ("/" or "\\")
5. Maximum length of 260 characters
6. Cannot start or end with a period.
7. Cannot contain consecutive periods.
8. Cannot consist of only periods. 

