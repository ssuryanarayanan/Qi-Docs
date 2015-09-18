This reference is a complete guide to the client reference API of the Qi Service.  We provide both the .NET Qi Libraries methods and the REST API.  Please refer to the readme docs within the language-specific folders for examples.

Each participant was allocated a tenant to represent their organization prior to the start of testing.  Each tenant is a self-contained entity within the Qi Service and may define the following entities:

* Type -- user defined structure denoting a single measured event or object for storage
* Stream -- basic unit of storage consisting of an ordered series of objects conforming to a type definition
* Stream Behavior -- defines whether interpolation occurs during event retrieval, and if so, how
