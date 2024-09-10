# SOLID

Explanation about the SOLID principles

<p align="center">
<img height="570" width="1572" alt="Image1" src="https://user-images.githubusercontent.com/13514156/120488978-df9e8100-c37c-11eb-80ec-e1fc38f0f19d.jpeg">
</p>

**SOLID** is an acronym that represents five principles very important when we develop with the **OOP** paradigm, in addition it is an essential knowledge that every developer must know.
Understanding and applying these principles will allow you to write better quality code and the therefore be a better develop

The SOLID principles were defined in the early 2000s by Robert C Martin (Uncle Bob). Uncle Bob elaborated some of these and identified others already existing and said that these principles should be used to get a good management of dependencies in our code

However, in the begining these principle were not yet known as SOLID until Michael Feathers observed that the initials of these principles fit perfectly under the acronym SOLID and that it was also a very representative name for iths definition

These principles help us to obtain the following benefits:
- Ease to maintain
- Ease to extend
- Robust code

But before we see what each SOLID principle means, we need to remember two relevant concepts in the development of any software, The **COUPLING** and the **COHESION**

```COUPLING:```
We can define it as the degree to wich a class, method or any other software entity, is directly linked to another. This degree of coupling can also be seen as a degree of dependence.

```COHESION:```
Is the measuare in which two or more partes of a sustem work together to obtain better results than each part individually

## The SOLID principles

<p align="center">
<img height="470" width="1472" alt="Image1" src="https://user-images.githubusercontent.com/13514156/120490805-54be8600-c37e-11eb-8edc-cff58960e39b.png">
</p>

* **S** - Single Responsability Principle
* **O** - Open Closed Principle
* **L** - Lisko Substitucion Principle
* **I** - Interface Segregation Principle
* **D** - Dependency Inversion Principle


## Single Responsability Principle (SRP)
A class should have a single function.

A class should be responsible for only one thing. The moment it acquires more responsibility it becomes docked, something that is not desirable if you want to ensure the maintenance of the application. This is because changing one of your responsibilities may affect the other and vice versa.

```java

class Vehicle {

    String brand;

    Vehicle(String brand){
        this.brand = brand;
    }

     String getVehicleIdentifier() { 
        return brand;
     }

     void saveVehicle(Vehicle vehicle) {
        //Implementation for save into database...
      }     
}
```

This class violates SRP. Why? The single responsibility principle states that each class should have a single function. However, the class **Vehicle** handles both properties ( getVehicleIdentifier) and database storage ( saveVehicle). This means that if, for example, a change occurs in the storage system of our application, it is likely to affect the way in which the properties are managed, forcing us to change the class Vehicleas well as the classes that are using it.

In other words, the SRP establishes a high degree of stiffness , so that there is no domino effect every time a change occurs.

One way to solve the previous example would be the following:

```java

class Vehicle {
    String brand;

    Vehicle(String brand){
        this.brand = brand;
    }
    String getVehicleIdentifier() { 
        return brand
    }
}

class VehicleDB {
    saveVehicle(Vehicle vehicle) { 
        //Implementation for save into database
    } 
    getVehicle(int idVehicle) {
        return idVehicle;
     }
}
```

## Open-Closed Principle (OCP)

It establishes that the software entities (classes, modules and functions) should be open for their extension, but closed for their modification.

Let's continue with our class **Vehicle**:

If we wanted to iterate through a list of vehicles and print their marks on the screen:

```java
class Vehicle {  
    String brand;

    Vehicle(String brand){ this.brand = brand; }

    String getBrandVehicle(){ return brand; }
}
```

If we wanted to iterate through a list of vehicle and print their brands on the screen:

```java
public static void main(String[] args) {  
    Vehicle[] arrayVehicles = {
            new Vehicle("Ford"),
            new Vehicle("Audi")
    };
    printAveragePriceVehicle(arrayVehicles);
}

public static void printAveragePriceVehicle(Vehicle[] arrayVehicles){  
    for (Vehicle vehicle : arrayVehicles) {
        if(vehicle.brand.equals("Ford")) System.out.println(18000);
        if(vehicle.brand.equals("Audi")) System.out.println(25000);
    }
}
```

This would not fulfill the open / closed principle, since if we decide to add a new vehicle from another brand:

```java
Vehicle[] arrayVehicles = {  
    new Vehicle("Ford"),
    new Vehicle("Audi"),
    new Vehicle("Mercedes")
};
```
We would also have to modify the method that we created previously:

```java
public static void printAveragePriceVehicle(Vehicle[] arrayVehicle){  
    for (Vehicle vehicle : arrayVehicle) {
        if(vehicle.brand.equals("Ford")) System.out.println(18000);
        if(vehicle.brand.equals("Audi")) System.out.println(25000);
        if(vehicle.brand.equals("Mercedes")) System.out.println(27000);
    }
}
```

As we can see, for each new car new logic would have to be added to the printAveragePriceVehicle () method. This is a simple example, but imagine that your application grows and grows… how many modifications would we have to make? Better to avoid this waste of time and headache, right?

To comply with this principle we could do the following:

```java
abstract class Vehicle {  
    // ...
    abstract int averagePriceVehicle();
}

class Ford extends Vehicle {  
    @Override
    int averagePriceVehicle() { return 18000; }
}

class Audi extends Vehicle {  
    @Override
    int averagePriceVehicle() { return 25000; }
}

class Mercedes extends Vehicle {  
    @Override
    int averagePriceVehicle() { return 27000; }
}

public static void main(String[] args) {

    Vehicle[] arrayVehicles = {
            new Ford(),
            new Audi(),
            new Mercedes()
    };

    printAveragePriceVehicle(arrayVehicles);
}

public static void printAveragePriceVehicle(Vehicle[] arrayVehicles){  
    for (Vehicle vehicle : arrayVehicles) {
        System.out.println(vehicle.averagePriceVehicle());
    }
}
```

Each vehicle extends the abstract class **Vehicle** and implements the abstract method averagePriceVehicle().

Thus, each vehicle has its own implementation of the averagePriceVehicle () method, so the printAveragePriceVehicle () method iterates through the array of vehicles and only calls the averagePriceVehicle() method.

Now, if we add a new vehicle, averagePriceVehicle () will not have to be modified. We will only have to add the new vehicle to the array, thus fulfilling the open / closed principle.

## Liskov Substitution Principle (LSP)


It declares that a subclass must be substitutable for its superclass, and if by doing this the program crashes, we are violating this principle.

Fulfilling this principle, it will be confirmed that our program has an easy to understand class hierarchy and reusable code.

Let's see an example:

```java
// ...
public static void printnNumbersSeats(Vehicle[] arrayVehicles){  
    for (Vehicle vehicle : arrayVeicles) {
        if(vehicle instanceof Ford)
            System.out.println(numSeatsFord(vehicle));
        if(vehicle instanceof Audi)
            System.out.println(numSeatsAudi(vehicle));
        if(vehicle instanceof Mercedes)
            System.out.println(numSeatsMercedes(vehicle));
    }
}
printNumSeats(arrayVehicles); 
```

This violates both the Liskov substitution principle and the open / closed principle. The program must know each type of vEHICLE and call its associated numSeats () method.

Thus, if we add a new Vvehicle, the method must be modified to accept it.

```java
// ...
Vehicle[] arrayVehicles = {  
        new Ford(),
        new Audi(),
        new Mercedes(),
        new Ferrari()
};

public static void printnNumbersSeats(Coche[] arrayVehicles){  
    for (Vehicle vehicle : arrayVehicles) {
        if(vehicle instanceof Ford)
            System.out.println(numASeatsFord(vehicle));
        if(vehicle instanceof Audi)
            System.out.println(numSeatsAudi(vehicle));
        if(vehicle instanceof Mercedes)
            System.out.println(numSeatssMercedes(vehicle));
        if(vehicle instanceof Ferrari)
            System.out.println(numSeatsFerrari(vehicle));
    }
}
printnNumbersSeats(arrayVehicles);  
```

For this method to comply with the principle, we will follow these principles:

If the superclass (Vehicle) has a method that accepts a parameter of the type of the superclass (Vehicle), then its subclass (Ford) should accept as an argument a type of the superclass (Vehicle) or a type of the subclass (Ford).
If the superclass returns a type of itself (Vehicle), then its subclass (Ford) should return a type of the superclass (Vehicle) or a type of the subclass (Ford).
If we implement the previous method again:

```java
public static void printnNumbersSeats(Vehicle[] arrayVehicles){  
        for (Vehicle vehicle : arrayvehicles) {
            System.out.println(vehicle.numSeats());
        }
    }

printnNumbersSeats(arrayVehicles); 
```
Now the method doesn't care about the class's type, it just calls the superclass's numSeats () method. It only knows that the parameter is of type vehicle, either Vehicle or one of the subclasses.

For this, now the Vehicle class must define the new method:

```java
abstract class Vehicle {

    // ...
    abstract int numSeats();
}
```
And the subclasses must implement such a method:

```java
class Ford extends Vehicle {

    // ...
    @Override
    int numSeats() {
        return 5;
    }
}
// ...
```
As we can see, now the printnNumbersSeats () method does not need to know what type of car it is going to perform its logic with, it simply calls the method numSeats () of the Vehicle type, since by contract, a subclass of Vehicle must implement said method.

## Interface Segregation Principle (ISP)
This principle states that clients should not be forced to rely on interfaces they do not use.

In other words, when a client depends on a class that implements an interface whose functionality this client does not use, but which other clients do use, this client will be affected by the changes that other clients force on that interface.

Let's imagine that we want to define the necessary classes to house some types of birds. For example, we would have parrots, toucans, and hawks:

```java
interface IBird {  
    void fly();
    void eat();
}

class Parrot implements IBird{

    @Override
    public void fly() {
        //...
    }

    @Override
    public void eat() {
        //..
    }
}

class Hawk implements IBird{  
    @Override
    public void fly() {
        //...
    }

    @Override
    public void eat() {
        //..
    }
}
```
So far so good. But now let's imagine we want to add the penguins. These are birds, but they also have the ability to swim. We could do this:

```java
interface IBird {  
    void fly();
    void eat();
    void swim();
}

class Parrot implements IBird{

    @Override
    public void fly() {
        //...
    }

    @Override
    public void eat() {
        //...
    }

    @Override
    public void swim() {
        //...
    }
}

class Penguin implements IBird{

    @Override
    public void fly() {
        //...
    }

    @Override
    public void comer() {
        //...
    }

    @Override
    public void eat() {
        //...
    }
}
```

The problem is that the parrot does not swim, and the penguin does not fly, so we would have to add an exception or warning if we try to call these methods. Furthermore, if we wanted to add another method to the IAve interface, we would have to go through each of the classes that implements it and add the implementation of that method to all of them. This violates the principle of interface segregation, since these classes (the clients) do not have to depend on methods they do not use.

The best thing to do would be to segregate the interfaces further, as necessary. In this case we could do the following:

```java
interface IBird {  
    void eat();
}
interface IFlyingBird {  
    void fly();
}

interface ISwimmingBird {  
    void swim();
}

class Parrot implements IBird, IFlyingBird{

    @Override
    public void fly() {
        //...
    }

    @Override
    public void eat() {
        //...
    }
}

class Penguin implements IBird, ISwimmingBird{

    @Override
    public void swim() {
        //...
    }

    @Override
    public void eat() {
        //...
    }
}
```

Thus, each class implements the interfaces of which it really needs to implement its methods. When adding new functionalities, this will save us a lot of time, and in addition, we comply with the first principle (Single Responsibility).

## Dependency Inversion Principle (DIP)

It states that the dependencies must be in the abstractions, not in the concretions. That is to say:

* High-level modules should not rely on low-level modules. Both should depend on abstractions.
* Abstractions shouldn't depend on details. Details should depend on abstractions.
At some point our program or application will become made up of many modules. When this happens, it is when we must use dependency injection, which will allow us to control the functionalities from a specific place instead of having them spread throughout the program. Furthermore, this isolation will allow us to perform testing much more easily.

Suppose we have a class to access data, and we do it through a DB:

```java
class DatabaseService{  
    //...
    void getData(){ //... }
}

class AccessData {

    private DatabaseService databaseService;

    public AccessData(DatabaseService databaseService){
        this.databaseService = databaseService;
    }

    Dato getData(){
        databaseService.getDatos();
        //...
    }
}
```

Let's imagine that in the future we want to change the DB service for a service that connects to an API. For a minute to think what should be done ... Do you see the problem? We would have to modify all the instances of the AccesoADatos class, one by one.

This is because our high-level module (AccesoADatos) depends on a lower-level module (DatabaseService), thus violating the principle of dependency inversion. The high-level module should depend on abstractions.

To fix this, we can make the DataAccess module depend on a more generic abstraction

```java
interface Connection {  
    Data getData();
    void setData();
}

class AccessData {

    private Connection conectionn;

    public AccessData(Connection connection){
        this.connection = connection;
    }

    Data getData(){
        connection.getData();
    }
}
```

Thus, regardless of the type of connection that is passed to the AccessData module, neither it nor its instances will have to change, so we will save a lot of work.

Now, each service that we want to pass to AccessData must implement the Connection interface:

```java
class DatabaseService implements connection {

    @Override
    public Data getData() { //... }

    @Override
    public void setData) { //... }
}

class APIService implements Connection{

    @Override
    public Data getData() { //... }

    @Override
    public void setData() { //... }
}
```

Thus, both the high-level and low-level modules depend on abstractions, so we comply with the principle of inversion of dependencies. Furthermore, this will force us to comply with the Liskov principle, since the types derived from Connection (DatabaseService and APIService) are substitutable by their abstraction (Connection interface).
