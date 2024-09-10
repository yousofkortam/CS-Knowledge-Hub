# Java 8 Default method

One of the novelties that Java 8 brings at the language level, are the **Default Methods**, which allow adding methods implemented in the interfaces.

```java
public default double getTotal(){
    return getQuantity() * getPrice();
}
```

- The default methods are used when it is necessary to implement a body without the need to implement an abstract class separately,

- In addition the default methods can be overridden in case the default implementation does not meet what we require. 
- default methods do not replace the abstract classes. 

Let's see a limitation of the default methods:

```java
public default double getTotal(){
    return quantity * price;
}
```

## Practical example

Let's imagine that we are programming a system that processes orders of various types, as in the case of an ERP, this system could create various types of orders, such as a purchase, sale, shipment order, etc. All these orders could have something in common, such as, for example, a total, a list of items associated with the order and the items could have a number of products and a price, likewise the details would have a total.

```java
package com.alejo.defaultmethod;

import java.util.List;

public interface IOrder {
    
    public List<IOrderItem> getOrderItem();
    
    public default double getTotal(){
        double total = 0;
        for (IOrderItem item : getOrderItem()) {
            total += item.getTotal();
        }
        return total;
    }
    
    public default void addOrderItem(IOrderItem orderItem){
        getOrderItem().add(orderItem);
    }
}
```

Let's analyze the methods of the **IOrder** class:

* `getOrderItem`: method that will return the items that the order has, we cannot implement it because it is not possible to define properties at class level, except constants. So the class that implements it must return a list of items.

* `getTotal`: this method will return the total of the order, that is, the sum of all the items in the order. How much with the default operator which allows us to implement it by going through all the items of the order and adding their total.

* `addOrderItem`: default method that allows us to easily add new items to the order.

```java
package com.alejo.defaultmethod;

public interface IOrderItem {
    
    public double getQuantity();
    public double getPrice();
    
    public default double getTotal(){
        return getQuantity() * getPrice();
    }
}
```

This second interface represents the items of the order, as well as the methods:

* `getQuantity`: it must return the quantity of products that the line has.

* `getPrice`: it must return the price per product.

* `getTotal`: default method that calculates the total of the line, multiplying the quantity by the price.

Up to this point we have implemented all the default methods and we will continue to create a couple of concrete classes that implement these two interfaces to see how it would work at runtime. For this we will create the following classes:

```java
package com.alejo.defaultmethod;

import java.util.ArrayList;
import java.util.List;

public class SalesOrder implements IOrder{
    
    private List<IOrderItem> orderItems;

    @Override
    public List<IOrderItem> getOrderItem() {
        if(orderItems==null){
            orderItems = new ArrayList<>();
        }
        return orderItems;
    }
```

In this class it will represent a sales order, for which it must implement the `IOrder` interface, but as we can see, it will only ask us to implement the `getOrderItem` method which is the only one that is not default, the `getTotal` and `addOrderItem` methods will already be implemented by default and can be used without having to declare them in the class.

The `getOrderItem` method only returns the list of items, in case the list is null it creates it and returns it.

The following class represents the items of the sales order:

```java

package com.alejo.defaultmethod;

public class SalesOrderItem implements IOrderItem {
    
    private double quantity;
    private double price;

    public SalesOrderItem(double quantity, double price) {
        this.quantity = quantity;
        this.price = price;
    }
    
    @Override
    public double getQuantity() {
        return quantity;
    }

    @Override
    public double getPrice() {
        return price;
    }
    
}
```

We can see that they implement the `IOrderItem` interface which will ask us to implement the `getQuantity` and getPrice methods which we have already explained. The `getTotal` method is already implemented by default from the interface, so it will no longer be necessary to implement it in the class.

Finally, the `Main` class, where we will test what we have learned to see how the default methods behave at runtime:

```java
Main.java

package com.alejo.defaultmethod;

public class Main {
    public static void main(String[] args) {

        SalesOrder salesOrder = new SalesOrder();
        salesOrder.addOrderItem(new SalesOrderItem(10, 100));
        salesOrder.addOrderItem(new SalesOrderItem(1, 20));
        System.out.println("total ==> " +salesOrder.getTotal() );
    }
}

```
The first thing we do is create an instance of the Sales Order (line 10), then in lines 11 and 12 we add two new items. Note that the **addOrderItem** method is implemented as default in the **IOrder** class. Each item defines a price and quantity.


Finally, in line 13 we print the total of the order, the total of the order is calculated from the default method of the **IOrder** class by adding the total of the items and the total of each item is also calculated by the default method implemented from the interface IOrderItem.

As a result of the execution we will have:

```
Output:
total ==> 1020.0
```

