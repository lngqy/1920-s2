
# Nested Classes
Nested classes are classes that are within another class. Amongst Nested classes, there are static and non-static inner classes.

Nested inner classes that are non-static can only be created from an instance of the outer class. Therefore, the inner class is able
to access all the members of its outer class without explicit reference.

On the other hand, static innner classes can be instantiated without instantiating the outer class' object. Hence, a static inner class 
can only access static members of its outer class.

Consequently, inner classes can have methods that make use of the members of the outer class. 

# Variable Capture
For classes that are defined within a method, variable capturing will occur. Variable capture is when the local variables within the
local function in which the nested class is defined is being captured inside of the instance of the inner class in the heap.
Looking at the code from Recitation 6 Q2,
```
class B {
  void f() {
    int x = 0;
    class A {
      int y = 0;
      A() {
        y = x + 1;
      }
    }
    A a = new A();
  }
}
```
In this example, the variable int x is defined in the enclosing function in which the class A is defined. This means that the variable x
is a local variable, and will be captured inside every instance of A in the heap, even if not explicitly declared in A. This means that
methods in class A will be able to make use of the variable x.

## Effectively final
From java 8, local variables that are captured must be final or effectively final. In the example above, this means that x must be final
or effectively final. Since variable x is captured by A, trying to change the value of x will result in a compilation error.
```
class B {
  void f() {
    int x = 0;
    class A {
      int y = 0;
      A() {
        y = x + 1;
      }
    }
    x = 1; //results in an error
    A a = new A();
  }
}
```
Somewhere that we often see this phenomenon is in lamba expressions. Like inner classes, lambda expressions also captures local
variables, which is why when we use a lambda expression inside a method, we can reference the variables that are defined in the
enclosing method within our lambda expression. Since local variables are captured by lambda expressions, this means that those
captured variables must also be final or effectively final. For example,
```
class Test {
  int supplierAdd(int x, int y) {
    Supplier<Integer> s = () -> x + y; //variables x and y are captured by lambda
    x++; //this results in a compilation error
    return s.get()
  }
}
```
