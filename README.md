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
				
