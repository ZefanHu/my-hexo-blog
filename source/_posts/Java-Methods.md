---
title: Java Methods
date: 2024-02-15 01:01:22
categories: 
    - "Java Web"
tags: 
    - "Java"
    - "Job-Application"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240215010921.png
---

# Use of Java Methods

when we are coding Java program, we usually need deal with thousands of line code. To maintain code clarity and manageability. Java provide a concept --- Method.

## Nature of Methods

Java methods are a collection of statements that encapsulate code to perform a specific task. They have the following properties:

\- **Problem-solving tools**: Methods are ordered combinations to solve a particular set of problems.

\- **Encapsulation**: Methods are usually contained within classes or objects.

\- **Callable**: Methods are defined in the program and can be called from other methods.

\- **Atomicity**: Each method focuses on completing a specific task, maintaining a single functionality.

## Main Method and Other Methods

\- **Main Method**: It's the entry point of the program, called by the Java runtime environment.

\- **Other Methods**: Defined by developers to implement specific functionalities.

## Declaration of Methods

Java method declaration follows a specific format, including modifiers, return type, method name, and parameter list:

```java
modifier returnType methodName(parameterType parameterName) {
    // Method body
    return returnValue;
}
```

### Example: Compare Two Numbers to Find the Maximum

```java
public static int max(int num1, int num2) {
    int result;
    if (num1 > num2) {
        result = num1;
    } else {
        result = num2;
    }
    return result;
}
```

## Method Invocation

**Static Methods**: Called directly using the class name.

**Instance Methods**: Require the creation of a class instance before calling.

### Static Method Invocation Example

```java
public class Demo {
    public static void main(String[] args) {
        int maxNumber = max(20, 30);
        System.out.println("最大值为: " + maxNumber);
    }
}
```

### Instance Method Invocation Example

```java
public class Demo {
    public static void main(String[] args) {
        Demo demo = new Demo();
        int maxNumber = demo.max(20, 30);
        System.out.println("最大值为: " + maxNumber);
    }
}
```

## Method Overloading

Method overloading allows defining multiple methods with the same name but different parameter lists within the same class.

### Conditions for Method Overloading

\- The method name must be the same.

\- The parameter list must be different (in number, type, or order of parameters).

\- The return type can be the same or different; it does not affect overloading.

## Variable Parameters

Java supports variable parameters, allowing a variable number of parameters to be passed in when a method is called. In this example, numbers is actually an int[] array. You can call the sum method just as you would any normal method, passing in any number of integers.

```java
public static int sum(int... numbers) {
    int total = 0;
    for (int num : numbers) {
        total += num;
    }
    return total;
}
```

## Value Passing and Reference Passing

\- Value passing: the method receives a copy of the parameter values and the original data is not changed.
\- Reference passing: the method receives a reference to the object and may modify the original object.

### Value Passing Example

```java
public class Demo2 {
    public static void main(String[] args) {
        int a = 1;
        System.out.println("调用前: " + a);
        change(a);
        System.out.println("调用后: " + a);
    }

    public static void change(int a) {
        a = 10;
    }
}
```

### Reference Passing Example

Objects in Java are stored in heap memory. When you create an object, such as Person person = new Person();, the person variable actually stores a reference to the Person object in heap memory, not the object itself.

When you pass the person object as a parameter to a method, a copy of this reference is passed, not a copy of the object itself. Therefore, the method receives another reference to the same object.

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}

public class Test {
    public static void main(String[] args) {
        Person person = new Person("John");
        System.out.println("Before: " + person.name); // 输出 John
        changeName(person);
        System.out.println("After: " + person.name); // 输出 Mike
    }

    public static void changeName(Person p) {
        p.name = "Mike";
    }
}
```

Notice
Invariance of references: while you can modify the state of an object (i.e., its properties) by reference, you cannot change the object to which the reference itself points. For example, if you try to point a reference to a new object in a method, this will not affect the original reference.

Special case of primitive data types: Primitive data types in Java (e.g. int, double, etc.) use value passing, not reference passing. This means that methods receive copies of the original values, and any modifications to those values will not affect the original data.