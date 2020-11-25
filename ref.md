# Cracking the Coding Interview.
## OOP
* Class - is a blueprint which you use to create objects.
* Object - is an instance of a class - it's a concrete 'thing' that you made
using a specific class
### Encapsulation
Information hiding -> Private properties and public methods to access.
Hiding the details of the object and providing a decent interface for the
entities in outer world to interact with that object or entity.
Can validate in these access methods. (Control of properties without outside
intervention)
### Abstraction
Expose only what’s required rather than modeling unnecessary unused details.
Outsider can only see restricted content. Enforce whats required.
Abstraction means to show only the necessary details to the client of the
object… expose only the details which are concern with the user (client) of
your object…
Can be used to hide the implementation. (Focuses on functionality rather
than implementation -> What it should do than how to do that)
### Inheritance
Inherit common implementations from parent classes -> avoid redundancy.
<br>IS-A relationship, also known as parent-child relationship [implements/extends]
<br> Has-A relationship, instantiate object [new CLASS();]
### Polymorphism
Take many forms according to context.-> to create generic functionality
Used implementation can be decided on runtime depending on data type.
<br>Method overloading -> static polymorphism
<br>Method overriding -> dynamic polymorphism (Runtime polymorphism)
![image](https://user-images.githubusercontent.com/20130001/99996566-eeaae200-2de1-11eb-854a-0e591b9dea89.png)
#### Function overloading vs Operator overloading
Java has no Operator overloading , It has just method overloading
####  Encaplustaion vs Abstraction
![image](https://user-images.githubusercontent.com/20130001/100000578-c7efaa00-2de7-11eb-8424-e76cb08c9c21.png)
#### Aggregation And Composition
![image](https://user-images.githubusercontent.com/20130001/100006958-50267d00-2df1-11eb-9fdc-f60476699479.png)
![image](https://user-images.githubusercontent.com/20130001/100006989-5b79a880-2df1-11eb-9cf5-2e37b7d3a6c2.png)
![image](https://user-images.githubusercontent.com/20130001/100007086-8106b200-2df1-11eb-8e9a-f7e8121eefd5.png)
![image](https://user-images.githubusercontent.com/20130001/100007155-a3003480-2df1-11eb-8115-e014b202d4c0.png)

## algo, data structures

### Linear data structure

### Non Linear data structure
#### Trees
![image](https://user-images.githubusercontent.com/20130001/100103136-8106c080-2e8a-11eb-80de-63a43f06c973.png)
![image](https://user-images.githubusercontent.com/20130001/100103413-e0fd6700-2e8a-11eb-8c46-afb3ad77fb28.png)
![image](https://user-images.githubusercontent.com/20130001/100103152-86640b00-2e8a-11eb-8f80-fe65219eac99.png)
#### Graphs

## Database 
### ACID,
![image](https://user-images.githubusercontent.com/20130001/99980073-d630cc80-2dcd-11eb-998d-5a5acf6fb295.png)
### Keys
![image](https://user-images.githubusercontent.com/20130001/100002555-c07dd000-2dea-11eb-816c-18ba9b81ce7c.png)
![image](https://user-images.githubusercontent.com/20130001/100002729-f9b64000-2dea-11eb-8e70-77352356e0b9.png)
* Super Key – The set of attributes which can uniquely identify a tuple is known as Super Key. So, a candidate key, primary key, and a unique key is a superkey, but vice-versa isn’t true.
* Primary Key – A set of attributes which are used to uniquely identify every tuple is also a primary key. In the above example, since EmployeeID, InsuranceNumber and PanNumber are candidate keys, any one of them can be chosen as a Primary Key. Here EmployeeID is chosen as the primary key.
* Candidate Key – A set of attributes which can uniquely identify a table can be termed as a Candidate Key. A table can have more than one candidate key, and out of the chosen candidate keys, one key can be chosen as a Primary Key. In the above example, since EmployeeID, InsuranceNumber and PanNumber can uniquely identify every tuple, they would be considered as a Candidate Key. 
* Alternate Key – Alternate Keys are the candidate keys, which are not chosen as a Primary key. From the above example, the alternate keys are PanNumber and Insurance Number.
* Unique Key –  The unique key is similar to the primary key, but allows one NULL value in the column. Here the Insurance Number and the Pan Number can be considered as unique keys.
* Foreign Key – An attribute that can only take the values present as the values of some other attribute, is the foreign key to the attribute to which it refers. in the above example, the Employee_ID from the Employee_Information Table is referred to the Employee_ID from the Employee_Salary Table.
* Composite Key – A composite key is a combination of two or more columns that identify each tuple uniquely. Here, the Employee_ID and Month-Year_Of_Salary can be grouped together to uniquely identify every tuple in the table.
### normalization
* Insertion anomaly - It occurs when we cannot insert data to the table without the presence of another attribute
* Update anomaly -  It is a data inconsistency that results from data redundancy and a partial update of data.
* Deletion Anomaly - It occurs when certain attributes are lost because of the deletion of other attributes.
### SQL
![image](https://user-images.githubusercontent.com/20130001/100004145-194e6800-2ded-11eb-8f17-bbde154c6ab8.png)
![image](https://user-images.githubusercontent.com/20130001/100004153-1bb0c200-2ded-11eb-8062-00a30a7fd664.png)
![image](https://user-images.githubusercontent.com/20130001/100004162-1f444900-2ded-11eb-9620-097276d7a2a3.png)
### Indexs
![image](https://user-images.githubusercontent.com/20130001/100004849-1dc75080-2dee-11eb-9dbe-a9060ac79218.png)
### SQL Joins
![image](https://user-images.githubusercontent.com/20130001/100003389-04bda000-2dec-11eb-9b22-aa0ce7ed4b5c.png)
### Transactions
### ER
![image](https://user-images.githubusercontent.com/20130001/100002254-5402d100-2dea-11eb-9a76-9148bac6f8b5.png)
### NoSQL
not only SQL
![image](https://user-images.githubusercontent.com/20130001/100001879-c7581300-2de9-11eb-8519-6e8a5f8370e7.png)
### CAP
![image](https://user-images.githubusercontent.com/20130001/100002033-06866400-2dea-11eb-8928-9c8782311016.png)



## language fundementals (relating to core computer science principles)
## Algo (queue, stack, linked list)

## OS (linux, kernel, thread vs processes)
### linux kernel
![image](https://user-images.githubusercontent.com/20130001/99972508-9b766680-2dc4-11eb-9b2b-7c4e130d5372.png)
### processes
![image](https://user-images.githubusercontent.com/20130001/99972909-23f50700-2dc5-11eb-9b05-5507dd217c8e.png)
#### Fork()
![image](https://user-images.githubusercontent.com/20130001/99972993-47b84d00-2dc5-11eb-83f9-748e3f8553e2.png)
```
#include <stdio.h> 
#include <sys/types.h> 
#include <unistd.h> 
void forkexample() 
{ 
    // child process because return value zero 
    if (fork() == 0) 
        printf("Hello from Child!\n"); 
  
    // parent process because return value non-zero. 
    else
        printf("Hello from Parent!\n"); 
} 
int main() 
{ 
    forkexample(); 
    return 0; 
} 
```
### Thread
![image](https://user-images.githubusercontent.com/20130001/99973211-8817cb00-2dc5-11eb-86ba-ec987bd8d56c.png)
### Thread vs Processes
![image](https://user-images.githubusercontent.com/20130001/99973443-c8774900-2dc5-11eb-83da-0eadbd185063.png)
![image](https://user-images.githubusercontent.com/20130001/99973334-a8e02080-2dc5-11eb-8936-81bdd05bf15e.png)
![image](https://user-images.githubusercontent.com/20130001/99973591-ef357f80-2dc5-11eb-8f43-6739380524b3.png)
### Contextswitch
In computing, a context switch is the process of storing the state of a process or thread, so that it can be restored and resume execution at a later point. This allows multiple processes to share a single central processing unit, and is an essential feature of a multitasking operating system.
![image](https://user-images.githubusercontent.com/20130001/99973861-3c195600-2dc6-11eb-868e-04edd683f404.png)
### Deadlock
In concurrent computing, a deadlock is a state in which each member of a group is waiting for another member, including itself, to take action, such as sending a message or more commonly releasing a lock
![image](https://user-images.githubusercontent.com/20130001/99977305-784eb580-2dca-11eb-81a7-587fa570d2c3.png)
### LiveLock
![image](https://user-images.githubusercontent.com/20130001/99977498-bb108d80-2dca-11eb-8493-7cbe00bd88a5.png)
### Mutual exclusion
![image](https://user-images.githubusercontent.com/20130001/99977710-ff039280-2dca-11eb-8fd1-d2566dd2c6c2.png)
### Mutex
In computer science, a lock or mutex is a synchronization mechanism for enforcing limits on access to a resource in an environment where there are many threads of execution. A lock is designed to enforce a mutual exclusion concurrency control policy.
![image](https://user-images.githubusercontent.com/20130001/99978629-3161bf80-2dcc-11eb-8a08-dde091cdcb41.png)
### Semaphore
In computer science, a semaphore is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multitasking operating system. A semaphore is simply a variable.
![image](https://user-images.githubusercontent.com/20130001/99978261-bd271c00-2dcb-11eb-9a07-39b0a25047bb.png)
### MutexVs Semaphore
![image](https://user-images.githubusercontent.com/20130001/99978138-94068b80-2dcb-11eb-8fd8-0ec9891d11cb.png)
## Design patterns
Singleton, factory, observer wage mona hari poddak balaganin
![image](https://user-images.githubusercontent.com/20130001/99936633-9ba54080-2d89-11eb-8fd7-5b57b5777227.png)
### Creational
These design patterns provide a way to create objects while hiding the creation logic, rather than instantiating objects directly using new operator. This gives program more flexibility in deciding which objects need to be created for a given use case.
#### Singleton Pattern
This pattern involves a single class which is responsible to create an object while making sure that only single object gets created. This class provides a way to access its only object which can be accessed directly without need to instantiate the object of the class.
![image](https://user-images.githubusercontent.com/20130001/99936836-1a9a7900-2d8a-11eb-8971-d8e8676f45e4.png)
<br>
* Singleton pattern is used for logging, database drivers objects, cacheing and thread pool.
* Singleton design pattern is also used  in other design patterns like Abstract Factory,  Builder Prototype, Facade etc.
* Singletopn design pattern is used in core Java classes also, for example ```java.lang.Runtime``` ```java.awt.Desktop```

```
public class SingleObject {

   //create an object of SingleObject
   private static SingleObject instance = new SingleObject();

   //make the constructor private so that this class cannot be
   //instantiated
   private SingleObject(){}

   //Get the only object available
   public static SingleObject getInstance(){
      return instance;
   }

   public void showMessage(){
      System.out.println("Hello World!");
   }
}
```
#### Factory Pattern
In Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.
![image](https://user-images.githubusercontent.com/20130001/99936918-533a5280-2d8a-11eb-8fa1-a3b15a4152b9.png)
```
public interface Shape {
   void draw();
}
```
```
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
```
```
public class Square implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}
```
```
public class Circle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}
```
```
public class ShapeFactory {
	
   //use getShape method to get object of type shape 
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }		
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
         
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
         
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      
      return null;
   }
}
```
```
public class FactoryPatternDemo {

   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();

      //get an object of Circle and call its draw method.
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //call draw method of Circle
      shape1.draw();

      //get an object of Rectangle and call its draw method.
      Shape shape2 = shapeFactory.getShape("RECTANGLE");

      //call draw method of Rectangle
      shape2.draw();

      //get an object of Square and call its draw method.
      Shape shape3 = shapeFactory.getShape("SQUARE");

      //call draw method of square
      shape3.draw();
   }
}
```
### Structural Patterns
These design patterns concern class and object composition. Concept of inheritance is used to compose interfaces and define ways to compose objects to obtain new functionalities.
#### Proxy Pattern
In proxy pattern, we create object having original object to interface its functionality to outer world.
![image](https://user-images.githubusercontent.com/20130001/99937148-f8edc180-2d8a-11eb-9be3-de27f6e50782.png)
```
public interface Image {
   void display();
}
```
```
public class RealImage implements Image {

   private String fileName;

   public RealImage(String fileName){
      this.fileName = fileName;
      loadFromDisk(fileName);
   }

   @Override
   public void display() {
      System.out.println("Displaying " + fileName);
   }

   private void loadFromDisk(String fileName){
      System.out.println("Loading " + fileName);
   }
}
```
```
public class ProxyImage implements Image{

   private RealImage realImage;
   private String fileName;

   public ProxyImage(String fileName){
      this.fileName = fileName;
   }

   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}
```
```
public class ProxyPatternDemo {
	
   public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");

      //image will be loaded from disk
      image.display(); 
      System.out.println("");
      
      //image will not be loaded from disk
      image.display(); 	
   }
}
```
### Behavioral Patterns
These design patterns are specifically concerned with communication between objects.
#### Observer Pattern
Observer pattern is used when there is one-to-many relationship between objects such as if one object is modified, its depenedent objects are to be notified automatically. Observer pattern falls under behavioral pattern category.<br>
Observer pattern uses three actor classes. Subject, Observer and Client. Subject is an object having methods to attach and detach observers to a client object. We have created an abstract class Observer and a concrete class Subject that is extending class Observer.

ObserverPatternDemo, our demo class, will use Subject and concrete class object to show observer pattern in action.
![image](https://user-images.githubusercontent.com/20130001/99937374-95b05f00-2d8b-11eb-86bd-cd5190dceaf5.png)
```
import java.util.ArrayList;
import java.util.List;

public class Subject {
	
   private List<Observer> observers = new ArrayList<Observer>();
   private int state;

   public int getState() {
      return state;
   }

   public void setState(int state) {
      this.state = state;
      notifyAllObservers();
   }

   public void attach(Observer observer){
      observers.add(observer);		
   }

   public void notifyAllObservers(){
      for (Observer observer : observers) {
         observer.update();
      }
   } 	
}
```
```
public abstract class Observer {
   protected Subject subject;
   public abstract void update();
}
```
```
public class BinaryObserver extends Observer{

   public BinaryObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Binary String: " + Integer.toBinaryString( subject.getState() ) ); 
   }
}
```
```
public class HexaObserver extends Observer{

   public HexaObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Hex String: " + Integer.toHexString( subject.getState() ).toUpperCase() ); 
   }
}
```

```
public class ObserverPatternDemo {
   public static void main(String[] args) {
      Subject subject = new Subject();

      new HexaObserver(subject);
      new OctalObserver(subject);
      new BinaryObserver(subject);

      System.out.println("First state change: 15");	
      subject.setState(15);
      System.out.println("Second state change: 10");	
      subject.setState(10);
   }
}
```
## server side frameworks
* Spring - The Spring Framework is an application framework and inversion of control container for the Java platform. The framework's core features can be used by any Java application, but there are extensions for building web applications on top of the Java EE platform.
* servlets - A Jakarta Servlet is a Java software component that extends the capabilities of a server. Although servlets can respond to many types of requests, they most commonly implement web containers for hosting web applications on web servers and thus qualify as a server-side servlet web API
* ejb - Jakarta Enterprise Beans is one of several Java APIs for modular construction of enterprise software.
* node.js - Node.js is an open-source, cross-platform, back-end, JavaScript runtime environment that executes JavaScript code outside a web browser.
* struts -Apache Struts 1 is an open-source web application framework for developing Java EE web applications
### What is JavaBeans
In computing based on the Java Platform, JavaBeans are classes that encapsulate many objects into a single object. They are serializable, have a zero-argument constructor, and allow access to properties using getter and setter methods
```
//Employee.java  
  
package mypack;  
public class Employee implements java.io.Serializable{  
private int id;  
private String name;  
public Employee(){}  
public void setId(int id){this.id=id;}  
public int getId(){return id;}  
public void setName(String name){this.name=name;}  
public String getName(){return name;}  
}  
```
```
package mypack;  
public class Test{  
public static void main(String args[]){  
Employee e=new Employee();//object is created  
e.setName("Arjun");//setting value to the object  
System.out.println(e.getName());  
}}  
```
## SDLC
### Waterfall
![image](https://user-images.githubusercontent.com/20130001/100053449-16cc2c80-2e46-11eb-83fb-37eed4f66776.png)
### Agile
AGILE methodology is a practice that promotes continuous iteration of development and testing throughout the software development lifecycle of the project. In the Agile model, both development and testing activities are concurrent, unlike the Waterfall mode
![image](https://user-images.githubusercontent.com/20130001/100053245-a3c2b600-2e45-11eb-8416-bab3b584a97d.png)
### Scrum
![image](https://user-images.githubusercontent.com/20130001/100053486-28adcf80-2e46-11eb-98b7-12553a649488.png)
### Test driven development
![image](https://user-images.githubusercontent.com/20130001/100054356-df5e7f80-2e47-11eb-8487-08910d81ca62.png)

## Devops
DevOps is a set of practices that combines software development (Dev) and IT operations (Ops). It aims to shorten the systems development life cycle and provide continuous delivery with high software quality. DevOps is complementary with Agile software development; 
#### DevOps Cycle
![image](https://user-images.githubusercontent.com/20130001/99970231-8fd57080-2dc1-11eb-8ce3-11ec7db3a32f.png)
#### continuous integration vs continuous delivery
![image](https://user-images.githubusercontent.com/20130001/99970448-db881a00-2dc1-11eb-807a-d07038817811.png)
#### Containerization 
Containerization is defined as a form of operating system virtualization, through which applications are run in isolated user spaces called containers, all using the same shared operating system (OS).
![image](https://user-images.githubusercontent.com/20130001/99970734-42a5ce80-2dc2-11eb-8023-ec6fff5fb2a4.png)
#### Docker
![image](https://user-images.githubusercontent.com/20130001/99997361-1484b680-2de3-11eb-9f6a-875297b48030.png)
<br>
![image](https://user-images.githubusercontent.com/20130001/99997663-7e04c500-2de3-11eb-8920-2225b4818825.png)
![image](https://user-images.githubusercontent.com/20130001/99997680-85c46980-2de3-11eb-9f47-230e0ed82d06.png)
![image](https://user-images.githubusercontent.com/20130001/99997714-91b02b80-2de3-11eb-8352-0ebc6b54fd9c.png)
![image](https://user-images.githubusercontent.com/20130001/99997735-996fd000-2de3-11eb-8081-a42642770071.png)
![image](https://user-images.githubusercontent.com/20130001/99997762-a260a180-2de3-11eb-954c-34ae1f84cc40.png)
![image](https://user-images.githubusercontent.com/20130001/99997823-b73d3500-2de3-11eb-867e-bdbbd714dc96.png)
![image](https://user-images.githubusercontent.com/20130001/99997883-ccb25f00-2de3-11eb-93bd-d9059a7b6bb6.png)
![image](https://user-images.githubusercontent.com/20130001/99997904-d20fa980-2de3-11eb-9290-47af8830ed45.png)
![image](https://user-images.githubusercontent.com/20130001/99997949-e5227980-2de3-11eb-881d-f7e49b63ebe0.png)
#### kubernetes
![image](https://user-images.githubusercontent.com/20130001/99999736-99bd9a80-2de6-11eb-8214-faca2c857a64.png)
#### etcd
lightweight, distributed key-value store that can be
configured to span across multiple nodes.
<br>Kubernetes uses etcd to store configuration data that can be accessed
by each of the nodes in the cluster.
<br>This can be used for service discovery
and can help components configure or reconfigure themselves according
to up-to-date information. 
<br>It also helps maintain cluster state with features
like leader election and distributed locking. 
#### kube-apiserver
One of the most important master services is an API server. This is the
main management point of the entire cluster as it allows a user to
configure Kubernetes’ workloads and organizational units. It is also
responsible for making sure that the etcd store and the service details of
deployed containers are in agreement. It acts as the bridge between various
components to maintain cluster health and disseminate information and
commands.
#### kube-controller-manager
The controller manager is a general service that has many responsibilities.
Primarily, it manages different controllers that regulate the state of the
cluster, manage workload life cycles, and perform routine tasks. For
instance, a replication controller ensures that the number of replicas
(identical copies) defined for a pod matches the number currently
deployed on the cluster. The details of these operations are written to
etcd, where the controller manager watches for changes through the API
server.
#### kube-scheduler
The process that actually assigns workloads to specific nodes in the cluster
is the scheduler. This service reads in a workload’s operating
requirements, analyzes the current infrastructure environment, and places
the work on an acceptable node or nodes.
#### Cloud controller managers 
Cloud controller managers act as the glue that allows Kubernetes to
interact providers with different capabilities, features, and APIs while
maintaining relatively generic constructs internally. This allows
Kubernetes to update its state information according to information
gathered from the cloud provider, adjust cloud resources as changes are
needed in the system, and create and use additional cloud services to
satisfy the work requirements submitted to the cluster.
#### A Container Runtime
The first component that each node must have is a container runtime.
Typically, this requirement is satisfied by installing and running Docker,
but alternatives like rkt and runc are also available.
#### kubelet
The main contact point for each node with the cluster group is a small
service called kubelet.
#### kube-proxy
To manage individual host subnetting and make services available to other
components, a small proxy service called kube-proxy is run on each node
server. This process forwards requests to the correct containers, can do
primitive load balancing, and is generally responsible for making sure the
networking environment is predictable and accessible, but isolated where
appropriate.
#### Ansible
#### Jenkins
## Architectural question 
### SOA
![image](https://user-images.githubusercontent.com/20130001/99964756-d1fab400-2db9-11eb-9374-a8faeb803849.png)
![image](https://user-images.githubusercontent.com/20130001/99964845-f5bdfa00-2db9-11eb-9ebc-547340d746e0.png)
### Microservices
![image](https://user-images.githubusercontent.com/20130001/99964886-0cfce780-2dba-11eb-80ac-998df0f52eab.png)
### SOA vs  Microservices
![image](https://user-images.githubusercontent.com/20130001/99964998-36b60e80-2dba-11eb-8a94-243b1d3b1f32.png)
### SOAP (Simple object access protocol)
![image](https://user-images.githubusercontent.com/20130001/99965174-7f6dc780-2dba-11eb-8fee-2907067b80c4.png)
![image](https://user-images.githubusercontent.com/20130001/99965369-c78cea00-2dba-11eb-8c76-da90c4f9473b.png)
![image](https://user-images.githubusercontent.com/20130001/99965290-ab894880-2dba-11eb-9b28-7ab321753f4f.png)
![image](https://user-images.githubusercontent.com/20130001/99965321-b643dd80-2dba-11eb-9589-9b48899048fc.png)
### REST (Representational state transfer)
![image](https://user-images.githubusercontent.com/20130001/99965601-1470c080-2dbb-11eb-9832-333ba33dafde.png)
#### Why is it said that “HTTP is a stateless protocol”?
HTTP is called as a stateless protocol because each request is executed independently, without any knowledge of the requests that were executed before it, which means once the transaction ends the connection between the browser and the server is also lost.
![image](https://user-images.githubusercontent.com/20130001/99967147-716d7600-2dbd-11eb-9e86-f1168eba30e5.png)
![image](https://user-images.githubusercontent.com/20130001/99966627-98777800-2dbc-11eb-96d3-d512e6cbb08f.png)
* Safe - HTTP methods are considered safe if they do not alter the server state. So safe methods can only be used for read-only operations. The HTTP RFC defines the following methods to be safe: GET, HEAD, OPTIONS and TRACE
* Idempotency - Idempotency means that multiple identical requests will have the same outcome. So it does not matter if a request is sent once or multiple times, GET, HEAD, OPTIONS, TRACE, PUT and DELETE
#### POST vs GET
![image](https://user-images.githubusercontent.com/20130001/99968311-ff962c00-2dbe-11eb-8d7e-01022cd1e4a0.png)
#### POST vs PUT
![image](https://user-images.githubusercontent.com/20130001/99968153-c8277f80-2dbe-11eb-8c93-791de478f17b.png)
### SOAP vs REST
![image](https://user-images.githubusercontent.com/20130001/99967316-af6a9a00-2dbd-11eb-92ff-d6f75bf38174.png)
![image](https://user-images.githubusercontent.com/20130001/99967986-931b2d00-2dbe-11eb-873d-2611b188757b.png)
![image](https://user-images.githubusercontent.com/20130001/99967374-cc06d200-2dbd-11eb-9f84-450f0b85888d.png)
#### WSDL Web Services Description Language
```
<definitions>

<types>
  data type definitions........
</types>

<message>
  definition of the data being communicated....
</message>

<portType>
  set of operations......
</portType>

<binding>
  protocol and data format specification....
</binding>

</definitions>
```
### JWT
![image](https://user-images.githubusercontent.com/20130001/99967863-66ffac00-2dbe-11eb-84a3-cb7bda8761d0.png)
![image](https://user-images.githubusercontent.com/20130001/99967789-4f282800-2dbe-11eb-85cc-c16a52c44407.png)

### Networks
#### ISO 7 layer model
![image](https://user-images.githubusercontent.com/20130001/99968548-4421c780-2dbf-11eb-8e57-56b199c535c6.png)
#### tcp/ip model
![image](https://user-images.githubusercontent.com/20130001/99968867-b397b700-2dbf-11eb-83a5-1523392753b7.png)
#### ISO 7 layer model vs tcp/ip model
![image](https://user-images.githubusercontent.com/20130001/99968794-a11d7d80-2dbf-11eb-918e-0aedf628e45d.png)
## Java 
### Threads
#### By Thread Class
```
// Java code for thread creation by extending 
// the Thread class 
class MultithreadingDemo extends Thread 
{ 
	public void run() 
	{ 
		try
		{ 
			// Displaying the thread that is running 
			System.out.println ("Thread " + 
				Thread.currentThread().getId() + 
				" is running"); 

		} 
		catch (Exception e) 
		{ 
			// Throwing an exception 
			System.out.println ("Exception is caught"); 
		} 
	} 
} 

// Main Class 
public class Multithread 
{ 
	public static void main(String[] args) 
	{ 
		int n = 8; // Number of threads 
		for (int i=0; i<n; i++) 
		{ 
			MultithreadingDemo object = new MultithreadingDemo(); 
			object.start(); 
		} 
	} 
} 

```
#### By Runnable
```
// Java code for thread creation by implementing 
// the Runnable Interface 
class MultithreadingDemo implements Runnable 
{ 
	public void run() 
	{ 
		try
		{ 
			// Displaying the thread that is running 
			System.out.println ("Thread " + 
								Thread.currentThread().getId() + 
								" is running"); 

		} 
		catch (Exception e) 
		{ 
			// Throwing an exception 
			System.out.println ("Exception is caught"); 
		} 
	} 
} 

// Main Class 
class Multithread 
{ 
	public static void main(String[] args) 
	{ 
		int n = 8; // Number of threads 
		for (int i=0; i<n; i++) 
		{ 
			Thread object = new Thread(new MultithreadingDemo()); 
			object.start(); 
		} 
	} 
} 
```
#### synchronization in java
```
// A Java program to demonstrate working of 
// synchronized. 
import java.io.*; 
import java.util.*; 

// A Class used to send a message 
class Sender 
{ 
	public void send(String msg) 
	{ 
		System.out.println("Sending\t" + msg ); 
		try
		{ 
			Thread.sleep(1000); 
		} 
		catch (Exception e) 
		{ 
			System.out.println("Thread interrupted."); 
		} 
		System.out.println("\n" + msg + "Sent"); 
	} 
} 

// Class for send a message using Threads 
class ThreadedSend extends Thread 
{ 
	private String msg; 
	Sender sender; 

	// Recieves a message object and a string 
	// message to be sent 
	ThreadedSend(String m, Sender obj) 
	{ 
		msg = m; 
		sender = obj; 
	} 

	public void run() 
	{ 
		// Only one thread can send a message 
		// at a time. 
		synchronized(sender) 
		{ 
			// synchronizing the snd object 
			sender.send(msg); 
		} 
	} 
} 

// Driver class 
class SyncDemo 
{ 
	public static void main(String args[]) 
	{ 
		Sender snd = new Sender(); 
		ThreadedSend S1 = 
			new ThreadedSend( " Hi " , snd ); 
		ThreadedSend S2 = 
			new ThreadedSend( " Bye " , snd ); 

		// Start two threads of ThreadedSend type 
		S1.start(); 
		S2.start(); 

		// wait for threads to end 
		try
		{ 
			S1.join(); 
			S2.join(); 
		} 
		catch(Exception e) 
		{ 
			System.out.println("Interrupted"); 
		} 
	} 
} 

```
```
// An alternate implementation to demonstrate 
// that we can use synchronized with method also. 
class Sender 
{ 
	public synchronized void send(String msg) 
	{ 
		System.out.println("Sending\t" + msg ); 
		try
		{ 
			Thread.sleep(1000); 
		} 
		catch (Exception e) 
		{ 
			System.out.println("Thread interrupted."); 
		} 
		System.out.println("\n" + msg + "Sent"); 
	} 
} 

```
```
// One more alternate implementation to demonstrate 
// that synchronized can be used with only a part of 
// method 
class Sender 
{ 
	public void send(String msg) 
	{ 
		synchronized(this) 
		{ 
			System.out.println("Sending\t" + msg ); 
			try
			{ 
				Thread.sleep(1000); 
			} 
			catch (Exception e) 
			{ 
				System.out.println("Thread interrupted."); 
			} 
			System.out.println("\n" + msg + "Sent"); 
		} 
	} 
} 
```
### Exceptions
![image](https://user-images.githubusercontent.com/20130001/100096045-772c8f80-2e81-11eb-92f4-67dbf82fe051.png)
### Generics
#### Generic Class
```
// A Simple Java program to show working of user defined 
// Generic classes 

// We use < > to specify Parameter type 
class Test<T> 
{ 
	// An object of type T is declared 
	T obj; 
	Test(T obj) { this.obj = obj; } // constructor 
	public T getObject() { return this.obj; } 
} 

// Driver class to test above 
class Main 
{ 
	public static void main (String[] args) 
	{ 
		// instance of Integer type 
		Test <Integer> iObj = new Test<Integer>(15); 
		System.out.println(iObj.getObject()); 

		// instance of String type 
		Test <String> sObj = 
						new Test<String>("GeeksForGeeks"); 
		System.out.println(sObj.getObject()); 
	} 
}

```
#### Generic Functions
```
// A Simple Java program to show working of user defined 
// Generic functions 

class Test 
{ 
	// A Generic method example 
	static <T> void genericDisplay (T element) 
	{ 
		System.out.println(element.getClass().getName() + 
						" = " + element); 
	} 

	// Driver method 
	public static void main(String[] args) 
	{ 
		// Calling generic method with Integer argument 
		genericDisplay(11); 

		// Calling generic method with String argument 
		genericDisplay("GeeksForGeeks"); 

		// Calling generic method with double argument 
		genericDisplay(1.0); 
	} 
}
```
#### Bounded Type Parameters
```
public class MaximumTest {
   // determines the largest of three Comparable objects
   
   public static <T extends Comparable<T>> T maximum(T x, T y, T z) {
      T max = x;   // assume x is initially the largest
      
      if(y.compareTo(max) > 0) {
         max = y;   // y is the largest so far
      }
      
      if(z.compareTo(max) > 0) {
         max = z;   // z is the largest now                 
      }
      return max;   // returns the largest object   
   }
   
   public static void main(String args[]) {
      System.out.printf("Max of %d, %d and %d is %d\n\n", 
         3, 4, 5, maximum( 3, 4, 5 ));

      System.out.printf("Max of %.1f,%.1f and %.1f is %.1f\n\n",
         6.6, 8.8, 7.7, maximum( 6.6, 8.8, 7.7 ));

      System.out.printf("Max of %s, %s and %s is %s\n","pear",
         "apple", "orange", maximum("pear", "apple", "orange"));
   }
}
```
### Collection Module
![image](https://user-images.githubusercontent.com/20130001/100097539-a98abc80-2e82-11eb-935a-e504b0c21612.png)
### Comparators
Comparator interface is used to order the objects of user-defined classes. A comparator object is capable of comparing two objects of two different classes. Following function compare obj1 with obj2
<br>
Syntax: 
```
public int compare(Object obj1, Object obj2)
```
* Method 1: One obvious approach is to write our own sort() function using one of the standard algorithms. This solution requires rewriting the whole sorting code for different criterion like Roll No. and Name.

* Method 2: Using comparator interface- Comparator interface is used to order the objects of user-defined class. This interface is present in java.util package and contains 2 methods compare(Object obj1, Object obj2) and equals(Object element). Using comparator, we can sort the elements based on data members. For instance it may be on rollno, name, age or anything else.

```
// Java program to demonstrate working of Comparator 
// interface 
import java.util.*; 
import java.lang.*; 
import java.io.*; 

// A class to represent a student. 
class Student 
{ 
	int rollno; 
	String name, address; 

	// Constructor 
	public Student(int rollno, String name, String address) 
	{ 
		this.rollno = rollno; 
		this.name = name; 
		this.address = address; 
	} 

	// Used to print student details in main() 
	public String toString() 
	{ 
		return this.rollno + " " + this.name + 
						" " + this.address; 
	} 
} 

class Sortbyroll implements Comparator<Student> 
{ 
	// Used for sorting in ascending order of 
	// roll number 
	public int compare(Student a, Student b) 
	{ 
		return a.rollno - b.rollno; 
	} 
} 

class Sortbyname implements Comparator<Student> 
{ 
	// Used for sorting in ascending order of 
	// roll name 
	public int compare(Student a, Student b) 
	{ 
		return a.name.compareTo(b.name); 
	} 
} 

// Driver class 
class Main 
{ 
	public static void main (String[] args) 
	{ 
		ArrayList<Student> ar = new ArrayList<Student>(); 
		ar.add(new Student(111, "bbbb", "london")); 
		ar.add(new Student(131, "aaaa", "nyc")); 
		ar.add(new Student(121, "cccc", "jaipur")); 

		System.out.println("Unsorted"); 
		for (int i=0; i<ar.size(); i++) 
			System.out.println(ar.get(i)); 

		Collections.sort(ar, new Sortbyroll()); 

		System.out.println("\nSorted by rollno"); 
		for (int i=0; i<ar.size(); i++) 
			System.out.println(ar.get(i)); 

		Collections.sort(ar, new Sortbyname()); 

		System.out.println("\nSorted by name"); 
		for (int i=0; i<ar.size(); i++) 
			System.out.println(ar.get(i)); 
	} 
} 

```
### IO
![image](https://user-images.githubusercontent.com/20130001/100097670-e060d280-2e82-11eb-8813-e4e0519a2cb9.png)
![image](https://user-images.githubusercontent.com/20130001/100097675-e35bc300-2e82-11eb-90a4-2e9b954fda67.png)
### String Buffer Vs Sting Builder
![image](https://user-images.githubusercontent.com/20130001/100100437-cfb25b80-2e86-11eb-86fd-a710b7d57e2f.png)
### Streming
Introduced in Java 8, the Stream API is used to process collections of objects. A stream is a sequence of objects that supports various methods which can be pipelined to produce the desired result.
<br>The features of Java stream are 
* A stream is not a data structure instead it takes input from the Collections, Arrays or I/O channels.
* Streams don’t change the original data structure, they only provide the result as per the pipelined methods.
* Each intermediate operation is lazily executed and returns a stream as a result, hence various intermediate operations can be pipelined. Terminal operations mark the end of the stream and return the result.

##### Different Operations On Streams-
###### Intermediate Operations:

* map: The map method is used to returns a stream consisting of the results of applying the given function to the elements of this stream.
```
List number = Arrays.asList(2,3,4,5);
List square = number.stream().map(x->x*x).collect(Collectors.toList());
```
* filter: The filter method is used to select elements as per the Predicate passed as argument.
```
List names = Arrays.asList("Reflection","Collection","Stream");
List result = names.stream().filter(s->s.startsWith("S")).collect(Collectors.toList());
```
* sorted: The sorted method is used to sort the stream.
```
List names = Arrays.asList("Reflection","Collection","Stream");
List result = names.stream().sorted().collect(Collectors.toList());
```
###### Terminal Operations:

* collect: The collect method is used to return the result of the intermediate operations performed on the stream.
```
List number = Arrays.asList(2,3,4,5,3);
Set square = number.stream().map(x->x*x).collect(Collectors.toSet());
```
* forEach: The forEach method is used to iterate through every element of the stream.
```
List number = Arrays.asList(2,3,4,5);
number.stream().map(x->x*x).forEach(y->System.out.println(y));
```
* reduce: The reduce method is used to reduce the elements of a stream to a single value. The reduce method takes a BinaryOperator as a parameter.
```
List number = Arrays.asList(2,3,4,5);
int even = number.stream().filter(x->x%2==0).reduce(0,(ans,i)-> ans+i);
```
### Lambda
###### Without Lambda Expression
```
interface Drawable{  
    public void draw();  
}  
public class LambdaExpressionExample {  
    public static void main(String[] args) {  
        int width=10;  
  
        //without lambda, Drawable implementation using anonymous class  
        Drawable d=new Drawable(){  
            public void draw(){System.out.println("Drawing "+width);}  
        };  
        d.draw();  
    }  
}  
```
##### Java Lambda Expression Example
```
interface Drawable{  
    public void draw();  
}  
  
public class LambdaExpressionExample2 {  
    public static void main(String[] args) {  
        int width=10;  
          
        //with lambda  
        Drawable d2=()->{  
            System.out.println("Drawing "+width);  
        };  
        d2.draw();  
    }  
}  
```
### JVM, JRE, JDK
![image](https://user-images.githubusercontent.com/20130001/100056540-d7084380-2e4b-11eb-8c6c-578e3b359e09.png)
### Garbage Collection
The garbage collector is a program which runs on the Java Virtual Machine which gets rid of objects which are not being used by a Java application anymore. It is a form of automatic memory management.
<br>An object becomes eligible for Garbage collection or GC if it's not reachable from any live threads or by any static references.
<br>In other words, you can say that an object becomes eligible for garbage collection if its all references are null. Cyclic dependencies are not counted as the reference so if object A has a reference to object B and object B has a reference to Object A and they don't have any other live reference then both Objects A and B will be eligible for Garbage collection.
#### Heap Generations for Garbage Collection 
Java objects are created in Heap and Heap is divided into three parts or generations for the sake of garbage collection in Java, these are called as Young(New) generation, Tenured(Old) Generation and Perm Area of the heap.
![image](https://user-images.githubusercontent.com/20130001/100056889-6a417900-2e4c-11eb-9abc-c04a58dd3072.png)
New Generation is further divided into three parts known as Eden space, Survivor 1 and Survivor 2 space. When an object first created in heap its gets created in new generation inside Eden space and after subsequent minor garbage collection if an object survives its gets moved to survivor 1 and then survivor 2 before major garbage collection moved that object to old or tenured generation.
* Perm space of Java Heap is where JVM stores Metadata about classes and methods, String pool and Class level details.
![image](https://user-images.githubusercontent.com/20130001/100057022-a7a60680-2e4c-11eb-9576-225b73cffc90.png)
You can't force JVM to run Garbage Collection though you can make a request using ```System.gc()``` or ```Runtime.gc()``` method.

##### In java.lang.System

```
public static void gc() {
    Runtime.getRuntime().gc();  
}
```
##### In java.lang.Runtime
```
public native void gc();  // note native  method
```
###### Mark and Sweep Algorithm

This is one of the most popular algorithms used by Garbage collection. Any garbage collection algorithm must perform 2 basic operations. One, it should be able to detect all the unreachable objects and secondly, it must reclaim the heap space used by the garbage objects and make the space available again to the program.
<br>The above operations are performed by Mark and Sweep Algorithm in two phases:
* Mark phase
* Sweep phase
### Stack vs Heap in Java
![image](https://user-images.githubusercontent.com/20130001/100057629-dd97ba80-2e4d-11eb-932a-eec4c929198b.png)
## SpringBoot
* What are the Spring Boot Annotations?
<br>The @RestController is a stereotype annotation. It adds @Controller and @ResponseBody annotations to the class. We need to import org.springframework.web.bind.annotation package in our file, in order to implement it.

* What is Spring Boot dependency management?
<br> Spring Boot manages dependencies and configuration automatically via maven/gradle. You don't need to specify version for any of that dependencies.
Spring Boot upgrades all dependencies automatically when you upgrade Spring Boot.

* What are the Spring Boot properties?
<br>Spring Boot provides various properties which can be specified inside our project's ```application.properties``` file. These properties have default values and you can set that inside the properties file. Properties are used to set values like: server-port number, database connection configuration etc.

* What are the Spring Boot Starters?
Starters are a set of convenient dependency descriptors which we can include in our application.
Spring Boot provides built-in starters which makes development easier and rapid. For example, if we want to get started using Spring and JPA for database access, just include the spring-boot-starter-data-jpa dependency in your project.

* What is Spring Boot Actuator?
<br>Spring Boot provides actuator to monitor and manage our application. Actuator is a tool which has HTTP endpoints. when application is pushed to production, you can choose to manage and monitor your application using HTTP endpoints.

* What is thymeleaf?
<br>It is a server side Java template engine for web application. It's main goal is to bring elegant natural templates to your web application.
<br>It can be integrate with Spring Framework and ideal for HTML5 Java web applications.

* How to use thymeleaf?
<br> In order to use Thymeleaf we must add it into our pom.xml file like:
```
<dependency>    
<groupId>org.springframework.boot</groupId>    
<artifactId>spring-boot-starter-thymeleaf</artifactId>    
</dependency> 
```

* How to connect Spring Boot to the database using JPA?
<br> Spring Boot provides spring-boot-starter-data-jpa starter to connect Spring application with relational database efficiently. You can use it into project POM (Project Object Model) file.

* How to connect Spring Boot application to database using JDBC?
<br> Spring Boot provides starter and libraries for connecting to our application with JDBC. Here, we are creating an application which connects with Mysql database. It includes the following steps to create and setup JDBC with Spring Boot.

* What is @RestController annotation in Spring Boot?
<br> The @RestController is a stereotype annotation. It adds @Controller and @ResponseBody annotations to the class. We need to import org.springframework.web.bind.annotation package in our file, in order to implement it.

* What is @RequestMapping annotation in Spring Boot?
<br> The @RequestMapping annotation is used to provide routing information. It tells to the Spring that any HTTP request should map to the corresponding method. We need to import org.springframework.web.annotation package in our file.

* Spring Vs Spring Boot?
* Spring is a web application framework based on Java. It provides tools and libraries to create a complete cutomized web application.

* Wheras Spring Boot is a spring module which is used to create spring application project that can just run.


* Model - DB model
* Services - CRUD operations
* Controller - REST operations 

## Javascript 
es6
Arrow function anam manm
callback hell
## NodeJs
virtual dom, shdow dom
event loop
## ReactJs
## Redux
## Angular
<br> * Components – A component controls one or more views. Each view is some specific section of the screen. Every Angular application has at least one component, known as the root component. It is bootstrapped inside the main module, known as the root module. A component contains application logic defined inside a class. This class is responsible for interacting with the view via an API of properties and methods.

<br> * Data Binding – The mechanism by which parts of a template coordinates with parts of a component is known as data binding. In order to let Angular know how to connect both sides (template and its component), the binding markup is added to the template HTML.

<br> * Dependency Injection (DI) – Angular makes use of DI to provide required dependencies to new components. Typically, dependencies required by a component are services. A component’s constructor parameters tell Angular about the services that a component requires. So, a dependency injection offers a way to supply fully-formed dependencies required by a new instance of a class.

<br> * Directives – The templates used by Angular are dynamic in nature. Directives are responsible for instructing Angular about how to transform the DOM when rendering a template. Actually, components are directives with a template. Other types of directives are attribute and structural directives.
<br> * Metadata – In order to let Angular know how to process a class, metadata is attached to the class. For doing so decorators are used.
<br> * Modules – Also known as NgModules, a module is an organized block of code with a specific set of capabilities. It has a specific application domain or a workflow. Like components, any Angular application has at least one module. This is known as the root module. Typically, an Angular application has several modules.
<br> * Routing – An Angular router is responsible for interpreting a browser URL as an instruction to navigate to a client-generated view. The router is bound to links on a page to tell Angular to navigate the application view when a user clicks on it.
<br> * Services – A very broad category, a service can be anything ranging from a value and function to a feature that is required by an Angular app. Technically, a service is a class with a well-defined purpose.
<br> * Template – Each component’s view is associated with its companion template. A template in Angular is a form of HTML tags that lets Angular know that how it is meant to render the component.
![image](https://user-images.githubusercontent.com/20130001/100079221-66245400-2e6a-11eb-8595-850749dbe444.png)
<br>
<br>
<br>* ngOnChanges(): This method is called whenever one or more input properties of the component changes. The hook receives a SimpleChanges object containing the previous and current values of the property.
<br>* ngOnInit(): This hook gets called once, after the ngOnChanges hook.
It initializes the component and sets the input properties of the component.
<br>* ngDoCheck(): It gets called after ngOnChanges and ngOnInit and is used to detect and act on changes that cannot be detected by Angular.
We can implement our change detection algorithm in this hook.
<br> * ngAfterContentInit(): It gets called after the first ngDoCheck hook. This hook responds after the content gets projected inside the component.
<br> * ngAfterContentChecked(): It gets called after ngAfterContentInit and every subsequent ngDoCheck. It responds after the projected content is checked.
<br> * ngAfterViewInit(): It responds after a component's view, or a child component's view is initialized.
<br> * ngAfterViewChecked(): It gets called after ngAfterViewInit, and it responds after the component's view, or the child component's view is checked.
<br> * ngOnDestroy(): It gets called just before Angular destroys the component. This hook can be used to clean up the code and detach event handlers.
<br>
```
ng new my-first-project
cd my-first-project
ng serve
```
#### pipes - Use pipes to transform strings

### One-way Data Binding

![image](https://user-images.githubusercontent.com/20130001/100083045-f5336b00-2e6e-11eb-82ff-37c2c0e8653b.png)

```
 <strong>{{firstName}}</strong>
```

### Two-way Data Binding

![image](https://user-images.githubusercontent.com/20130001/100083063-fbc1e280-2e6e-11eb-8952-8baf2c8d0960.png)

```
<input type="text" [(ngModel)] = 'val' />
```

![image](https://user-images.githubusercontent.com/20130001/100083153-14ca9380-2e6f-11eb-9c65-d48292bcb534.png)

