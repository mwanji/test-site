---
title: "Understanding Java 8 Method References"
layout: post
og_description: "Method references aren't quite what they appear to be"
---

Method references are a nice facility in Java 8. They can make code very concise and expressive. However, there are a few things to understand about them: they are merely a shorthand for lambdas and they are desugared in a specific way.

**Method References Are Abbreviated Lambdas**

One fundamental thing I hadn't understood is that a method reference doesn't refer directly to the method literal. It's not a typesafe equivalent of `User.class.getMethod("getName")`, it's syntactic sugar for a lambda. You can see this in the following code:

````
class MyClass {
  public static void printFunction(Function<String, String> func) {
    System.out.println(func);
  }

  public static void main(String[] args) {
    printFunction(String::valueOf);
  }
}

````

Running this prints something like "MyClass$$Lambda$1/1510467688". This shows that method references should be considered abbreviations for common lambdas. So you can't use a method reference to reflect on a method's signature in a typesafe way, which is what I was hoping to do.

**Referring to Instance Methods**

There are four types of method references. References to static methods, references to methods of a specific instance and references to constructors are easy to understand. The explanation of the fourth type, the "Reference to an Instance Method of an Arbitrary Object of a Particular Type", provided in Oracle's [method reference tutorial](http://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html) is very uninformative. Reading it has not helped me use one myself.

Thankfully, Toby Weston [explains method references](http://baddotrobot.com/blog/2014/02/18/method-references-in-java8/) much more clearly and with nice examples.

**Desugaring Lambdas**

Another source of confusion is Oracle's example of using a method reference to an instance method of an unknown object (an "object supplied later", as Weston puts it):

````
String[] stringArray = { "Barbara", "James", "Mary", "John", "Patricia", "Robert", "Michael", "Linda" };
Arrays.sort(stringArray, String::compareToIgnoreCase);
````

How can String's compareToIgnoreCase serve as a Comparator? The answer is found in Brian Goetz's [Translation of Lambda Expressions](http://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html) from April 2012. He says that, when converting a method reference to a lambda, "if the desugared method is an instance method, the receiver is considered to be the first argument". Also, the lambda's remaining arguments are passed as arguments to the referred method.

The desugaring process can be broken down into the following steps:

* `Arrays#sort` expects a `Comparator<String>`
* `Comparator<String>`'s abstract method is `int sort(String, String)`, which is equivalent to `BiFunction<String, String, Integer>`
* The comparator instance could therefore be supplied by a `BiFunction`-compatible lambda: `(String a, String b) -> a.compareToIgnoreCase(b)`
* `String::compareToIgnoreCase` refers to an instance method that takes a String argument, so it is compatible with the above lambda: `String a` becomes the receiver and `String b` becomes the method argument

To try it for myself, I defined a TriFunction equivalent to BiFunction and a corresponding method reference:

````
interface TriFunction<T1, T2, T3, R> {
  R apply(T1 receiver, T2 argument1, T3 argument2);
}

class MyClass {

  Integer triRef(String s1, String s2) {
    return 1;
  }

  static void tri(TriFunction<MyClass, String, String, Integer> func) {}

  public static void main(String[] args) {
    tri(MyClass::triRef); // is equivalent to
    tri((MyClass myClass, String s1, String s2) -> myClass.triRef(s1, s2));
  }
}

````

When a method reference is used where a `TriFunction` is required, the referred method (`triRef` in this case) will be called on `receiver` and `argument1` and `argument2` will be passed as parameters to the referred method.
