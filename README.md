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
