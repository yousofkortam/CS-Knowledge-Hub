# EntityManager

JPA has as its core interface the EntityManager, which is the component that is in charge of controlling the life cycle of all the entities defined in the persistence unit, and it is through this interface that the basic operations of a database can be performed , such as query, update, delete, create (CRUD). It is also the class by which transactions are controlled.

EntityManagers are always configured from the persistence units defined in the persistence.xml file and it is possible to create them in two ways, which will depend on the type of application:

Desktop application: In the case of running our program as a desktop or stanalone application, it will be necessary to instantiate the EntityManager through the EntityManagerFactory class. In the first part of this tutorial we will be using this method. An important detail of this method is that the programmer is responsible for opening and closing transactions.
Business Applications: When our application lives within an Application Server or precisely in an EJB Container, the EntityManager is referenced by means of Dependency Injection (CDI), using the @PersistenceContext annotation, in this way, the Application Server is in charge of creating the EntityManager for injection. An interesting detail of this method is that the Application Server can automatically take care of transactions, opening a transaction at the beginning of the execution of a business method and closing the transaction automatically at the end of the method invocation. We will discuss this method later.

