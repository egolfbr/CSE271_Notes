# CSE 271 Notes

## Table of Contents 

**[Java Classes](#Java-Classes)**<br>
**[UML](#uml)**<br>
**[Type Casting](#type-casting)**<br>
**[Abstract Classes](#Abstract-Classes-and-Methods)**<br>
**[Interfaces](#interfaces)**<br>
**[Unit Testing and Debugging](#Unit-Testing)**<br>

## Java Classes  

4 pillars of object-oriented programming:

- Abstraction
- Polymorphism
- Inheritence
- Encapsulation

Encapsultation: Ability to encapsulate all neccessary variables and functions.

Abstraction: Ability to hide details of how the class operates.

Inheritence: Ability to inherit variables and functions from a parent class.

Polymorphism: Child class can override parent class function.

When we have a parent class that does not have enough information to implement
the functions in it, we have two options. 1. return null or dummy variables
until we gain the information. 2. Make the class abstract.

In Java classes, each class must have a constructor. A constructor is a method
that creates the object of said class. It is defined as follows

```public ClassName(){}```

The ```ClassName``` of the constructor must be the EXACT same as the class file
name and the class name. The example constructor is one that that takes in no
variables and does noting in the body. It is possible to make multiple constructors
to handle different construction scenarios. Take the following class as an example

```java
public Class Fruit{
    private double cost;
    private double weight; 
    private int amount;

    public Fruit(){};
    public Fruit(int amount, double cost, double weight){
        this.cost = cost;
        this.amount = amount; 
        this.weight = weight;
    }
}
```

This is a class named ```Fruit``` with a few private instance variables and two
constructors. The two constructors tell us that we can construct on object of
type ```Fruit``` in two different ways. We can either pass in no variables or
three variables.

What we can also do is this:

```java
public Class Fruit{
        private double cost;
        private double weight; 
        private int amount;

        public Fruit(){
        this(0,0.0,0.0);
    };

    public Fruit(int amount){
        this(amount,0.0,0.0);
    };

    public Fruit(int amount, double cost){
        this(amount, cost, 0.0);
    }
        
    public Fruit(int amount, double cost, double weight){
                this.cost = cost;
                this.amount = amount; 
                this.weight = weight;
        }
}
```

Here we see that the constructor can call a method named ```this```. This means
that the method will call the constructor but with whatever variables are passed
into the ```this``` method. Thus, when 0 parameters are passed, the program will
go into the ```Fruit()``` constructor, see that I called ```this(0,0.0,0.0)```
and execute the ```Fruit(int amount, double cost, double weight)``` constructor.

This is useful when there are many different ways that a person can construct
an object and there are many variables to fill.

Another tool that java allows is called the ```super``` command. This allows
a programmer to call the constructer in a parent class. Look at the following
example.

```java
public class Food(){
        public Food(int num, String type){};
}
```

```java
public class Fruit() extends Food{
        public Fruit(){
            super(0,"strawberry");
        }
}
```

As you can see, the ```Fruit``` class extends the ```Food``` class. This means that
```Fruit``` is a child of ```Food```. ```Fruit``` inherits instance variables and functions
of ```Food``` and when we call the constructor in ```Fruit``` the ```super``` call will call
the constructor in the class above (the parent), in our case the ```Food``` class. A person
can only call ```this()``` OR ```super()``` NOT both. You can however call a ```this()``` and
in a different constructor call ```super```.

## UML

-: private

+: public

\#: protected

~: package access

->: From child to parent (solid line)
-->: From class to interface (dashed line)

Example:

```dot
digraph G {
    node [shape=box]
    Shape [label="Shape\n#x: int\n#y: int\n#height: int\n#width: int\n#color: String\n-----------\n+Shape(String)\n+getArea(): Double"]
    Circle [label="Circle\n----------\n+Circle(int)\n+getArea(): double"]
    Rectangle [label="Rectangle\n----------\n+Rectangle(int)"]
    Triangle [label="Triangle\n----------\n+Triangle(int)\n+getArea(): double"]
    Shape -> Circle [dir=back]
    Shape -> Rectangle [dir=back]
    Shape -> Triangle [dir=back]
}
```

## Type Casting

Upcasting: From child class to parent class. ```Parent p = child;```

This is a "safe" type cast becasue if a child reference is valid, then the parent reference has to valid.

Downcasting: From parent to child class. ```Child c = (parent) p;```

This is "unsafe" because there is no guarantee that a parent reference points to a valid child object. This
can casue a runtime exception.

```instanceof``` operator return boolean of if an object is an instance if a certain class.
Returns True if the specified class is present in teh descendents.

comparing objects can be as easy as using ```.equals()``` method. The method takes in another object
and compares to the implicit parameter ```this```.

## Abstract Classes and Methods

Slanted chars in UML. Abstract classes are incomplete classes. They do not have all the info that they need.

```public abstract String getName()```

The derived classes will override this. Abstract method has to have the abstract identifier, no body and the class,
must also have the abstract identifier before ```class``` tag. If there is ONE abstract method, the class must become
abstract as well. You cannot instantiate an abstract class.

```java
public abstract class Thing{
   public Thing(){}

   public static void main(){
        Thing th = new Thing();
   }
}
```

This is invalid and cannot be done since ```Thing``` is abstract.

## Interfaces

When a class needs to implement more than 1 functions from a given class, you can use interfaces. Interfaces are defined as follows:

```java
public interface example {
 public void sayHi();
}
```

Now we can create a class that extends another class AND implements the interface like this:

```java
public class myClass extends mySuperClass implements example{
    public void sayHi(){
        System.out.println("Hello World");
    }
}
```

All methods in an interface MUST be abstract. This is by definition. That way each class that implements it, can override those methods.

### Clone

Java has a ```clonable```interface. This interface has no functions. Example usage:

```java
public class Person implements clonable {
    public Person clone() {
        return Person(this);
    }
}
```

There are two types of copying data: Shallow and deep copies.

### Shallow Copies

Shallow copies reuse the object references. This means that if I make 3 shallow copies of an object and change a value in one of them, then that value will be changed for all 3 copies.

Example:

```java
public class Student implements
Cloneable, Comparable<Student> {

    protected String name;
    protected int[] scores;

    public Student(Student src) {
        this.name = src.name;
        this.scores = src.scores;
    }

    @Override
    public Student clone() {
        return new Student(this);
    }
}
```

### Deep Copies

Deep copies creates new objects and copies the values. This will create separate objects so a change in object 2 will not affet object 1. This increases memory usage.

Example:

```java
public class Student implements
Cloneable, Comparable<Student> {

    protected String name;
    protected int[] scores;

    public Student(Student src) {
        this.name = src.name;
        this.scores = Arrays.copyOf(src.scores,src.scores.length);
    }

    @Override
    public Student clone() {
        return new Student(this);
    }
}
```

## Unit Testing

Two types of errors:

- Semantic errors: Logic errors, will compile but program output is not what is expected. Hard to debug, usually need debugger.
- Syntax errors: Error in how one wrote the code. Will not compile, easier to find. Might need documentation to fix.

Different types of testing:

- Unit testing: Test methods and classes.
- Functional testing: Test functionality of features.
- Regression testing: Aims to detect if existing features are broken.
- Integration testing: Tests interoperability between software systems.
- Performance testing: Tests speed, response time, scalability etc.

It is impossible to test a program completly. Typically 80% coverage is the standard. Testing DOES NOT prove the abscence of errors in code. Simply means they have not been found yet. It is impossible to test every set of inputs to a method or class so you should focus on edge cases and outliers that are most likely to expose bugs.

Unit testing is when one looks for errors in an isolated subsystem. Java provides a class called ```JUnit``` which provides the framework for testing java code. Typically when creating a class,
a programmer will create the class i.e. ```exampleClass``` and also create a testing class ```exampleClassTester```. Inside the tester class will be different methods for testing different cases and
methods in the developed class. ```JUnit``` provides the assert statements to try and find the errors in the code.

### JUnit Methods

|   Methods   |   Description   |
|-------------|-----------------|
| ```assertTrue(test)``` | Fails if test is false.|
| ```assertFalse(test)``` | Fails if test is true.|
| ```assertEquals(expected, actual)``` | Fails if expected is not equal to actual.|
| ```assertSame(expected, actual)``` | Fails if expected is not same as actual (by ==)|
| ```assertNotSame(expected,actual)``` | Fails if expected is same as actual (by ==)|
| ```assertNull(value)``` | Fails if value is not null.|
| ```assertNotNull(value)``` | Fails if value is null. |
| ```fail()``` |  Causes current test to immediately fail.|

You can add error messages to the above tests by adding a string to the input parameters as follows:

```assertEquals("The two objects failed the equals unit test", expected, actual)```

It is often neccessary to test for exceptions below is an example.

```java
@Test(expected=ArrayIndexOutOfBoundsException.class)
public void testMethod() {
    ArrayList<Integer> list = new ArrayList<Integer>();
    list.get(4);
}
```

You can also test for timeouts which are useful for infinite loop scenarios.

```java
@Test(timeout = 500)
public void methodToTest(){
    int i = 0;
    while(true){
        i++;
    }
}
```

### How to use JUnit in Eclipse

- Right Click on class in project overview window
- Select ```Other``` at bottom of pop up menu.
- Select ```JUnit 4 test``` Option.
  - Optionally, you can select the class from this menu at the bottom of the screen.
- Select methods for testing.
- Select ```Coverage as``` option once you right click on the class agian.
- Select the ```JUnit Test``` option.
- View Report.
