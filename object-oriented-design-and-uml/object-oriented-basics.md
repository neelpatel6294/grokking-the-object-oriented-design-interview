<h1 align="center">Object Oriented Basics</h1>

Object-oriented programming (OOP) is a style of programming that focuses on using objects to design and build applications. Contrary to procedure-oriented programming where programs are designed as blocks of statements to manipulate data, OOP organizes the program to combine data and functionality and wrap it inside something called an “Object”.

If you have never used an object-oriented programming language before, you will need to learn a few basic concepts before you can begin writing any code. This chapter will introduce some basic concepts of OOP:

* **Objects:** Objects represent a real-world entity and the basic building block of OOP. For example, an Online Shopping System will have objects such as shopping cart, customer, product item, etc.

* **Class:** Class is the prototype or blueprint of an object. It is a template definition of the attributes and methods of an object. For example, in the Online Shopping System, the Customer object will have attributes like shipping address, credit card, etc., and methods for placing an order, canceling an order, etc.

**A Simple Code Snippet of Class and Object**

**Class Code Snippet:**

```Java 
import java.util.HashMap;
import java.util.Map;

public class ShoppingCart {
    private double total;
    private Map<String, Integer> items;

    public ShoppingCart() {
        this.total = 0;
        this.items = new HashMap<>();
    }

    public void addItem(String itemName, int quantity, double price) {
        this.total += (quantity * price);
        this.items.put(itemName, this.items.getOrDefault(itemName, 0) + quantity);
    }

    public void removeItem(String itemName, int quantity, double price) {
        if (this.items.containsKey(itemName)) {
            int currentQuantity = this.items.get(itemName);
            if (quantity >= currentQuantity) {
                this.items.remove(itemName);
                this.total -= (currentQuantity * price);
            } else {
                this.items.put(itemName, currentQuantity - quantity);
                this.total -= (quantity * price);
            }
        }
    }

    public String checkout(double cashPaid) {
        if (cashPaid < this.total) {
            return """
                   You paid %f but cart amount is %f
                   """.formatted(cashPaid, this.total);
        }
        double balance = cashPaid - this.total;
        return """
               Exchange amount: %f
               """.formatted(balance);
    }
}

```

**Object and it's Uses Code Snippet:**
```Java
public class ShoppingCartTest {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // Add items
        cart.addItem("A", 10, 50);
        cart.addItem("B", 5, 20);

        // Remove items
        cart.removeItem("B", 1, 20);

        // Checkout
        String result = cart.checkout(600);

        // Output results
        System.out.println("Total cart amount: " + cart.getTotal());
        System.out.println("Cart items: " + cart.getItems());
        System.out.println(result);
    }
}
```

**Response:**
```
Total cart amount: 580
Cart items: {'B': 4, 'A': 10}
Exchange amount: 20
``` 

<p align="center">
    <img src="/media-files/oop-principles.svg" alt="OOP Principles">
</p>

The four principles of object-oriented programming are encapsulation, abstraction, inheritance, and polymorphism.

* **Encapsulation:** Encapsulation is the mechanism of binding the data together and hiding it from the outside world. Encapsulation is achieved when each object keeps its state private so that other objects don’t have direct access to its state. Instead, they can access this state only through a set of public functions.

**Encapsulation Code Snippet:**

```Java
public class Product {
    private int maxPrice;

    public Product() {
        this.maxPrice = 900; // default max price
    }

    public void sell() {
        System.out.println("Selling Price: " + this.maxPrice);
    }

    public void setMaxPrice(int price) {
        this.maxPrice = price;
    }

    public static void main(String[] args) {
        Product product = new Product();
        product.sell(); // Display the initial selling price

        // Attempt to change the price directly (this would not compile if uncommented)
        // product.maxPrice = 1000; // This line would cause a compile error in Java

        // Using setter function to change the price
        product.setMaxPrice(1000);
        product.sell(); // Display the new selling price
    }
}
```


**Response:**
```
Selling Price: 900
Selling Price: 900
Selling Price: 1000
```

* **Abstraction:** Abstraction can be thought of as the natural extension of encapsulation. It means hiding all but the relevant data about an object in order to reduce the complexity of the system. In a large system, objects talk to each other, which makes it difficult to maintain a large code base; abstraction helps by hiding internal implementation details of objects and only revealing operations that are relevant to other objects.

**Abstraction Code Snippet:**

```Java
from abc import ABC, abstractmethod

abstract class Parent {
    // Common method in the abstract class
    public void common() {
        System.out.println("In common method of Parent");
    }

    // Abstract method that must be overridden
    public abstract void vary();
}

class Child1 extends Parent {
    @Override
    public void vary() {
        System.out.println("In vary method of Child1");
    }
}

class Child2 extends Parent {
    @Override
    public void vary() {
        System.out.println("In vary method of Child2");
    }
}

public class Main {
    public static void main(String[] args) {
        // Object of Child1 class
        Child1 child1 = new Child1();
        child1.common();
        child1.vary();

        // Object of Child2 class
        Child2 child2 = new Child2();
        child2.common();
        child2.vary();
    }
}

```


**Response:**
```
In common method of Parent
In vary method of Child1
In common method of Parent
In vary method of Child2
```

* **Inheritance:** Inheritance is the mechanism of creating new classes from existing ones.

**Inheritance Code Snippet:**

```Java
class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public boolean isEmployee() {
        return false;
    }
}

class Employee extends Person {
    public Employee(String name) {
        super(name);
    }

    @Override
    public boolean isEmployee() {
        return true;
    }
}

public class Main {
    public static void main(String[] args) {
        Person emp = new Person("Person 1");
        System.out.println(emp.getName() + " is employee: " + emp.isEmployee());

        emp = new Employee("Employee 1");
        System.out.println(emp.getName() + " is employee: " + emp.isEmployee());
    }
}

```


**Response:**
```
Person 1 is employee: False
Employee 1 is employee: True
```

* **Polymorphism:** Polymorphism (from Greek, meaning “many forms”) is the ability of an object to take different forms and thus, depending upon the context, to respond to the same message in different ways. Take the example of a chess game; a chess piece can take many forms, like bishop, castle, or knight and all these pieces will respond differently to the ‘move’ message.

**Polymorphism Code Snippet:**

```Java
interface ChessPiece {
    void move();
}

class Bishops implements ChessPiece {
    @Override
    public void move() {
        System.out.println("Bishops can move diagonally");
    }
}

class Knights implements ChessPiece {
    @Override
    public void move() {
        System.out.println("Knights can move two squares vertically and one square horizontally, or two squares horizontally and one square vertically");
    }
}

public class ChessGame {
    public static void main(String[] args) {
        ChessPiece bishop = new Bishops();
        ChessPiece knight = new Knights();

        moveTest(bishop);
        moveTest(knight);
    }

    public static void moveTest(ChessPiece chessPiece) {
        chessPiece.move();
    }
}

```


**Response:**
```
Bishops can move diagonally
Knights can move two squares vertically and one square horizontally, or two squares horizontally and one square vertically
```
