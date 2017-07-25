# Microservices

## Basics

### Why?

* They are small - helping to divide a big system in smaller units
* They are loosely coupled over REST or messaging
* Shared-nothing state - they help avoiding the erosion of an architecture, when classes are used wrongly and dependencies are build up between modules.
* They enforce standards for consuming and emitting data
* They enforce a single standard for piping data from one process to another.
* They are exchangeable (flexibility to add or change modules later)
* They can be implemented in different technologies stacks
* they can have different performance requirements (CPU, space or such)
* New technologies can be tested in single microservices (minimized risk)
* Deployment from changes is possible without deployment of the others
* Different teams can work on them (with different security levels)
* they can be tested on itself
* Scalibility is possible for each microservices on its own
* team members only need to understand the business logic / technology of the microservice(s), they work on and the interaction between the services.
* a failure in one microservice should not affect an other microservice

### Problems

* Working on the architecture is more important (there are complicated relationships between micro services)
* Refactoring of microservies could be a problem
* An overhead is added, the system gets more complex regarding deployment and monitoring.
* Serialization and deserialization of objects is needed
* New infrastructure services for logging or monitoring needed
* Automation is needed because manual work would be to complex / laborious
* Calls over the network are clearly slower then calling functions the same process
* Calls over the network could lead to (new) problems
* Interchangability between teams / services could be a problem because of different knowledge / technologies
* Stubs could be needed for local testing

### Thoughts on size of a microservice

* The size of the microservice depends on a functional and sustainable architecture -there should be no refactoring from functions to other micro services
* Communucation with other micro servies should be avoided (Martin Fowler: First Law of Distributed Object Design: Don’t distribute your objects)
* The more (smaller) microservices, the more infrastructure is needed!
* A microservice should be not bigger as a single agile team (3-9 persons) can handle
* Every microservice should be deployable on its own with a Continuous-Delivery-pipeline and the needed infrastructure
* As smaller the microservice, as easier is the replacement
* Microservices should respect the ACID-characteristics of a transaction, this is harder between different microservices - but can happen with messaging-systems:
  * atomicity (will be completed alltogether or not at all. An error will lead to an rollback)
  * consistency (the data is valid before and after every transaction)
  * isolation (the operations of the transaction can be separated)
  * durability (the changes are persistent)
* Microservices are too small, if transactions and the consistency of data are depending on several microservices.
* "Kompensationstransaktionen" could be helpful for transaction consistency - they are rolling back transactions in form of a new business process (for example "Cancelations" or "Refunds" of a former transaction)

### Conway's law

* "organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations"
* The law is based on the reasoning that in order for a software module to function, multiple authors must communicate frequently with each other. In a monolith system everybody needs to talk to everybody to avoid problems. The coordination gets easier with modularization and interfaces as people only have to talk to people regarding the interfaces, they work with.
* Microservices are good for smaller, cross-functional teams as Scrum is demanding - they can talk about the used technology stack and the interfaces. They avoid coordination problems in bigger companies
* Business units should correlate to the architecture of the microservices - having a IT & business microservice unit, makes independencies really small and the coordination gets less problematic. Also the work on parallel features gets easier.

### Domain-driven Design (DDD)

* a collection of pattern, useful for microservices because it helps stucturing microservices on business functions. 
* Every microservice should be a functional unit - business logic changes should only change one microservice
* Ubiquitous Language: The software should use the same names as the domain experts - in the code, in the variable names and database schemata to make sure, that the important elements of the domain are included in the Software (e.g. "1-Click-Order" vs. Order-table with a flag "fast")
* Basic types of the DDD:
  * ENTITY - an object with its own identity (e.g. a customer or a product)
  * VALUE OBJECTS - objects with NO own identify (e.g. an address - it is only useful in a context with a customer)
  * AGGREGATES - composed domain objects to use rules or such (e.g. an Order could be an aggregate of order rows)
  * SERVICES - containing business logic using the objects (e.g. the order process, using Products, Customers and an order)
  * REPOSITORIES - the summary of all entitites (e.g. in a database)
  * FACTORIES - for complex bsiness objects with many associations. 
* Aggregates are used for consistency rules and can not be shared between two microservices. 
* Bounded Contexts: 
  * Domain models are only useful in some parts of a system. Complex systems have several bounded contexts.
  * It mirrors the complexity of the real world, where objects have different meanings in different contexts. It is the opposite of the idea to use standarized canonic models (CDM). It is okay to use different attributes in different contexts instead of trying to discuss a standard.
  * Example: To give credits in a bonus program, the CRM needs the program number, the delivery module needs the size and weight of the product, the delivery address and the used delivery company, the billing service the billing address and the tax.
  * Every bounded context has an own model of the Customer. This makes microservices changeable independently.  
  * A CONTEXT MAP can give a overview of the bounded contexts.
* Bounded Context Collaboration:
  * It could be useful, to save base information of a customer in a separate service - used by different service
  * More dependent types: SAME KERNEL (sharing the context), SHARED KERNEL (share some common elements), CUSTOMER/SUPPLIER (the caller decided on the model), CONFORMIST (the caller uses the imposed model) - teams need to discuss more about collaborations 
  * More independent types: ANTICORRUPTION LAYER (translations loose models), SEPERATE WAYS (independent models), OPEN HOST SERVICE (a service for everybody to create its own integration), PUBLISHED LANGUAGE (it provides a common language).
* Large-scale structure (how can the overall system be viewed?)
  * SYSTEM METAPHOR: functional processes base structure (e.g. Search, Compare and Order for an E-Commerce Shop)
  * RESPONSIBILITY LAYER: Defining layers, which can call other layers in one direction (e.g. Catalog, Order process and billing process in that order).
  * EVOLVING ORDER: The stucture is not given
  
### Microservices with UI?

* An UI can help to focus on business terminoloy and to provide the possibility to change functionality.
* An RESTful HTTP interface could render JSON as well as HTML UI or Javascript and a Single Page App
* A Self-contained system (SCS) would divide the system vertical by functionality - horicontal divisions would be technical (like UI, logic and persistence).This could be a registration or an order process.
* UI can be in an own artifact, if microservices get to big - but UI and logic should stay in the same team
* historically there was a three-level-architecture: Webserver (UI) -> Middle Tier (EJB, CORBA) -> Database

### Microservices and SOA (service-oriented architecture)

* It contains a business functionality and can used independently, it is available in a network and has a interface which can be used from other rservices. The service is registered in a directory. 
* There is an integration platform with its own intelligence to enable the communication between the sevices (orchestration)
* There is one or many portals with an UI for different user types
* microservices lay on isolation - a user interaction is processed ideally in just one microservice - SOA divides the logic on the portal, orchestration and the single services.
* the architecture of a SOA takes care of the enterprise as a whole - it defines how systems interact. Microservices are an architecure for one single system. 
* decisions on a microservice architecure are project based, SOA decisions affect the company as a whole
* the communication is different. Microservice do use simple communication without own intelligence. They call each other or send messages. There is no integration layer with a logic.
* microservices have their own UI, SOA has portals as a universal UI for the services

### Why should we use APIs over rpcs?

* One service running (instead of run_nameko)
* authentications build in, use via HTTP Headers possible
* use of versions
* use of caching
* documentation and Schema generation (with Swagger) - Nameko would need some kind of Service registry
* Throttling to avoid amok runs
* Get a direct response - status code (200) or some failure (500) or HTTP Timeouts
* flexibility to have a more public service
* They could answer either synchron or asynchronously via a Parameter
* We can use Celery locally on each service
* SSL/TLS
* HATEOAS with links between ressources for flexibility of structure
* Services can be moved (301, 302)
* Other answers possible with Accept-Header (JSON, XML, etc.)
* 202 Accepted: The request has been accepted for processing
* Resources
  * https://capgemini.github.io/architecture/is-rest-best-microservices/
  

### Why should we not use APIs?

* No fire & forget!  Which HTTP method to use?
* Side effects with differen HTTP methods
* In failure case can not retry, because of non-itempotency

### Why do we have a Monolith repo with all microservice?

* Changes, which need deployments on different microservices, are easier
* Makes sharing libraries between microservices easier
* Documentation and integration tests such can be shared
* Less pull requests, when changes to all microservices (deployment scripts e.g.)
* http://danluu.com/monorepo/

### Best Practices

* Netflix
* Amazon
  * services for search, recommendations etc. - integrated in the UI
  * every team has to run the service, they build. Slogan: "You build it, you run it"). 
* DeliveryHero: lymph
  * https://github.com/deliveryhero/lymph
  * Discovery: pluggable service discovery (e.g. backed by ZooKeeper)
  * RPC: request-reply messaging (via ZeroMQ + MessagePack)
  * Events: pluggable and reliable pub-sub messaging (e.g. backed by RabbitMQ)
  * Process Management
* Nameko
  * https://github.com/nameko/nameko
  * AMQP RPC and Events (pub-sub)
  * HTTP GET, POST & websockets
* Slides and other resources
  * [PDF: REST vs. Messaging](files/REST_VS._Messaging.pdf)
  * https://github.com/mfornos/awesome-microservices/

## Microservice design

### Basics

* The design should be the same - installation
* There should be an standarized monitoring for logging and technologies
* They should give informations about themselve as documentation and metadata
* Code redundancy is okay and it leads to independency

### Functional architecture

* Do not divide services by objects, for example "Customer", "Products" and "Orders". This conflicts with the Bounded Context idea, because a common modeling is impossible. 
* Circular dependencies should be avoided (e.g. order process <-> billing service)
* Start big with only some big microservices and create microservices step by step - it makes the shift of code easier- As more teams are build up, more and more microservices can be built as well. 
* it should be really easy to build up a new microservice to avoid a microservice becoming monolithic over time (e.g. an registration service which an external, asynchronous age verification and mixed up customer and process data)

### Technical architecture

* a microservice should only build on one business case (bounded context). A microservicde should not be just for the orchestration of other microservices with own logic to avoid changes to be done on severl microservices at once.
* Microservices should be stateless so that they can be exchanged without problems

### Tools for management and visualisation

### When are changes of the architecture needed?

* To Big: When the microservice is to big that one team can not work on it or if it contains more then one bounded context
* Functionality must be moved: When functionality belongs to an other microservice, maybe when the communication gets to much between to microservices. Then they are not loosely coupled anymore.
* Code sharing: Functionality needs to be used by different microservices -> common library

### Transfering legacy systems intro microservices

* The microservices can call legacy systema as a blackboc
* More and more functionalities can be relocated into microservices (without understanding the code in detail)
* an solution would be a Message router, which decides if requests will be handled by the legacy system or a (new) microservice
* There are several Enterprise Integration Patterns: a Message router, a content based router (routing depends on the content), a message filter, a message format translator, conrent enricher or content filter (adding or deleting informations)
* it should be avoided to use the same database, since changes would need to be done on both sides
* No big Bang! 

### Event-driven Architecture (EDA)

* Example: A order process service, which calls the billing service and the delivery process service
* When calling the services, the caller needs to know the services of the called services.
* With EDA, only an event is sent by the service (Event Emitter). All interested microservices (Event Consumers) can process the order
* New microservices can register, no modification is needed in the order process service

## Technical aspects

### Coordination and configuration

* Consistency: All nodes give back the same data. If there is an outage, according to the CAP-theorem there can be only an inconstent answer - or none. 
* This can be used for Locking - a config value is set and only one service can run the code at once
* a configuration service is an discrepancy to the immutuable server idea. When the config changes, the server gets changed. This makes error searches more problematic
* Good are installation toools, which create configuration from a centralized service, which microservices read. This enables a central configuration AND the concept of a immutable server

#### etcd

* Used by Docker & Kubernetes
* https://coreos.com/etcd / https://github.com/coreos/etcd/
* etc sometimes gives rather wrong answers instead of no answers

#### Consul

* https://www.consul.io / http://txt.fliglio.com/2014/05/encapsulated-services-with-consul-and-confd/
* provides an HTTP/JSON interface
* also offers Service discovery and health checks

### Service discovery

* They make sure, that services find each other
* It must be dynamic, because microservices crash or they get deployed from time to time
* one microservice is not bind to one other, it can contact one or another depending on the work load
* an registration helps with an overview of the architecture and helps with monitoring
* service discovery can be done with configuration solutions - but a discovery needs even a better reliability and the trade off between consistency and availibility is different

#### DNS (Domain Name System)

* its hierarchy is good for the independency of the services
* Reliable, fast, is scalable because of caching
* DNS has SRV-records for Ports, with prioritys and weights for servers - this supports reliability and load balancing
* BIND
* Eureka from Netflix stack

### Load balancing

* appear to be one instance but orchestras the load on many instances
* messages can shared to 1 receiver (point-to-point) or with all receivers (publish / subscribe)
* good for deployment - new versions can start slow and become (more) requests, when everything is good
* the load balancer gets information from the services, it also can remove servers, when they do not answer on requests
* can become a bottleneck, a outage leads to outages of the microservices
* therefore there should be load balancer for each microservice
* Nginx can configured as load balancer, it can deliver CSS and images as well
* HaProxy is a solution for TCP
* Service discovery can be used to include load balancing - this can be done with DNS and a time out for DNS data

### Scalibility

* needed to work on more load
* horizontal: more ressources, vertical: more powerful ressources (usually horizontal is better because its cheaper to have more slower computers instead of a less faster ones)
* microservices needs to have NO specific state for a user - a state needs to be saved in a database or in-memory-store
* Sharding, for example on customer names / numbers with specific endings
* response times: horizontal scaling does not help, vertical scaling could help; caches could be used;  

### Security

* every microservice needs to know, who called
* authentication: Identity of the user (should be implemented on a central service)
* authorisation: decision, if a user can take a action (every microservice should decide on this)
* the security access definitions for users are saved in the microservice to be more flexible - an authentication server should only take care of user groups
* a solution for this is OAuth2, it works great with HTML - an other solution is Kerberos and SAML or SAML 2.0 (for XML and HTTP)
* the communication should be saved with SSL/TLS
* save less informations (data economy)

#### OAuth2

*Basics*

* Client asks the Resource Owner, if it gets access on a ressource (e.g. the profile)
* Client gets an answer
* Client uses the the answer from the RA to start a request on the authorization server
* The authorization server gives back an ACCESS TOKEN
* With this ACCESS TOKEN, a client cann call a resource server to get informations. This can be done in a HTTP Header
* The resource server gives an answer

*Different processes*

* Password: In step 1 - can be a problem, if the client is insecure
* Authorization: Redirect to a webpage; user logs in on the authorization server, the client gets a Code back, which it can use to get a Access token; the client is a backend app
* Implicit: as Authorization, but the client gets a access token (more insecure but needed if it is a mobile app or javascript code=
* Client credentials: Using a key to get the access token from the authorization server (for offline access)

*Access token*

* needs to be secured, because all resources can be used with it
* The Taken can be encoded with informations like the validating server, the user name, the user group and the access level - it gets encoded with BASE64, is used in a header and has size restrictions. 
* JSON web Token (JWT) is a standard for informations, to validate it, a digital signature is neeeded with JSon Web Signature.
* Also JSON Web Encryption (JWE) can be used.

*Process in microservices*

* User is authenticated within one service
* The access token is passed over to other microservices 
* With JWT the access token gets decrypted and the signature is validated 
* The information in the token are used to decide, if the user has access

#### Intrusion detection

* Firewalls could help to avoid intruders to use other microservices as well
* This could be handled with monitoring

#### Hashicorp Vault
 
* https://www.hashicorp.com/blog/vault.html
* a good tool for securing microservices
* certificates and secrets can be saved
* Secrets can be leased to services - they can be invalidated
* Data can be encrypted or decrypted with keys without a microservice saving the key itself
* Accesses get saved with an Audit (who got when which secret?)
* In the background uses HSMs, SQL databses or Amazon IAM to save secrets
* Vault helps the work with keys - it is really hard to implement those things secure on your own

#### Other security aspects

* integrity: There should be non undocumented changes on data - data should be signed for example
* obligation: Changes should not be disputed by a user - data should be signed with an user key. The architecture must provide the key, the signature is part of the servide
* date security: The data should not get lost. Backup and high availability are needed. 
* availability: every micro service must take care of this, resilience helps

### Documentation and Metadata

* Provide Meta information of the microservice (name, responsible persons, source code informations, libraries, feature toggles and config parameters)
* have the documentation be generated automatically by the information you have

### Shared code base

* Code libraries could be okay - but they should not contain domain business objects, 
* authentication is an example for a function, all services are using
* coordination is needed 
* easier for technical code (for example creation of metrics)

## Communication between microservices

### Basics

* The system needs to be reliable even when single microservices have an outage. The outage should not influence the other services - there should not be a outage chain
* It needs to be decided what happens at a outage
  * Is an cache okay, when old data is presented? 
  * Is there a local algorithm, which can be used instead of a external microservide?
* These are business decisions. Example is an ATM service asking a user service for the account balance of a user
  * The ATM could refuse to give money - this is secure but the user could be angry and there is a loss of revemue
  * The ATM could give money to a limit 
* Timeouts need to be low 
* Different integration types:
  * Via UI 
  * Via Logic with REST, SOAP or RCP
  * Via date replication
  
### Web and UI

#### Single-Page-Apps

* microservices should have their own UI. For the overall system the UI of all micro services needs to be integrated
* A single-Page-App integrated all in one page - the logic is built in javascript; URLs in the browser can be manipulated so that browser features and bookmarks can be used - SPAs are pushing HTML as web technology to the side
* SPAs have advantages with complex interactions and offline use.
* AngularJS with binding of data between UI and code
* When SPAs are divided, a tighter integration is tougher - the browser needs some loading time when switching the SPAS
* An common asset server can serve static files
* A proxy-server can share requests to the asset server and the micro services - all micro services can be reached under one URL structure then
* JavaScript can only access code from the same domain (Same Origin Policy). The problem can be solved with CORS (Cross Origin Resource Sharing). 
* With only one SPA for several micro services, the SPA should be divided into modules - every module belongs to a different microservice and should be deployed with javascript files on its own. 
* AngularJS has a module concept. A module can integrate other modules. This leads to problems: The deployment needs to be coordinated, also the micro service modules could call each other.

#### HTML

* When changing pages, only a new html page is rendered
* ROCA (Resource Oriented Client Architecture) is a possiblity to design the work with JavaScript and dynamic elements. The application should be usable without JavaScript.
* It could be an idea to build up pages with Ajax, an example for this is BigPipe from Facebook
* ESI and SSI are technologies to build up a HTML page from fragments
 
### REST

* Resources with URIS
* Methods (GET, POST, etc. )
* Different Reprensentations (f.e. HTML, JSON or PDF)
* The server in a REST-system should be stateless - HTTP is stateless
* Could work with processes - first have a POST and then a GET for status updates

### SOAP / RPC

* Like REST but only POST, calling methods in another process (RPC)

### Messaging

* messages can be transfered when parts of the network are down
* the messaging system can garantie the correct transfer and processing
* answers will be transfered asynchronous 
* an call is not blocking the caller
* the sender does not know the receiver - it is just sending to the queue or topic. Se
* for transactions the sending and receiving are part of a transaction - database changes are rolled back then
* Two-Phase-commit (2PC) can be used to connect databases and messaging systems, alternatives are database/messaging products like ORacle AQ or ActiveMQ
* technologies:
  * AMQP (Advanced message queiung model) - an implementation of that protocol is RabbitMQ
  * Apache Kafka - focus on high performance, replication and outage safety
  * oMQ (ZeroMQ) - works without server and is pretty light weight therefore
  * Atom Feeds - usually used for Blog contents

### Data replication

* sharing a database has some problems - the representation can not be changed easily, fast changes are problematic, databases tend to get more complex with time
* It breaks the main law that components should have their own database representation
* It would be possible to have different Schemata, but there should not be any relationships between those
* Replication would be a solution - an example are data warehouses, they save replicate but save data different
* With replication it takes some time until the data is consistent. In a data warehouse or reporting tool this will not be a problem
* Replication should only be in one direction

### Interfaces

* Own Calls should be tested, callers should treated nicely. They should accept formal errors and only read parameters which are really needed. 

#### Versioning

* Versions should have the format MAJOR.MINOR.PATCH
* A MAJOR is not backwards compatible, a MINOR is for new features, PATCH is for bug fixes.
* A REST interface shoudl not have a version in its URL. Accept-headers should be used for versions.

## Architecture of a microservice

### CQRS (Command query responsibility segregation)

* changing operations with side effects (commands)
* reading operations, it returns something (queries), those can be cached
* An operation can not do both at the same time - this sperateion is called Command Query Seperation (CQS)
* CQRS seperates the processing of the queries and commands completely - Commands are stored in a Command Store, there can be a command handler as well. A query handler uses the database.
* A messaging solution in a microservice can offer the command queue. When using REST, then a microservice needs to implement the command queue and give commands to interested command handlers
* Every command handler can react on a command with own logic. 
* CRQS has some advantages:
  * the reading and writing can be seperated in microservices
  * There can be different models for reading and writing
  * reading and writing can be scaled differently
  * the command queue can be buffered at load peaks
  * there can be different versions of command handlers parallel - this helps with deployment
* Problems with CRQS:
  * Transactions with read- and write-operations are more problematic to huild
  * consistency will be a problem, because things work asynchronously
  * there are more system parts and it the environment gets more compley
  
### Event sourcing

* Event sourcing invokes on the domain-driven-design
* As commands are specific (they tell, what needs to be changed in an object), events only contain informations, WHAT happened
* Both can be combined. An command can change something, this can result in events, other components react on
* Event sourcing saves not the state but the events, leading to the actually state. The state needs to be reconstructed from the events
* The Event queue sends events to different receivers, the event store saves all events, the event handler reacts on events with business logic. 
* Events can not be corrected - wrong events need to be fixed with new events
* Events are good for auditing or accounting entries, because it is more reproducible, who changed which booking. Its also easier to reconstruct a historical data set
* Event sourcing is really meaningful if the system works with event-driven-architecture. 

### Resilience and stability

* Resilience: The ability to survive the outage of an other microservice
* Long timeouts can lead to a domino effect, because all threads are blocked
* A microservice can implement circuit breaker, timeouts when working wiht other micro services, when called, it can use fail fast
* Circuit breaker ("Sicherung")
  * if one microservice has failures or timeouts, then the breaker prevents calling it
  * The circuit breaker gives errors right away to unload the concerned microservice
  * After some time, the circuit breaker is closing again and reqquests are going through again 
  * A circuit breaker can be combined with a timeout
  * If the circuit breaker is open, there is no need for errors. There can be limited functionality.
  * An open circuit breaker should be shown in monitoring
* Bulkhead ("Schott") - microservices are working with own resources to avoid problems at other microservices, when one is overloaded - this is a normal pattern for microservices
* Steady State: Systems should be concepted to run forever, f.e. logs should be deleted after some time, cache values need to be removed at some points
* Fail Fast: A system should fail fast to use less ressources
 * Handshakes: Some protocols (not HTTP) allow handshaking - to ask for overloaded situations. This can be rebuild with Health checks. 
 * Test Harness: there should be tests, how apps react on TCP/IP level errors
 * Decoupling with middleware. An system should not wait on answers and go ahead with work. 
 * Reactive manifesto: calls will be processed aynchronousl. Hystrix: A Java solution to implement timeout and circuit breakers - it works with teread pools
 
### Reactive Manifesto: 

* Apps have actors. Working in an actor is sequentiell, different actos can work on different tasks parallel. 
* Attributes
  *  Responsive: it should responde fast, this is good for fail fast and stability. Reactive cann block new messages when it is full. Also I/O is not blocking
  * Resilience
  * Elastic: new deployments can be started in runtime to share the load
  * Message driven: Commmuniation over messages
* Technologies
  * Akka for Scala, Framework Play
  * RxJS for Javascript
  * Vert.x on JVM, working on Python as well
  
### Testing

* Mocks are simulating a call with a given result
* Stubs are simulating the microservice as a whole
* Tests on micro services level:
  * Unit tests
  * Integration tests are testing everything together
  * Ui tests
* The system at all
  * Integration tess
  * UI-Tests
* Manual tests (should be on explorative errors only)
* Other tests
  * Performance tests are testing the speed and how many customers a system can handle, load tests the behavior with load
  * Acceptance tests are testing the requirements of the customer, Tools are BDD (Behavior driven design) and ATDD (Acceptance Test Driven design)
* Tools like Serverspec could be helpful to test the servers and check, which ports are open etc

#### Stubs

* When stubs are used, the microservice team needs to provide this as well
* The stubs can run on a specified port
* Tools for definitions and stubs
  * mountebank (Javascript / node.js)
  * WireMock
  * Moco
  * stubby4j
  
#### Consumer-driven contract tests

* Most tests should be in the microservice to avoid longer integration test times
* They test the excpectations on the interface of an microsercie
* the using microservice deposits his requirements on a interface in a test, the microservice can run
* The consumer driven contract test tests the stubs as well to test the correct simulation of the the microservice
* Part of the contract
  * the data format
  * the interface
  * the protocols 
  * the meta informations
  * non functional aspects like latency
* Different types:
  * Provider contract (the service) - only 1 for every service
  * Consumer contract (the using service) - for every consumer of a service, only the parts which are used
* This helps to get an overview what is used
* Service users must be able to change the tests - if they are in the service Git, the users need to have access. If they are in the Git of the users, then the service needs to get the parts form Git everytime and test those
* Tools for CDC tests
  * Pacto
  * Pact
 
## Operations of an microservice
 
### Basics

* Micro/Macroarchitectures - what is decidec by every team and what by all teams?
* Templates are good to get started, but its okay if services code / templates become redundant.
 
### Logging

* Tools (ELK-stack)
  * Logstash (collecting & Parsing)
  * elasticsearch (Saving)
  * Kibana (Reporting)
* Testing of the performance and request times: Zipkin is a g ood tool
  
### Monitoring

* it should show I/O and network metrics as well as business metrics
* can take of automatic scaling, depending on extraordinary values
* Values:
  * I am alive
  * Health metrics - which parts are not working? 
  * Meta informations
  * latency, error counts
* Stake holders
  * Operation - what are the network / os problems?
  * Developer - what requests are having problems and why?
  * Business - how are the numbers?
* Data can be correlated with releases to check, if a new version is better for some things
* Tools
  * Graphite
  * Grafana
  * Seyren
  * Nagios
  * Icinga
  * Riemann
  * Packetbeat
  * New Relic (in the cloud)
  * Metrics framework (StatsD, Collectd)
* Stack:
  * the microservices themself (with a library or an api)
  * maybe an agent, collecting data from the system
  * the monitoring tool to save and visualize data and give alarms
  * for analytics with complex algorithm there can be a solution parallel with big data tools
  
### Deployment


  
