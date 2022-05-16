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
- Uniform Interface  Layered Systems
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
