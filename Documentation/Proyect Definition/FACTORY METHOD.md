# APPLICATION OF FACTORY METHOD
The Factory Method pattern, or Factory Method Pattern, is a creational design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. In other words, this pattern defines an abstract 'factory' that declares a method for creating objects, but the concrete subclasses of the factory are responsible for implementing that method and deciding which specific class of object to create.

-	Common Interface: The superclass provides a common interface that declares the abstract factory method. This method is used to create objects but does not specify the concrete class of object to be created.

-	Concrete Subclasses: Concrete subclasses of the superclass implement the factory method and determine which specific class of object will be created. Each subclass can have its own implementation of the factory method.

-	Decoupling: The Factory Method pattern promotes decoupling between the code that uses the class and the concrete classes that are created. This makes the code more flexible and easier to maintain since new subclasses can be added without modifying the superclass.
  
The Factory Method pattern is useful when you have a base class that needs to delegate the creation of objects to its subclasses, allowing these subclasses to choose what type of object to create based on specific application needs. This is especially helpful in situations where the exact type of object to be created is not known in advance.

<div id="imgFM" align="center">
  <img src="https://www.oscarblancarteblog.com/wp-content/uploads/2018/12/factory-method-diagram1.png" width="500px"/> 
</div>
<br>
 
An example of a framework that uses this pattern is the Spring Framework. Spring employs the Factory Method pattern in various parts of its code to create and manage application beans (beans are application objects managed by the Spring container. These beans represent application components such as services, controllers, repositories, etc.). 

The Spring container uses Factory Methods to efficiently create and configure objects.The use of the Factory Method pattern in Spring provides advantages such as centralized bean configuration, dependency management, and flexibility to change bean implementations without modifying the application's source code.

<div id="imgSF" align="center">
  <img src="https://programaenlinea.net/wp-content/uploads/2019/11/spring-framework.png" width="500px"/> 
</div>


BeansFactory interface is an example the simplest container providing an advanced configuration mechanism to instantiate, configure and manage the life cycle of beans. Beans are java objects that are configured at run-time by Spring IoC Container. BeanFactory represents a basic IoC container which is a parent interface of ApplicationContext.

Example Map BeanFactoty: 


<div id="imgBFM" align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/uploads/20210702121645/m3.png" width="700px"/> 
</div>



Upload by: [Carlos Ruz](https://github.com/XxCharlyRuzxX)




