---
title: Java-Basic
date: 2024-01-13 20:17:10
tags: 
    - "Java"
    - "Job-Application"

thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240113202037.png
---

##  Java Comments

* `single-line`: //
* `multi-line`: /* */
* `documentation comments`: /** */

## Identifiers and Keywords

**Keywords**

- `class`: Defines a class.

- `public`, `private`, `protected`: Access modifiers for classes and members.

- `static`: This keyword denotes that a particular member (variable or method) belongs to the class itself, rather than to instances of the class. This means that it can be accessed without creating an object of the class.


  ```java
  class MyClass {
      static int staticVariable = 10;
  	static void staticMethod() {
      System.out.println("This is a static method.");
      }
  }
  // Usage
  int value = MyClass.staticVariable; // Accessing static variable
  MyClass.staticMethod(); // Calling static method
  ```

- `void`: Specifies that a method does not return a value.

- `if`, `else`: Used in conditional statements.

- `for`, `while`, `do`: Loop control structures.

- `try`, `catch`, `finally`: Exception handling blocks.

- `return`: Exits a method and optionally returns a value.

- `import`: Includes other Java packages into a class.

- `new`: Keyword is used to create new instances (objects) of a class. It allocates memory for the new object and returns a reference to that memory.

  ```java
  class MyClass {
      int instanceVariable;
  	MyClass() {
      	instanceVariable = 5;
  	}
  }
  // Usage
  MyClass myObject = new MyClass(); // Creating a new object
  int value = myObject.instanceVariable; // Accessing instance variable
  ```

**Considerations for Identifiers:**

- All identifiers must begin with a letter or underscore.
- Following the initial character, the identifier can consist of letters, underscores, and digits in any combination.
- Avoid using keywords as identifiers.
- Identifiers are case-sensitive.
- While it's possible to name identifiers in Chinese, it is not recommended.

## Data Types

**The data types in Java are divided into primitive types and reference data types.**

**Primitive types include:**

- Numerical values
- Boolean (true and false)

**Numerical values include:**

- Integer (byte, short, int, long)
- Floating point numbers (float, double)
- Characters (char)

**Reference data types include:**

- Classes
- Interfaces
- Arrays

## Type Conversion

**The priority of data types is: byte, short, char -> int -> long -> float -> double**

**When converting from higher to lower: explicit casting is required**.

**When converting from lower to higher: automatic conversion occurs.**

**Note:**

- Boolean values cannot be converted
- Objects cannot be converted to unrelated types
- Precision issues may arise during the conversion process

## Variables

**Java variables are divided into three types:**

* `Class variables`: Created directly in the class starting with 'static': static int a;They can be directly output in methods.

* `Instance variables`: Created directly in the class: int b; Numeric types are initialized to 0, boolean types are initialized to false;

  When using in methods, you need to first create a variable of the variable type before you can use the instance variable in this variable

  ```java
  VariableType variableName = new VariableClass ();
  variableName.instance variable;
  ```

* `Local variables`: Variables defined in methods, can only be used in that method

## Constants

**A variable whose value cannot be changed after it is defined.**

```java
final int MAX_HEIGHT = 200;
```

## Naming Conventions

- `Class members`: The first letter is lowercase, following camel case.
- `Method names`: The first letter is lowercase, following camel case.
- `Local variables`: The first letter is lowercase, following camel case.
- `Class names`: The first letter is uppercase, following camel case.
- `Constants`: All uppercase letters with underscores.

## Operators

**Arithmetic operators are not written here, only two notes are written.**

**Note:**

- The logical operator && has a short-circuit situation. When the condition on the left is not true, the condition on the right is not judged.
- Bit operations are performed as required according to the binary system

## Package System

- In order to better organize classes, Java provides a package mechanism to distinguish the namespace of class names.

- Definition: package pkg1 (automatically written in idea)

- Generally, the inverted company domain name is used as the package name (for example: com.baidu)

- When you want to use members of other packages, you need to import other packages

- Syntax: import package name

## JavaDoc Command

**The javaDoc command is used to generate your own API documentation**

**Relevant information includes:**

- @author author
- @version version
- @since jdk version
- @param parameter
- @return return status
- @throws Exceptions

