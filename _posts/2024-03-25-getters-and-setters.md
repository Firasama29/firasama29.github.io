---
title: Java Questions and Answers (Part 1) — Type-Casting
date: 2024-04-02
categories: [java]
tags: [java, type-casting]
---

# What are Getters and Setters in Java and Why Use Them?

Getters and setters are simply methods that are commonly used in Java applications to access and modify private fields of a class. They are typically defined with the below syntax:

```java
  public class Example {
    private String name;
    
    //getter
    public String getName() {
        return name;
    }
    
    //setter
    public void setName(String name) {
        this.name = name;
    }
  }
```

As you can see, we created a class with a private field and two methods:

- `getName()`: used to fetch the name variable from the class. Since it’s private, it can’t be accessed directly. The getter method provides a way to retrieve it.
- `setName(String name)`: allows other classes to modify the internal state of a class by updating the value of its private field. Setter methods accept a variety of parameters, including primitive types, objects, and complex data structures like arrays and collections.

By convention, the getters start with ‘get’ while setters start with ‘set’ followed by the variable name.

Now let’s invoke the getter and setter methods:

```java
  public class AnotherClass {
  
    public static void main(String[] args) {
        Example example = new Example();
    
        //getter
        String name = example.getName();
       
        System.out.println(name);
        
        //setter
        example.setName("example class");
       
        System.out.println(name);
    }
  }
```

In the above class, we created an instance of Example class then we did the following:

- We invoked the getter method and assigned it to a String variable. We then printed the output. The output is null here because it doesn’t hold any value yet, which is the default for objects.
- Next, we invoked the setter method and passed a String inside. The name property is now updated. The output is: `example class`.

## What’s the point of using getters and setters?
We’ve seen earlier how to use these two methods, but they’re not the only way to retrieve and modify fields of a class. We can simply declare a public variable and call it or update it directly:

```java
  //set the value
  obj.name = "public variable";
  
  //retrieve the value
  System.out.println(obj.name);
```

If this approach is possible, why even introduce a middleman when we can simply expose the variable directly?

Well, getters and setters are the conventional approach in Java due to various reasons. Let’s discuss a couple of them:

- They’re designed to provide an indirect and controlled access to variables. In other words, they provide an additional functionality such as validation in cases where you want others to use variables of your class in the manner that YOU want:

```java
  class Item {
    private int number;
    
    public void setNumber(int number) {
      if(number < 0 || number > 10) {
        throw new IllegalStateException("Number is not within valid range");
      }
      this.number = number;
    }
    
    public int getNumber() {
      if(number < 0 || number > 10) {
        throw new IllegalStateException("Number is not within valid range");
      }
      return number;
      }
  }
```

In the above example, we defined a constraint in each method to ensure that we’re able to update number and retrieve it only if the value is between 0 and 10, If the constraint is violated, an exception will be thrown.

- They play a crucial role in data encapsulation, which is a fundamental concept in Object Oriented Programming. Using this approach, you can hide the internal implementation and let the caller interact with the data as you dictate, unlike the case where you publicly expose the internal implementation to external users, potentially risking unexpected results.

## Conclusion
Getters and setters in Java provide restricted access to private member variables of a class, ensuring safe use of the variables. We discussed what getters and setters are, their syntax, how they’re different from the approach of direct exposure of variables and finally, we discussed why we should even use them in the first place.

Thanks for reading!


