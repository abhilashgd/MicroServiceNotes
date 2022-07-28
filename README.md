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
