# MicroServiceNotes

#1 what are monolith application?

		- In monolith architecture the application is built as a single unit
		- such applications comprises of client side interface, server-side application and a database
		- normally a monolith application have one single code base and it lack modularity

#2 Monolithic application architecture

		- Clients -> presentation layer -> controller layer -> business layer ->repository layer ->DB

#3 Disadvantages of monolithic architecture

		- the code base get larger in size with time and hence its very difficult to manage
		- it is very difficult to introduce new technology as it affects the whole application
		- a single bug in any module can bring down the whole application
		- it is very difficult to scale a single module. One has to scale the whole application
		- continuous deployment is extremely difficult. Large monolithic applications are actually an obstacle to frequent deployments. In order to update one component, we have to redeploy the entire application.

#4 what are micro services ?

		- while monolith application works as a single component, a micro service architecture breaks it down to independent standalone small applications, each serving one particular requirement. Ex: 1 micro service for handling all vaccination centre operations, 1 for handling all the user base

		- within this micro service architecture, the entire functionality is split in independent deployable module which communicate with each other through APIs ( RestFul web services)
		
		Microservice application architecture
		UI -> service1 ->DB
		UI ->service 2 ->DB 
		Etc etc


#5 advantages of micro services

		- All the services are independent of each other. Therefore testing and deployment is easy as compare to monolith application
		- If there is a bug in one micro service it has an impact only on a particular service and does not affect the entire application. Even if ur vaccination centre service is down, u still have ur users register to your application
		- with micro service architecture, its easy to build complex applications
		it will give flexibility to choose technologies and framework for each micro services independently


#6 How to start with micro services?

		- If you have a monolith application, identify all possible standalone functionalities
		- once you have identified them, you need to create standalone projects, we are taking spring boot to create these micro services
		- you need them to interact with each other through some ways, it can be rest API or messaging. We are going to use restful architecture for the same
		- but just doing this does not make sure that you have implemented micro service architecture. These are till now just 2 Restful web services. You need load balancer, eureka service discovery (useful during load balancing and deployments) API gateways and many more
		
		Microservice1 -> Register -> Eureka
		Microservice2 -> register -> eureka

		Client/Browser -> eureka

#7 Microservice aarchitecture ?

	FE/Client -> Security and identity management -> API gateway -> microService1, Microservice2, etc
	API gateway ->Service Discovery

      	Security and identity management like OKTA - Spring Security
	Service Discovery - server like eureka or Netflix
$8 what are the main features of micro services?

		- micro services architecture breaks an application into smaller services, and it is possible to develop, deploy each micro service independently. This makes the introduction of new features in an application very easier
		- Decentralisation: Microservices architecture leads to distributed systems. The data management is decentralised. There will be a monolithic database containing all data belonging to the application. Each service has the ownership of the data related to the business functionality of that service
		- Black Box: Every microservice is defined as black box. The details of the complexity are hidden from other services/components
		- Security: The micro service platform itself should provide capabilities for certificate management. Different types of credentials, authentication and authorisation based on RBAC ( Role based access model). Security is decoupled from the micro service development team as platform standardisation help with it.
		
$9 how does micro service communicate with each other ?

		- In the case of micro service architecture, there are 2 different types of inter-service communication. Between micro services

		- synchronous communication
		- asynchronous communication

		Synchronous communication: 
	
		in the case of Synchronous communication between micro services, the client service waits for the response within a time limit. The possible solution is using http protocol using via REST API for inter service communication.

		Asynchronous Communication:
		
		In case of Asynchronous communication, the client service doesnt wait for the response from another service. When the client micro service calls another micro service, the thread is not blocked until a response comes from the server.
		The message producer service generates a message and sends the message to a message broker on a defined topic. The message producer waits for only the acknowledgement from the message broker to know that message is received by the broker.

		The consuming service subscribes to a topic in the message queue. All the messages belonging to that topic will be fed to the consuming systems. The message producer service and consuming services don't even know each other. The response is received in the same methodology through a message broker via defined message topics.

		Different messaging tools are based on the AMQP ( Advanced message Queuing Protocol). Some examples are given below

		a. Apache Kafka
		b. RabbitMQ
		c. Apache ActiveMQ


#10 what is the difference between monolithic, SOA and micro service architecture ?

		- Monolithic architecture is similar to a big container wherein all the software components of an application are assembled together and tightly packaged
		- A service-Oriented Architecture is a collection of services which communicate with each other. The communication can involve either simple data passing or it could involve two or more services co-ordinating some activity
		- micro service architecture is an architectural style that structures an application as a collection of small autonomous services, modemed around a business domain
		- main difference between SOA and MS architecture is based on sharing of data and info. SOA shares and reuses as much as possible while MS focuses on sharing as little as possible

#11 how to design micro services?
		
		- A separate data store for each of the microservice.
		- The application needs to be split into loosely coupled microservices based on business functionality.
		- Decentralized framework.
		- Polyglot architecture as per the business needs.
		- Services to be designed, developed, deployed, and managed separately.
		- Domain-driven design
		- Real-time monitoring of the application should be possible.
		- Deploy in containers
		
#12 Fault tolerence in Microservices Hystrix (Circuit Breaker)

			- Fault can arise when a particular service is down
			- Fault tolerance is the property that enables a system to continue operating properly in the event of failure of some its components

#13 how to configure circuit breaker design pattern ?
	
			- whenever a micro service is down, stop calling that service and call a fallback method configured.
			
			Step1: in pom.xml add dependency
			
			<dependency>‹groupId›org.springframework.cloud</groupId>
			<artifactId›spring-cloud-starter-netflix-hystrix</artifactId>
			<version>2.2.8.RELEASE</version>
			</dependency>
			
			Step2: in the spring boot application class add
				@EnableCircuitBreaker
				
			Step3: in controller class the method which can break add
			@HystrixCommand(fallbackMethod = "handleOtherMicroserviceDownTimme")
			
			Step4: create method handleOtherMicroserviceDownTimme()
			{
			handle the downtime scenario may by sending response of parent 
			}
			
				Idea behind circuit breaker is 
			- wrap your rest api call in circuit breaker object which monitors for failure
			- once the failures reach a certain threshold, the circuit breaker trips, and all further calls to the circuit breaker return with an error
			- hence comes our task: if it fails and circuit is open, configure a fallback method which will be executed as soon as circuit breaks or opens
			- in this way, your vaccination centre service rest controller is wrapped with proxy class and monitor its calls. Everything is done internally handles everything for you

			@HystrixCommand Elements
				1. fallbackMethod : Specifies a method to process fallback logic
				2. threadPoolKey : The thread-pool key is used to represent a Hystrix ThreadPool for monitoring,metrics publishing, caching and other such uses.
				3. threadPoolProperties: Specifies thread pool properties.
				4. groupKey: The command group key is used for grouping together commands such as for reporting, alerting, dashboards or team/library ownership
				5) commandProperties: Specifies command properties like
						a) circuitreaker.request Volume Threshold : number of requests reties before which circuit breaks
						b) circuitBreaker.error ThresholdPrecentage eg 60%: If 60 % request fails out of total requests
						made then open the circuit
						c) timeout s: Its like wait for milliseconds and if no response, break circuit
						
						
				A typical distributed system consists of many services collaborating together.

				These services are prone to failure or delayed responses. If a service fails it may impact on other services affecting performance and possibly making other parts of application inaccessible or in the worst case bring down the whole application.

				Of course, there are solutions available that help make applications resilient and fault tolerant – one such framework is Hystrix circuit breaker.

				The Hystrix circuit breaker framework library helps to control the interaction between services by providing fault tolerance and latency tolerance. It improves overall resilience of the system by isolating the failing services and stopping the cascading effect of failures.
				
#14 API Gateway - why do we need it ?

			We had eureka server, hystrix implemented in Previous videos, Is that not enough ? Why do we
			need API Gateway?
			The reasons are many :
			a) When we scale up our microservices, clients needs to know exactly which microservice they
			need. Client/ FE Dont need such information that backend developer is developing which
			microservice for which functionality. To over come this we have API Gateway to create an
			abstraction layer between FE and Backend developers . Thus FE developers need not
			depend on Backend people anymore. This is as good as implementing Abstraction
			principle in Java OOps.
			b) It becomes harder to change microservice without affecting the client. In reality, the client
			doesn't need to know microservice and its implementation behind.
			
			a) With API gateway we can centralise common functionalities like authentication, logging,
				and authorization. Now API will authenticate all requests hence each MS need not
				implement the same. Thus reduces code redundancy.
			b) Multiple Versions of Service : Our MS is updated with the new changes, but the updated
				service is not compatible with the mobile client. Our mobile client will continue using
				version-V1 while other clients will move to the version-V2. This can be done using API
				Gateway Routing Technique
				To address these issues, the architecture now contains another layer between the client and the
				microservices. This is API Gateway. API Gateway acts like a proxy that routes the request to the
				appropriate microservices and returns a response to the client. Microservices can also interact with each other through this Gateway.

#15 What is API gateway in microservices?

			API Gateway in Microservices is a Microservices Architecture pattern.

			API Gateway is a server and is a single-entry point into the system. API Gateway is responsible for routing the request, composition, and translation of the protocol. All the requests from the clients first come to the API Gateway and the API Gateway routes the request to the correct microservice.

			API Gateway can also aggregate the results from the microservices back to the client. API Gateway can also translate between web protocols like HTTP, web socket, etc.

			API Gateway can provide every client with a custom API as well. 

#16 What is the CQRS pattern in microservice?

			CQRS stands for Command Query Responsibility Segregator. Every microservice as per design will have a database per service model or shared database per service. As applications become more complex, the handling of detailed queries and validations also more complex. Traditional CRUD (Create, Read, Update and Delete) data model will become cumbersome to implement and maintain.

			CQRS pattern proposes the separation of the read data model (Read) from the writing data model (Create, Update and Delete). The application will be segregated into Command and Query parts. The command part will be responsible for the Create, update, and delete operations. The query part will be responsible for the read operation through materialized views. This segregation provides scalability, ease of maintenance, and optimization of the database.

#17 What is a circuit breaker pattern in Microservices?

			Circuit Breaker is a microservice design pattern. In a microservices architecture, it is typical that a request could span multiple services. For any request, if one of the services involved in the response is not working, a circuit Breaker is used to stop the process of request and response.

			Without a circuit breaker in place, the client would have continuously sent requests to the service which is down. Resources will get exhausted with low performance and a bad user experience due to this. To avoid this kind of problem, a circuit breaker pattern can be used. 

			In the case of a circuit breaker pattern, the client will invoke a remote service via a proxy. This proxy will behave as a circuit barrier. In case of failure, when the number of failures crosses a defined threshold number, the circuit breaker will trip for a defined period. During this time, requests to invoke remote service will fail. Once the time-out period for the breaker is complete, a circuit breaker will allow a defined number of tests to pass through, and only when they succeed, a circuit breaker will resume back to normal operation and will start fulfilling the requests.


#18 What is the Service Discovery pattern in microservices?

			In the case of Microservices, issues for calling services need to be addressed.

			With container technology, serverless architecture, IP addresses can be dynamically allocated to service instances. When address changes, consumer service can break and will need changes.

			This issue can be solved by a service registry. Service registry will keep metadata of every producer service in the ecosystem. A service instance needs to register to the registry whenever it starts and should de-register when shutting down. Consumer service can query the Service registry every time it needs to find out the location of the service. This is called the Service Discovery pattern.						

						
						
