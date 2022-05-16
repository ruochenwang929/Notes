# Week 1 -  Introduction to mobile and distributed computing

## Distributed computing

A computing paradigm where **a number** of **autonomous** entities (most likely **heterogeneous**)

which are **geographically distributed**

can **communicate and exchange messages**

through a **computer network**

to **achieve certain related tasks** (common goals)

## Vertical and Horizontal Distribution

Vertical distribution: placing logically different layers/components on different machines.

- Each layer on one single machine

Horizontal distribution: a single logical layer/ component is distributed across multiple machines to improve scalability

- E.g. distributing a database on multiple machines (distributed database)

## Mobile and Distributed Computing

It is a class of distributed computing systems

It integrates mobile and wireless devices into distributed systems

- Wireless sensors, wearables, smartwatches, smartphones, tables, and smart things (e.g. smart refrigerator)

Mobile computing is associated with mobility of hardware, users, data and software in computer applications

Mobile computing is possible because of wireless communication technologies:

- WiFi, WiMAX, Bluetooth, ZigBee, NFC, RFID, cellular networks, ...

## Distributed Computing Models

The client/server model

- Server processes offer some services to clients processes
- Usually there is a data storage at the backend

Peer-to-peer

- Each process logically equal to each other
- Data flows between the processes

## Distributed Computing and SOA (Service-Oriented Architecture)

Service-oriented architecture was introduced as a paradigm for distributed systems

- Software components are developed as independent modules
- A standard interface protocol between different modules, aka an application programming interface (API)
- Message based interactions through these interfaces
- Application functionalities are provided as loosely coupled services
- Reuse of services and composition of services
- Interoperability to support different platforms

## Web Services

- SOA is implemented by creating web services
- “A Web service is a piece of software/code designed to support interoperable machine-to-machine interaction over a network” (W3C)
- Web services provide an interface to make the functionalities/methods available to the public (other components or web services)
- Web services provide access to business logic, data and processes or other services
- Web services were originally implemented as SOAP web services and later evolved into RESTful web services (RESTful Web APIs)
  - SOAP is a messaging protocol (in XML)
- Today they can be created as smaller (more independent) components called microservices

## REST (REpresentational State Transfer)

RESTful web service were emerged based on the REST architecture’s concept (introduced by Roy Fielding)

REST slides are adopted from: Roy Fielding’s PhD thesis:

<http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm>

## REST is an architecture

REST is not a protocol, a technology, a standard, or a specification

The architecture consists of elements and relationships between these
elements

The REST architecture’s constraints that control the roles the roles/features of these elements and also their allowed relationships

- Architectural Constraints
- Interface Constraints

While REST is not a standard, it uses standards:

- HTTP
- URL
- XML/HTML/JSON/etc
- MIME Types or Media types such as text/xml, image/gif, application/json, audio/mpeg

## Architectural Constraints

- Client/Server
- Stateless
- Cache
- Uniform Interface
- Layered Systems
- Code-On-Demand

### Client-Server

- the client-server architectural style
- ‘Separation of concerns’:
  - separating the user interface concerns (e.g. of clients) from the data storage concerns (e.g. of servers)
  - allows the components to evolve independently
  - improve the portability of the user interface across multiple
platforms
  - improves scalability by simplifying the server components

### Stateless

- A constraint to the client-server interaction
- “communication must be stateless in nature”
  - each request from the client to the server must contain all the information necessary to understand the request
  - it cannot take advantage of any stored context on the server
  - Improves scalability and reliability

### Cache

- ...the data within a response to a request be implicitly or explicitly labelled as cacheable or non- cacheable.”
  - If a response is cacheable, the response can be reused for equivalent requests later
  - Improves network efficiency and performance
  - “reducing the average latency of a series of interactions”
  - decreases reliability (possibility of stale data)

### Uniform Interface

- all resources are accessed with a generic interface (e.g., HTTP GET, POST, PUT, DELETE)
  - PATCH is an HTTP method that enables updating part of the resources
  - HEAD is similar to GET but with no response body
  - OPTIONS is a request for information about the communication options
- the overall system architecture is simplified
- It is standardised and not specific to an application's needs

#### REST methods

Method:
GET, Retrieve a copy of a Resource (Idempotent)
DELETE, Remove a Resource (Idempotent)
POST, Create or sometimes update (Not Idempotent)
PUT, Update a Resource or sometimes create (Idempotent)

An idempotent method is a method that can be invoked once or many times and its effect will be the same (considering the state on the server side)

### Layered System

- allows an architecture to be composed of hierarchical layers
  - each component cannot "see" beyond the immediate layer with which they are interacting
  - Clients have no knowledge that services they invoke may also invoke other services
  - This can be used to improve scalability by enabling load balancing of services across multiple networks and processors

### Code-On-Demand

- an optional constraint
- “allows client functionality to be extended by downloading and executing code in the form of applets or scripts.”

## REST- Interface Constraints

1. identification of resources
2. manipulation of resources through representations
3. self-descriptive messages
4. hypermedia as the engine of application state (HATEOAS)

### Interface Constraints - Resources

REST is based on these notions:

- A resource
  - Any information that can be named can be a resource
  - A document or image, a service, a non-virtual object (e.g. a person)
  - Nouns instead of verbs

- A resource identifier
  - Each resource accessible by a URI/URL
  - A resource identifier (URI) identifies a particular resource
  - ‘Nice’ URIs

### REST Resources

- Each resource addressable by a URI
  - A bucket e.g. /restws.books
- GET /restws.books
  - Get all the items
- GET /restws.books/{an existing-id}
  - Get the item based on its id
- POST /restws.books (requires sending some data)
  - Create a new item
- PUT /restws.books/{an existing-id} (requires sending some data)
  - Update an existing item based on its id
- DELETE /restws.books/{an existing-id}
  - Delete an existing item based on its id

### Interface Constraints - Representation

- A **representation** of a resource is **a document capturing the current state of a resource**
- **A resource can have different representations** (e.g. JSON or XML)
  - Client can specify which representation can accept (e.g. in http accept header)
- REST (REpresentational **State Transfer**): each resource state has a representation, and this representation can be updated and transferred from the server to the client application

### JSON

JSON stands for JavaScript Object Notation

JSON is lightweight text-data interchange format

JSON is "self-describing" and easy to understand

JSON supports two structures:

**Objects**: a collection of name/value pairs
{"firstName": "John"}

A value can be a string, a number, true/false or null, or an object or an array (nested)

Arrays: an ordered list of values

    {"phoneNumber":[ 
      {
        "type": "home", "number": "212 555-1234" 
      },
      {
        "type": "fax", "number": "646 555-4567"
      } ] }

### JSON (cont’d)

Objects in name/value pairs , each name is followed by a colon

Data is separated by commas

Curly braces hold objects

Square brackets hold arrays

    {
      "firstName": "John", "lastName": "Smith", "age": 25, "address": { 
        "streetAddress": "21 2nd Street",
      "city": "New York",
      "state": "NY",
      "postalCode": 10021
      }, 
      "phoneNumber": [
      {
        "type": "home", "number": "212 555-1234" },
      {
      }] 
    }

### JSON Data Types

a string `{ "name":"John" }`

a number `{ "age":30 }`

an object (JSON object)

    "address": {
    "streetAddress": "21 2nd Street", 
    "city": "New York", 
    "state": "NY", 
    "postalCode": 10021 },

an array

    {"phoneNumber":[
    {
      "type": "home", "number": "212 555-1234"
    },
    {
      "type": "fax", "number": "646 555-4567"
    }]}

a boolean `{"sale":true}`

null `{"middlename":null}`

### Parsing JSON

JSON parsing online

http://json.parser.online.fr/

https://jsoneditoronline.org/

### Parsing JSON (cont’d)

There are libraries to create and parse JSON such as Google Gson libraries

In Android, we will use org.json libraries

`import org.json.JSONObject;`

The JSONObject class is used to create or parse JSON

    JSONObject jsonObject = new JSONObject(result); 
    JSONArray jsonArray = jsonObject.getJSONArray("items"); 
    if(jsonArray != null && jsonArray.length() > 0) {
      snippet =jsonArray.getJSONObject(0).getString("snippet"); 
    }

### REST-Interface Constraints

1. identification of resources
2. manipulation of resources through representations
3. **self-descriptive messages**
4. hypermedia as the engine of application state (HATEOAS)

#### Interface Constraints – Self-Descriptive Messages

self-descriptive messages

- Messages between components are self-descriptive to support intermediate processing
- Each message contains all the information and semantics necessary to complete the task