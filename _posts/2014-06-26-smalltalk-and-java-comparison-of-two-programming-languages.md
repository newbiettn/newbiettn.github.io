---
id: 6
title: 'Smalltalk and Java: Comparison of two programming languages'
date: 2014-06-26T23:04:34+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=6
permalink: /2014/06/smalltalk-and-java-comparison-of-two-programming-languages/
categories:
  - Uncategorized
tags:
  - comparision
  - java
  - oop
  - smalltalk
---
<p style="text-align: justify;">
  <strong>1. INTRODUCTION</strong><br /> Smalltalk is the pioneer in the object-oriented programming. Many inventions in Smalltalk programming languages, such as the virtual machine, debugging by inspection, IDE, Model-View-Controller architecture, have become prevailingly adopted in other programming languages. Java was born later and influenced by Smalltalk though, the language is supplied with distinct features including interfaces, abstract classes, anonymous classes and so on. The Java language has profoundly evolved since the introduction of lambda expression and the functional interface in the latest version 1.8.
</p>

<p style="text-align: justify;">
  In this paper, we introduce the comparison of Java and Smalltalk. The discussion does not limit on direct comparison of their own distinguishable traits, but also examine how developers would utilize facilities &#8211; such as the class-instance variables in Smalltalk or the anonymous classes and virtual extension methods in Java &#8211; and the drawbacks, benefits behind of the language designs such as the collection API.
</p>

<p style="text-align: justify;">
  <strong>2. SYNTAX</strong>
</p>

<p style="text-align: justify;">
  Smalltalk offers a minimal syntax. Smalltalk syntax is mainly about message sending. Java syntax is similar to C and much more complicated compared to Smalltalk.
</p>

<p style="text-align: justify;">
  Java developers would be impressed to know that Smalltalk only has a few keywords including self, super, true, false, nil, thisContext, and local variable declara- tion (<span class="lang:default decode:true  crayon-inline ">|. . . |</span> ). Everything else (such as the assignment op- erator (<span class="lang:default decode:true  crayon-inline ">:=</span> ), the return operator (<span class="lang:default decode:true  crayon-inline ">↑</span> )) are merely syntactic sugar. For example, the return operator is equivalent to <span class="lang:default decode:true  crayon-inline ">thisContext return: anObject</span> , or <span class="lang:default decode:true  crayon-inline ">#(1 2 3)</span>  can be expressed by <span class="lang:default decode:true  crayon-inline ">Array with: 1 with 2: with 3</span> .
</p>

<p style="text-align: justify;">
  A brief comparison of syntax between Smalltalk and Java is supplied in the Table 1.
</p>

<p style="text-align: justify;">
  <strong>3. TYPES</strong>
</p>

<p style="text-align: justify;">
  Smalltalk actually does not have types. Because everything in Smalltalk is an object, new object can be created in the same manner as creating new instance of a class. Because Smalltalk has dynamic types, we can send all messages to a Smalltalk ob ject. In case the object does not understand the received message, the message <span class="lang:default decode:true  crayon-inline ">doesNotUnderstand: #message</span>  will be send to the current ob ject.
</p>

<p style="text-align: justify;">
  Smalltalk provides duck typing happening at runtime [29]. The behaviors of objects are called by sending any messages to the ob jects. Thus, if the ob ject understands the message we send to, we can assume its type even we do not know what class is involved.
</p>

<p style="text-align: justify;">
  Java type system is both static and dynamic [2]. Built-in types or primitive types, such as number or characters, are the rock bottom of the Java type system. They are fixed and not objects. On the other hand, Java data types can also behave similar to Smalltalk when new data types can be constructed as new classes, which can be dynamically loaded at runtime. While primitive types are passed by values, class types are passed by reference.
</p>

<p style="text-align: justify;">
  <strong>4. EXPRESSIONS</strong><br /> <strong> 4.1 Keyword message/method invocation</strong>
</p>

<p style="text-align: justify;">
  Smalltalk uses message sending, not method invocation. To invoke a method of an object, the programmer sends corresponding message to it with all necessary parameters. Strictly speaking, the only way to interact with objects in Smalltalk is through their sets of messages, called the interfaces. When the object receives the message, it will search its method dictionary. If the sending message is not found in its dictionary, it will continue the search in the dictionaries of its superclasses. If no matching message is found, the receiver will send to itself the <span class="lang:default decode:true  crayon-inline ">doesNotUnderstand</span>  message.
</p>

<p style="text-align: justify;">
  Java calls functions of a class by the method invocation. Java method invocation is typically like <span class="lang:default decode:true  crayon-inline ">a.bar()</span> , which is to invoke the method <span class="lang:default decode:true  crayon-inline ">bar()</span>  inside the object a.
</p>

<p style="text-align: justify;">
  From the usage point of view, Smalltalk keyword message is more useful than Java function call because the keyword message adds meanings and descriptions to both methods and arguments.
</p>

<p style="text-align: justify;">
  <strong>4.2 Message precedence</strong>
</p>

<p style="text-align: justify;">
  One of the most variation between two languages is the message precedence. For example, the calculation <span class="lang:default decode:true  crayon-inline ">1 + 2 * 3</span>  would be 9 in Smalltalk instead of 6 in Java. Programmers should be cautious in using, otherwise it may lead to inconsistent logics.
</p>

<p style="text-align: justify;">
  <strong>4.3 Message chaining/cascading</strong>
</p>

<p style="text-align: justify;">
  Both Smalltalk and Java have method chaining. However, method chaining in Smalltalk is modest because it is not comfortable for readers to follow the chaining of more than two message sendings. Java programmers do not restrict method chaining. Since Java 8 was released, method chaining has turned to be more effective in providing better readability and conciseness of the code.
</p>

<p style="text-align: justify;">
  Smalltalk has message cascading, which Java lacks. However, message cascading is not necessary in Java because Java uses function invocation rather than keyword message.
</p>

<p style="text-align: justify;">
  <strong>4.4 Object creation</strong>
</p>

<p style="text-align: justify;">
  Smalltalk allows to create new object by sending message <span class="lang:default decode:true  crayon-inline ">new</span>  to the class. Notice that <span class="lang:default decode:true  crayon-inline ">new</span>  is not a language keyword. It is indeed an ordinary method named <span class="lang:default decode:true  crayon-inline ">new</span> . However, unlike Java, Smalltalk does not have constructor. In order to initialize the new instance, we have to pass the parameters to the created objects indirectly by getter or setter messages.
</p>

<p style="text-align: justify;">
  Java new object can be created using <span class="lang:default decode:true  crayon-inline ">new</span>  keyword to return the reference to new object. The required argument to <span class="lang:default decode:true  crayon-inline ">new</span>  is class constructors. We can also pass parameters directly to the class constructors. In the Section 5.3.1, we will discuss in detail about the class constructors.
</p>

<p style="text-align: justify;">
  <strong>4.5 Self/this</strong>
</p>

<p style="text-align: justify;">
  Both keywords self in Smalltalk and this in Java are similar. However, this is usually omitted in Java when calling the functions of this. For example, calling <span class="lang:default decode:true  crayon-inline ">this.foo()</span>  is the same to <span class="lang:default decode:true  crayon-inline ">foo()</span> . It is different in Smalltalk when we cannot abbreviate self from self foo.
</p>

<p style="text-align: justify;">
  <strong>4.6 Instance variables</strong>
</p>

<p style="text-align: justify;">
  Smalltalk instance variables cannot be accessed from outside of an instance. In the words, objects need to use accessing messages in order to access or modify instance variables of other instances of the same class. Compared to Java, instance variables in Smalltalk are Java protected variables since developers are able to access them directly by name inside instance methods of their defined classes as well as the subclasses.
</p>

<p style="text-align: justify;">
  Instance variables in Java can be retrieved straightforwardly using <span class="lang:default decode:true  crayon-inline ">(.)</span>  operator. It makes the classes become the encapsulation boundary of Java, while in Smalltalk we have the instance.
</p>

<p style="text-align: justify;">
  <strong>4.7 Pragmas/annotations</strong>
</p>

<p style="text-align: justify;">
  As said, everything in Smalltalk is an object, except some primitives. When browsing the message <span class="lang:default decode:true  crayon-inline ">whatIsAPrimitive</span>  in the <span class="lang:default decode:true  crayon-inline ">Object</span>  class, one might get some information &#8220;A primitive response is performed directly by the interpreter rather than by evaluating expressions in a method. The methods for these messages indicate the presence of a primitive response by including <span class="lang:default decode:true  crayon-inline ">⟨primitive: xx⟩</span>  before the first expression in the method&#8221;. So, what is <span class="lang:default decode:true  crayon-inline">⟨primitive: xx⟩</span> ?
</p>

<p style="text-align: justify;">
  What we have seen is not a regular message in Smalltalk. It is known as the pragmas. In a nutshell, the pragmas are used to tag the methods in Smalltalk by carrying the metadata of the methods.
</p>

<p style="text-align: justify;">
  Take a look at the body of the method basicAt in the Object class.
</p>

<pre class="lang:default decode:true ">basicAt: index
&lt;primitive: 60&gt;
index isInteger ifTrue: [self

errorSubscriptBounds: index]. index isNumber

ifTrue: [^self basicAt: index asInteger]

ifFalse: [self errorNonIntegerIndex]</pre>

<p style="text-align: justify;">
  The method <span class="lang:default decode:true  crayon-inline ">basicAt</span>  is a primitive, which is invoked by the pragma <span class="lang:default decode:true  crayon-inline ">⟨primitive: 60⟩</span> . In case the pragma fails, the following Smalltalk code will be evaluated.
</p>

<p style="text-align: justify;">
  Smalltalk pragmas share common characteristics with Java annotations. Both of them are to provide metadata to the compiler, and sometimes instruct the compiler to generate comments, codes, etc. However, there are two major differences between them. Firstly, annotations can be used at runtime when pragmas cannot. Secondly, Java developers are allowed to define their own annotations whereas Smalltalk developers cannot.
</p>

<p style="text-align: justify;">
  <strong>5. OBJECT MODEL</strong>
</p>

<p style="text-align: justify;">
  <strong>5.1 Classes</strong>
</p>

<p style="text-align: justify;">
  Classes in both Smalltalk and Java are blueprints used to manufacture objects. The class has its own methods and variables, which should be distinguished from methods and variables of instances. In detail, a class can contains a set of information which is distinctly compared in the Table 2. There are important points need to be remarked:
</p>

<ul style="text-align: justify;">
  <li>
    Both languages offer a set of variable names to their class instances. The instance variables, however, are private to every instance.
  </li>
  <li>
    Class variables in Smalltalk are equivalent to static field variables in Java.
  </li>
  <li>
    There is no class-instance variable in Java. This unique feature results in different inheritance implementations between Java and Smalltalk, which we will discuss in the Section 6.
  </li>
  <li>
    Java offers the class constructor, which Smalltalk lacks.
  </li>
</ul>

<p style="text-align: justify;">
  <strong>5.1.1 Abstract class</strong>
</p>

<p style="text-align: justify;">
  In Java, an abstract class and interface share similarities, such as requiring subclasses to implement methods, they still have differences:
</p>

<ul style="text-align: justify;">
  <li>
    Abstract classes can define concrete methods, whereas interfaces cannot. In Java 8, by using virtual extension methods, interfaces are allowed to define concrete methods like abstract classes. We will discuss about virtual extension methods of Java in the Section 6.3.2.
  </li>
  <li>
    Abstract classes can declare non-public methods, interfaces cannot.
  </li>
  <li>
    Abstract classes are meant to be inherited while interfaces can be implemented multiple times.
  </li>
</ul>

<p style="text-align: justify;">
  Smalltalk does not dedicate any particular syntax for abstract classes like Java. If one of methods in classes contains the expression self <span class="lang:default decode:true  crayon-inline ">subclassResponsibility</span> , the class is considered as an abstract class. The marker self <span class="lang:default decode:true  crayon-inline ">subclassResponsibility</span>  indicates subclasses have responsibility of defining its own method.When compare abstract classes in Smalltalk and abstract classes in Java, they are similar in conceptual model, except that all methods in Smalltalk abstract class must be public. In case all methods in abstract classes are abstract, the abstract classes will be similar to Java interfaces, but note that interfaces cannot bed declared with field variables like Smalltalk abstract classes.
</p>

<p style="text-align: justify;">
  <strong>5.2 Methods</strong>
</p>

<p style="text-align: justify;">
  Both Smalltalk and Java define methods in the class body. In the methods, programmers can declare local variables and expressions to be executed when the methods are called. However, there are some differences between two languages:
</p>

<ul style="text-align: justify;">
  <li>
    Smalltalk methods do not have return types whereas Java requires developers to declare specific return types for methods. Return types of Java methods consist of primitive types, reference types or no returned value void.
  </li>
  <li>
    Statements in Smalltalk method are divided by dots (.) whereas in Java the semicolon (;) is used to end a statement.
  </li>
  <li>
    Smalltalk methods are considered as public methods in Java because they are completely exposed. One minor difference is Smalltalk uses protocol to group similar methods.
  </li>
  <li>
    Both languages offer method overloading or ad-hoc polymorphism. While Java resolves overloaded meth- ods during compilation [11], method overloading hap- pens at runtime in Smalltalk [28].
  </li>
</ul>

<p style="text-align: justify;">
  <strong>5.3 Objects</strong>
</p>

<p style="text-align: justify;">
  Smalltalk is pure object oriented programming language [30]. Everything in Smalltalk is an object [3]. In contrast, besides reference data types, Java has primitive types, which are predefined by the language and do not as- sociate with any classes [22].
</p>

<p style="text-align: justify;">
  Objects in Smalltalk and Java are derived from the highest superclass called <span class="lang:default decode:true  crayon-inline ">Object</span>  class [20] [4]. To instantiate a new <span class="lang:default decode:true  crayon-inline ">Foo</span>  object in Smalltalk, we could use Java-like expression such as <span class="lang:default decode:true  crayon-inline ">foo := Foo new</span> .
</p>

<p style="text-align: justify;">
  What will happen when we use class message new here, even though, there is no such new method in the code? The answer is there exists a method lookup behind the scenes when we send message new to <span class="lang:default decode:true  crayon-inline ">Foo</span>  class. The lookup then starts from <span class="lang:default decode:true  crayon-inline ">Foo</span>  and goes up to the root class <span class="lang:default decode:true  crayon-inline ">Object</span> .
</p>

<p style="text-align: justify;">
  <strong>5.3.1 Constructors</strong>
</p>

<p style="text-align: justify;">
  Both Smalltalk and Java use new keyword to allocate memory for new instances. In Java, whenever the new operator is called, the class constructor will be invoked. Java class constructor uses the same name with the class and has no return type [23]. Because class constructor accepts arguments, Java developers use it to initialize the new objects.
</p>

<p style="text-align: justify;">
  For each class, there is always a default constructor. The default constructor will be invoked once we call the new operator without supplied arguments. Because the constructor can be overloaded, one might define a custom constructor for a class. However, Java class constructor cannot be inherited.
</p>

<p style="text-align: justify;">
  Unlike Java, Smalltalk does not support class constructor. Whenever we send the message new to the class, we simply tell Smalltalk allocates space for the instance variables of the object, then set their values to nil. To make the initialization meaningful and accomplished, we need to pass values to the variables in following expressions. For example:
</p>

<pre class="lang:default decode:true ">p := Person new.
p name: ’Ngoc Tran’ address: ’Brussels’</pre>

<p style="text-align: justify;">
  This expression works exactly as we want but it is untidy. [1] proposes to use <em>Constructor Parameter Method</em>, which makes our code become:
</p>

<pre class="lang:default decode:true ">p := Person name: ’Ngoc Tran’ address: ’Brussels’</pre>

<p style="text-align: justify;">
  The code now becomes more clear and succinct. We firstly need to define a class method <span class="lang:default decode:true  crayon-inline ">name: #aName address: #anAddress</span>  for the <span class="lang:default decode:true  crayon-inline ">Person</span>  class. Inside the method, we send the message new to super. The pseudo-variable super here indicates the receiver to start method lookup from the parent of <span class="lang:default decode:true  crayon-inline ">Person</span>  class. When the method is found, a <span class="lang:default decode:true  crayon-inline ">Person</span>  object is created, which later will receive a set of messages initialize, <span class="lang:default decode:true  crayon-inline ">name: </span> and <span class="lang:default decode:true  crayon-inline ">address:</span> . Java developers may not familiar with how the method is defined but in Smalltalk, we can &#8220;cascade&#8221; side-effect calls to the same object.
</p>

<pre class="lang:default decode:true ">name: aName address: anAddress ^super new
initialize;name: aName; address: anAddress</pre>

<p style="text-align: justify;">
  Similar to new, <span class="lang:default decode:true  crayon-inline ">name: #aName address: #anAddress</span>  is a class method, and it can only sent to <span class="lang:default decode:true  crayon-inline ">Person</span>  class. It is specialized in Smalltalk when the class can only receive messages from the class methods, and in contrast, instances only take instance messages and variables. To read and write instance variables inside the body of the class method, we have to use messages.
</p>

<p style="text-align: justify;">
  When compare Smalltalk and Java on this feature, we see that Java is much more convenient than Smalltalk. Class constructor not only provides a more extensive meaning for instance instantiation but also helps developers make sure instance variables fully initialized before using.
</p>

<p style="text-align: justify;">
  <strong>5.3.2 Is single responsibility principle violated in Smalltalk?</strong>
</p>

<p style="text-align: justify;">
  When browsing source code in Pharo, one might see that <span class="lang:default decode:true  crayon-inline ">Object</span>  class contains 339 methods, which is overwhelming when compare to Java. Take into consideration the principle of single responsibility, which states &#8220;<em>a class should have only one reason to change&#8221;</em> [13], with hundreds of methods inside the class <span class="lang:default decode:true  crayon-inline ">Object</span> , does Smalltalk violate the principle? I am unqualified to answer the question but there is a few comments. Firstly, the <span class="lang:default decode:true  crayon-inline ">Object</span>  class perhaps does not handle as much responsibility as it seems to be because many methods in the class uses double dispatch. Secondly, because Smalltalk uses traditional mirror reflective design, it could be reasonable to put reflection methods, such as inspect, inside the class body.
</p>

<p style="text-align: justify;">
  <strong>6. RELATIONSHIPS AMONG CLASSES</strong>
</p>

<p style="text-align: justify;">
  <strong>6.1 Inheritance</strong>
</p>

<p style="text-align: justify;">
  Both Smalltalk and Java feature single-inheritance in order to enhance reusability and extensibility of code. It allows classes to inherit from its superclasses, including both variables and methods. The subclasses can also add new methods or override inherited methods.
</p>

<p style="text-align: justify;">
  <strong>6.1.1 Class side inheritance</strong>
</p>

<p style="text-align: justify;">
  The most difference between Smalltalk and Java is Smalltalk supports class side inheritance whereas Java does not. For example, if the class <span class="lang:default decode:true  crayon-inline ">Foo</span>  inherits from the class <span class="lang:default decode:true  crayon-inline ">Bar</span> , and <span class="lang:default decode:true  crayon-inline ">foo</span>  and <span class="lang:default decode:true  crayon-inline ">bar</span>  are instances of classes <span class="lang:default decode:true  crayon-inline ">Foo</span>  and <span class="lang:default decode:true  crayon-inline ">Bar</span>  respectively, then <span class="lang:default decode:true  crayon-inline ">foo</span>  class will inherit from <span class="lang:default decode:true  crayon-inline ">bar</span>  class as a result. It would not happen in Java. The method call <span class="lang:default decode:true  crayon-inline ">getClass()</span>  in instances <span class="lang:default decode:true  crayon-inline ">foo</span>  and <span class="lang:default decode:true  crayon-inline ">bar</span>  in Java will return instances of the class <span class="lang:default decode:true  crayon-inline ">Class</span> , which have no relationships to each other.
</p>

<p style="text-align: justify;">
  <strong>6.1.2 Class instance variable inheritance</strong>
</p>

<p style="text-align: justify;">
  As said in the Section 5, class instance variables only exist in Smalltalk. Class instance variables are private to the class itself, and will be inherited by subclasses.
</p>

<p style="text-align: justify;">
  Suppose we have the class Database which handles the database layer. Because the application use various database sources (such as JSON, SQL), it is necessary to define subclasses that inherit from Database class but also maintain only one single instance of Database class in the whole application. In the class side of Database class we define some class instance variables. However, for the sake of simplicity, we keep it simple as follows:
</p>

<pre class="lang:default decode:true ">Database class instanceVariableNames: ’dbName’</pre>

<p style="text-align: justify;">
  We have seen that the Database class has a class instance variable <span class="lang:default decode:true  crayon-inline ">dbName</span> , which should be inherited by subclasses such as <span class="lang:default decode:true  crayon-inline ">SQLDatabase</span> , <span class="lang:default decode:true  crayon-inline ">JSONDatabase</span> . Here we define a method <span class="lang:default decode:true  crayon-inline ">dbName</span>  to get the name of the database
</p>

<pre class="lang:default decode:true">Database class &gt;&gt;dbName ^self dbName</pre>

<p style="text-align: justify;">
  Note that the method <span class="lang:default decode:true  crayon-inline ">dbName</span>  will be inherited by subclasses. We indeed do not need to use keyword <span class="lang:default decode:true  crayon-inline ">self</span> , but in order to illustrate that the class which invokes the message will return its own class instance variables dbName. Therefore, if the <span class="lang:default decode:true  crayon-inline ">SQLDatabase</span>  class invokes the <span class="lang:default decode:true  crayon-inline ">dbName</span>  method, the class instance variable <span class="lang:default decode:true  crayon-inline ">dbName</span>  will be returned, which is different from <span class="lang:default decode:true  crayon-inline ">dbName</span>  variable of <span class="lang:default decode:true  crayon-inline ">Database</span>  class.
</p>

<p style="text-align: justify;">
  <strong>6.1.3 Discussion</strong><br /> Both Smalltalk and Java have single inheritance though, they have uncommon attributes including class side inheritance and class-instance variable inheritance. These differences origin from the basic characteristic of Smalltalk &#8211; everything is an object. It implies that class is an object, and should have its own variables and inheritance.
</p>

<p style="text-align: justify;">
  <strong>6.2 Multiple inheritance</strong>
</p>

<p style="text-align: justify;">
  To compensate for the lack of multiple inheritance, Java classes can implement multiple interfaces while Smalltalk classes can reuse protocols of other classes by traits. We will discuss about traits and interfaces in the Section 6.3.
</p>

<p style="text-align: justify;">
  <strong>6.3 Interfaces/Traits</strong>
</p>

<p style="text-align: justify;">
  <strong>6.3.1 Interfaces in Java</strong>
</p>

<p style="text-align: justify;">
  Interfaces are the unique feature of Java, which is defined as a set of methods the classes need to implements. The purpose of introducing interfaces in Java is greater since it allows an object to escape from the single inheritance. One, for example, can use composition to simulate multiple inheritances, which do not natively exist in Java.
</p>

<p style="text-align: justify;">
  Java interfaces can be used to establish data types. Suppose we have a class <span class="lang:default decode:true  crayon-inline ">Person</span>  implementing two interfaces <span class="lang:default decode:true  crayon-inline ">IWorker</span>  and <span class="lang:default decode:true  crayon-inline ">IHusband</span> . At work, a <span class="lang:default decode:true  crayon-inline ">Person</span>  instance only care about implementing methods according to <span class="lang:default decode:true  crayon-inline ">IWorker</span>  interface. Being a husband, implementing methods according to <span class="lang:default decode:true  crayon-inline ">IHusband</span>  is more important. Therefore, the type is different depending on different situations.
</p>

<p style="text-align: justify;">
  <strong>6.3.2 Virtual Extension Methods in Java 8</strong>
</p>

<p style="text-align: justify;">
  Along with lambda expression, Java 8 introduces <em>default methods</em> (defender methods) or<em> virtual extension methods</em>. The purpose is to provide the backward compatibility in the new Java [16]. For example, we have an interface <span class="lang:default decode:true  crayon-inline ">A</span>  and a class <span class="lang:default decode:true  crayon-inline ">Clazz</span>  implements the interface:
</p>

<pre class="lang:java decode:true">public interface A {
void foo();
}

public class Clazz implements A{

@Override
public void foo() {} }</pre>

<p style="text-align: justify;">
  After that we add the new method bar() to the interface A. Now the interface A will look like:
</p>

<pre class="lang:java decode:true">public void void

}

interface A { foo();
bar();</pre>

<p style="text-align: justify;">
  While the class <span class="lang:default decode:true  crayon-inline ">Clazz</span>  implements the interface <span class="lang:default decode:true  crayon-inline ">A</span> , the interface evolves, which results in the compilation error of the class. The virtual extension methods provide the new facility to solve the problem:
</p>

<div class="column">
  <pre class="lang:java decode:true ">public interface A { void foo();

default void bar() {} }</pre>

  <p style="text-align: justify;">
    The class <span class="lang:default decode:true  crayon-inline ">Clazz</span>  no longer has to worry about new method <span class="lang:default decode:true  crayon-inline ">bar()</span>  because it is provided by the interface.
  </p>

  <p style="text-align: justify;">
    <strong>6.3.3 Traits in Smalltalk</strong>
  </p>

  <p style="text-align: justify;">
    Smalltalk supports trait implementation. In a nutshell, traits allow the developer to reuse a set of methods in order to extend the functionality of the classes ??. We can also use traits to avoid complicated inheritance. In Smalltalk, due to the limitation of single inheritance, traits of different collection properties is created to build diverse hierarchy of classes without code duplication [25].
  </p>

  <p style="text-align: justify;">
    <strong>6.3.4 Discussion</strong>
  </p>

  <p style="text-align: justify;">
    In general, the combination of interfaces and virtual extension methods in Java are fairly similar to traits in Smalltalk from the usage point of view. Both of them provide multiple inheritances. However, in terms of the language structure, interfaces are necessary in Java but not in Smalltalk. Java variables whose types are interface can reference to any objects that implement the interface. This feature is particularly helpful since Java is statically typed. But because Smalltalk is dynamic typed, interfaces consequently do not serve any purposes.
  </p>

  <p style="text-align: justify;">
    In Java 7 and previous versions, sometimes it is annoying to implement all methods in an interface whereas in Smalltalk, you can create default implementations for methods and let classes reimplement only required methods. With the support of virtual extension methods, Java has a significant improvement in the language, allowing interfaces behave conceptually similar to traits.
  </p>

  <p style="text-align: justify;">
    <strong>6.4 Encapsulation</strong>
  </p>

  <p style="text-align: justify;">
    There is no access modifiers in Smalltalk whereas Java obtains a number of modifiers including public, private, protected for both variables and methods .
  </p>

  <p style="text-align: justify;">
    In detail, Smalltalk methods or messages are always considered to be public properties of an object in Smalltalk, or we can say a set of messages of an object is the object interface. Variables in Smalltalk, on the other hand, are the private properties of an object. The only way to access the instance variables inside Smalltalk object is to send the message to the object. The Smalltalk instance variables can be compared to protected instance variables of Java.
  </p>

  <p style="text-align: justify;">
    In Java, we can have both private and public methods inside a single method body in which private methods are only invoked within the containing class while public methods can be used anywhere.
  </p>

  <p style="text-align: justify;">
    Finally, the access modifier protected is unique to Java itself.
  </p>

  <p style="text-align: justify;">
    <strong>6.5 Polymorphism</strong>
  </p>

  <p style="text-align: justify;">
    Both languages support polymorphism. There are no differences in their approach. Smalltalk allows sending messages having the same names to inherited object, while Java instances of classes in the same hierarchy can invoke methods of the same name but different implementations.There is an discussion about how these languages achieve polymorphism [7]. The idea is reflection languages like Smalltalk and Java do not approach polymorphism by inheritance. Instead, they use reflection.
  </p>

  <p style="text-align: justify;">
    <strong>6.6 Inner classes</strong>
  </p>

  <p style="text-align: justify;">
    Inner classes in Java are classes defined inside the body of another classes or methods. By using inner classes, we can create new instance variables of new classes aesthetically and dynamically whenever we need it. For example, let’s have a NonStaticBar and StaticBar class nested inside the Foo class:
  </p>

  <pre class="lang:java decode:true ">Class Foo {
Class NonStaticBar {}; static class StaticBar {}; void doSomething () {};

}</pre>

  <p style="text-align: justify;">
    Within <span class="lang:default decode:true  crayon-inline ">Foo</span> , one can create new <span class="lang:default decode:true  crayon-inline ">NonStaticBar</span>  objects or call <span class="lang:default decode:true  crayon-inline ">doSomething()</span>  but outside <span class="lang:default decode:true  crayon-inline ">Foo</span>  scope, both <span class="lang:default decode:true  crayon-inline ">NonStaticBar</span>  and <span class="lang:default decode:true  crayon-inline ">doSomething()</span>  are not accessible without static keyword. In other words, <span class="lang:default decode:true  crayon-inline ">NonStaticBar</span>  objects are always enclosed by instances of the <span class="lang:default decode:true  crayon-inline ">Foo</span>  class. On the other hand, we can use <span class="lang:default decode:true  crayon-inline ">StaticBar</span>  class as a top-level class regardless its enclosing instances.
  </p>

  <p style="text-align: justify;">
    <strong>6.7 Closures</strong>
  </p>

  <p style="text-align: justify;">
    <strong>6.7.1 Anonymous inner class</strong>
  </p>

  <p style="text-align: justify;">
    Besides classes that are nested inside classes or methods, Java support anonymous inner class. Using anonymous in- ner class, one can declare the class and instantiate new instance of that class at the same time as follows:
  </p>

  <pre class="lang:java decode:true ">final int number = 0; AnonymousInnerClass aic = new

AnonymousInnerClass() { public void read () {

System.out.println("number :" +

number); }

}</pre>

  <p style="text-align: justify;">
    In our case, the compiler automatically generates the constructor of anonymous class <span class="lang:default decode:true  crayon-inline ">AnonymousInnerClass</span>  and passes the variable number with final modifier to the constructor. The reason for using final is to make sure all instances of <span class="lang:default decode:true  crayon-inline ">AnonymousInnerClass</span>  class have a reference of local variable number in case the garbage collection happens.
  </p>

  <p style="text-align: justify;">
    Anonymous objects in Java can be used to model closures as we may see in Smalltalk, however, the most frustrating problem is that we cannot access variable outside the scope of anonymous objects without final keyword. Going back in time, Java actually supported the modification of out-of-scope variables without the constraint of using <span class="lang:default decode:true  crayon-inline ">final</span>  keyword. The Java language itself had no issue with referenced objects with this principle but there was memory problem regarding to garbage collection when it came to primitive types. As default, Java allocates primitive variables on stack but later the language allocated them into heap in order for the change. After many debates happened due to the issue of garbage collection, Java decided to add <span class="lang:default decode:true  crayon-inline ">final</span>  keyword [12].
  </p>

  <p style="text-align: justify;">
    Another problem with Java anonymous inner classes is type checking. When one creates an anonymous class, he is required to implement an interface or extend a class. This obligation is to guarantee type safety is preserved at compile time.
  </p>

  <p style="text-align: justify;">
    However, Java inner classes are not beautiful when we inspect what happens behind the scenes. Inner classes are pure syntactic sugar [15]. It implies that Java virtual machine does not support inner classes; instead compiler does a good trick when mapping inner classes to top-level classes with a dollar sign. In our case, we would have two binary classes <span class="lang:default decode:true  crayon-inline ">Foo.class</span>  and <span class="lang:default decode:true  crayon-inline ">FooBar.class</span>  generated by the compiler.
  </p>

  <p style="text-align: justify;">
    <strong>6.7.2 Lambda expression in Java 8</strong>
  </p>

  <p style="text-align: justify;">
    Java 8 introduces lambda expression as the new language feature. For example, we have a list of integers:
  </p>

  <pre class="lang:java decode:true ">List&lt;Integer&gt; numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

One might loop over the list by using for statement in Java 7 as follows:

for (Integer number: numbers) { System.out.println(number);

}</pre>

  <p style="text-align: justify;">
    In the code snippet, the list is looped externally when every item in the list is pulled out for further implementation. It is apparent but the control flow might be limited in case we need to process the list in parallel.
  </p>

  <p style="text-align: justify;">
    Alternatively, one might use <span class="lang:default decode:true  crayon-inline ">forEach()</span>  to internally loop over the list with anonymous class. For example:
  </p>

  <pre class="lang:default decode:true ">numbers.forEach(new Consumer &lt;Integer &gt;() {

@Override

public void accept(Integer value) { System.out.println(value);

} });</pre>

  <p style="text-align: justify;">
    Java 8 has the Consumer functional interface and <span class="lang:default decode:true  crayon-inline ">forEach()</span> . We will not discuss much about the <span class="lang:default decode:true  crayon-inline ">Consumer</span>  interface but rather focus on the lambda expression. Using lambda expression, we can make the code above to be more concise.
  </p>

  <pre class="lang:java decode:true ">numbers.forEach((Integer value)
-&gt; System.out.println(value));</pre>

  <p style="text-align: justify;">
    As we can see, lambda expressions are fairly similar to anonymous objects with one method in terms of expressiveness. However, lambda expressions are not syntactic sugar of anonymous inner classes because JVM treats lambda expression more than a regular class [14].
  </p>

  <p style="text-align: justify;">
    <strong>6.7.3 First-class function in Java 7</strong>
  </p>

  <p style="text-align: justify;">
    In Smalltalk, blocks are first-class but Java 7 and previous versions do not have any thing similar. Java requires developers put functions within the classes, even in case the classes merely contain static functions. However, there are two schemes by which we can use to simulate first-class functions in Java to a certain degree &#8211;<em> anonymous inner class</em> and <em>Java core reflection</em>.
  </p>

  <p style="text-align: justify;">
    Java core reflection can partly illustrate how first-class functions can be demonstrated by Java language. One can use reflection package of Java to write an example as:
  </p>

  <pre class="lang:java decode:true ">Class c = foo.getClass();
Method m = c.getMethod("saySomething"); // saySomething is a static method, we ignore the first parameter

static methods it is ignored String s = (String)m.invoke(null);</pre>

  <p style="text-align: justify;">
    What we have seen is to use Java reflection API to assign the static <span class="lang:default decode:true  crayon-inline ">saySomething</span>  method to a <span class="lang:default decode:true  crayon-inline ">Method</span>  object and invoke it by using the new object. However, neither anonymous inner class nor reflection API can fully make Java functions as first-class because the drawbacks outweigh the advantages they provide.
  </p>

  <div class="page" title="Page 7">
    <div class="layoutArea">
      <div class="column">
        <p style="text-align: justify;">
          <strong>6.7.4 First-class function in Java 8</strong>
        </p>

        <p style="text-align: justify;">
          However, in Java 8, we no longer need anonymous inner classes and reflection to simulate functional-style operations. The latest Java introduces interfaces by which developers can use to create higher-order functions like Smalltalk. For example, one can use <span class="lang:default decode:true  crayon-inline ">filter()</span>  in package <span class="lang:default decode:true  crayon-inline ">java.util.stream</span>  to select numbers larger than 5 in a list by passing the predicate in form of a lambda expression.
        </p>

        <pre class="lang:java decode:true ">Stream s = numbers.stream()
.filter(x -&gt; x &gt; 5);</pre>

        <p style="text-align: justify;">
          <strong>6.7.5 Smalltalk block</strong>
        </p>

        <p style="text-align: justify;">
          Inner class does not exist in Smalltalk because Smalltalk posses more handy mechanism called block. Smalltalk blocks are first-class [26]. We can store blocks in variables, pass them around, send message to them and return as the results of the functions. In some cases, even we can pass more than one block to a method. For example, one can define a block and later pass this block as an argument into another block:
        </p>

        <pre class="lang:default decode:true ">foo := [:x :y | x + y ]
bar := [:x :y :theblock | theblock value:x value: y].</pre>

        <p style="text-align: justify;">
          Each block in Smalltalk is a full closure and also an object because every block is an instance of the Block class. Thus, we can always refer to the environment recorded by the block object as long as the block object exists.
        </p>

        <p style="text-align: justify;">
          Smalltalk block is equivalent to the lambda expression. As a lambda expression, we can have arguments for a block. In the block, we can also have multiple expressions, however, only the last expression will be returned when the block is executed. It is important to know that Smalltalk block does not have name and type.
        </p>

        <p style="text-align: justify;">
          <strong>7. REFLECTION</strong>
        </p>

        <p style="text-align: justify;">
          Both Smalltalk and Java have reflective architectures. In a nutshell, reflection is &#8220;<em>the ability of a program to manipu- late as data something representing the state of the program during its own execution&#8221;</em> [8].
        </p>

        <p style="text-align: justify;">
          Table 3 summarizes the reflection facilities of Java core reflection, Java debug interface and Smalltalk [6].
        </p>

        <p style="text-align: justify;">
          <strong>7.1 Java core reflection and JDI</strong>
        </p>

        <p style="text-align: justify;">
          The reflection facility in Java is weak. The language provides two APIs for meta-programming &#8211; <em>Java core reflection</em> and the <em>Java Debugger Interface (JDI)</em>.
        </p>

        <p style="text-align: justify;">
          <strong>7.1.1 Java core reflection</strong>
        </p>

        <p style="text-align: justify;">
          It is a traditional reflective API [6], in which every class in Java is an instance of <span class="lang:default decode:true  crayon-inline ">java.lang.Class</span> . Class provides facility for introspection, which allows us to inspect the properties of a class. One, for example, can use <span class="lang:default decode:true  crayon-inline ">Class.getSuperclass()</span>  to get the superclass of a class; or <span class="lang:default decode:true  crayon-inline ">Class. getDeclaredField()</span>  to get the declare fields of the class.
        </p>

        <p style="text-align: justify;">
          <strong>7.1.2 Java Debug Interface (JDI)</strong>
        </p>

        <p style="text-align: justify;">
          JDI is a mirror-based reflective API [6], which consists a set of methods for examining remote virtual machine [21].
        </p>

        <p style="text-align: justify;">
          <strong>7.2 Smalltalk reflection</strong>
        </p>

        <p style="text-align: justify;">
          Smalltalk is a reflective programming language and &#8220;<em>has one of the most complete sets of reflective facilities&#8221;</em> [24]. This capability allows Smalltalk programs examine and modify their runtime stacks and redefine their methods. As said, we can only access instance variables by the instance accessors. However, Smalltalk allows us to inspect the object, modify its instances and send message to it. In the example, we firstly use the message class to ask the class of declared instance student. Next, we use the message <span class="lang:default decode:true  crayon-inline ">isKindOf: aClass</span>  to verify if student is an instance of the classes <span class="lang:default decode:true  crayon-inline ">Student</span> , <span class="lang:default decode:true  crayon-inline ">Person</span>  and <span class="lang:default decode:true  crayon-inline ">Float</span> .
        </p>

        <pre class="lang:default decode:true ">student Student name: ’Ngoc Tran’ address: ’Brussels’.
student class. Student
student isKindOf: Student. true student isKindOf: Person. true student isKindOf: Float. false

</pre>

        <p style="text-align: justify;">
          In addition to inspecting objects and classes, Smalltalk intersection provides the capability of modifying objects and meta-objects at runtime, or adding and removing methods. For example, suppose we create a class whose instances act as proxies for other objects. Any messages sent to the real receiver have to pass through the proxy instance, in which necessary implementations, e.g., logging, will be executed before messages are delegated.
        </p>

        <p style="text-align: justify;">
          In practice, reflection provides a framework to develop programming tools such as debuggers, or to change the pro- gramming paradigm.
        </p>

        <p style="text-align: justify;">
          Classes in Smalltalk and Java are blueprint used to produce objects, but every class is an also object, and has its corresponding class. However, what is the class of class? The answer is metaclass, which holds all methods of its class and will be created automatically behind the scenes whenever a class is created. For example, the metaclass of the class Foo is Foo class. Inheritance is also possible in metaclass. It is significantly important because all methods in super metaclass will be inherited by sub-metaclass. More interestingly, if we want to find the super class of the <span class="lang:default decode:true  crayon-inline ">Object</span>  class, we will get an endless query.
        </p>

        <p style="text-align: justify;">
          Indeed, metaclasses is difficult to grasp the whole concept but programmers do not have to understand it to use Smalltalk. The parallel of metaclass hierarchy and the class hierarchy seems in Smalltalk to be confusing but in real world, it helps creating elegant applications [27].<br /> <strong> 8. GENERICS</strong>
        </p>

        <p style="text-align: justify;">
          Generics are one of the most crucial updates in Java 5, and have thoroughly affected Java developers ever since [11]. Generics allow developers to create abstract types without troublesome type casting [18]. In fact, the compiler is in charge of type checking and casting. It assures that type safety never fails.
        </p>

        <p style="text-align: justify;">
          Smalltalk support generics in a different manner compared to Java. In essence, all programming in Smalltalk is generic due to two properties of the language. Firstly, Smalltalk is dynamically typed. Secondly, everything in Smalltalk is object, which operates based on methods defined in classes. These two properties imply that messages in Smalltalk can be applied with different types of param- eters. In other words, programming in Smalltalk is about generic programming.
        </p>

        <p style="text-align: justify;">
          Smalltalk uses double dispatch technique to deal with generics [9]. For example, the plus operator (+) is defined in the Integer class as follows:
        </p>

        <pre class="lang:default decode:true ">Integer &gt;&gt;+ aNumber
"Answer a Number which is the sum of the receiver and aNumber"
^aNumber addToInteger: self</pre>

        <p style="text-align: justify;">
          Inside the method (+), the message addToInteger is sent to the receiver of the method +, #aNumber. The purpose is to dispatch the function call (+) to the function addtToInteger, letting aNumber handle the plus operator instead of using multiple if-and-else. To assure that the message <span class="lang:default decode:true  crayon-inline ">addToInteger</span>  can be understood by all instances of the class Number, every subclasses of <span class="lang:default decode:true  crayon-inline ">Number</span>  has to implement the method.
        </p>

        <p style="text-align: justify;">
          Compared Java and Smalltalk in terms of generic programming, Smalltalk is more powerful. However, because type check only happens at runtime and the compiler provides no guarantees of type safety, we cannot assure correctness and robustness of the program [10]. In contrast, by using parameterized types, Java developers can create generic data structure whose types can be verified by the compiler.
        </p>

        <p style="text-align: justify;">
          <strong>9. CORE UTILITIES</strong>
        </p>

        <p style="text-align: justify;">
          <strong>9.1 Collections</strong>
        </p>

        <p style="text-align: justify;">
          Collections are used to represent a group of objects [19]. In both languages Smalltalk and Java, the collection API lies at the heart of the language.
        </p>

        <p style="text-align: justify;">
          In Smalltalk, the Collection class includes several subclasses: <span class="lang:default decode:true  crayon-inline ">Bag</span> , <span class="lang:default decode:true  crayon-inline ">OrderedCollection</span> , <span class="lang:default decode:true  crayon-inline ">SortedCollection</span> , <span class="lang:default decode:true  crayon-inline ">Array</span> , <span class="lang:default decode:true  crayon-inline ">String</span> , <span class="lang:default decode:true  crayon-inline ">Symbol</span> , <span class="lang:default decode:true  crayon-inline ">Set</span>  and <span class="lang:default decode:true  crayon-inline ">Dictionary</span> . In Java, the collection API is packed in <span class="lang:default decode:true  crayon-inline ">java.util</span>  package, including two largest sub-interfaces <span class="lang:default decode:true  crayon-inline ">Collection</span>  and <span class="lang:default decode:true  crayon-inline ">Map</span> . Under the <span class="lang:default decode:true  crayon-inline ">Collection</span>  interfaces we have <span class="lang:default decode:true  crayon-inline ">Set</span> , <span class="lang:default decode:true  crayon-inline ">List</span> , <span class="lang:default decode:true  crayon-inline ">Queue</span> .
        </p>

        <p style="text-align: justify;">
          In particular, we notice a variation in the collection hierarchies between two languages. In Smalltalk, the <span class="lang:default decode:true  crayon-inline ">Dictionary</span>  class extends the <span class="lang:default decode:true  crayon-inline ">Collection</span>  class, while in Java, Map is outside of the <span class="lang:default decode:true  crayon-inline ">Collection</span>  interface. According to [17], a map with key, values is not a collection semantically, and should not be put in the same interface.
        </p>

        <p style="text-align: justify;">
          The difference in conceptual model of the collection API results in different implementation. Firstly, Smalltalk <span class="lang:default decode:true  crayon-inline ">Dictionary</span>  can reuse predefined functionalities in its superclass, while Java needs to re-define the whole interface, which is more expensive.
        </p>

        <p style="text-align: justify;">
          As said, we discuss the differences in the model of the collections of two languages, and following consequences. However, it is subjective to conclude either Java or Small is better on this feature.
        </p>

        <p style="text-align: justify;">
          <strong>10. DEVELOPMENT ENVIRONMENT</strong>
        </p>

        <p style="text-align: justify;">
          One of the most differentiation between two languages is Smalltalk uses a standalone editing environment that runs on <em>image-based persistent</em> [31], which would help reduce the cycle between running and editing the application [5]. But the image-based environment also exposes developers to many dangers. For example, during program development, the developers would need to commit the code instead of backing up the image. Secondly, rebuilding the image is cumbersome in the long run.
        </p>

        <p style="text-align: justify;">
          Using the image-based environment implies that Smalltalk programs have no source codes. Everything is contained inside the image. But when it comes to class modification, it seems to be inconvenient for minor changes. Fortunately, Smalltalk has introspection, helping developers inspect and modify objects on the fly.
        </p>

        <p style="text-align: justify;">
          Java does not offer the image-based environment. The code needs to be compiled, and later loaded into the virtual machine for further execution. There must be good reasons for avoiding the image. Perhaps one of good reason is to separate the IDE and the programming language process.
        </p>

        <p style="text-align: justify;">
          <strong>11. DISCUSSION</strong>
        </p>

        <p style="text-align: justify;">
          In this paper, we discuss two object-oriented programming languages Smalltalk and Java on some basic and advanced features. We briefly compare syntax, and analyze language expressions. In detail, we study their object models, rela- tionships among classes and reflection API. Our concerns are not only about their variations but also how programmers would take advantage of the language features including the use of interfaces and traits, closures, and reflection. We also raise questions about the design principle of Smalltalk &#8211; the overwhelming number of methods in Object class. We note some differences in reflection API of two languages, and particularly suggest how Java could use reflection API to simulate higher-order function.
        </p>

        <p style="text-align: justify;">
          Our analysis include several Java versions, in which we highlight new features &#8211; lambda expressions and functional interfaces &#8211; and explains how it would possibly introduce a new programming paradigm in the language. We then compare these new features with existing models in Smalltalk.
        </p>

        <p>
          <strong>12. REFERENCES</strong><br /> [1] Beck, K. Smalltalk Best Practice Patterns. Volume 1: Coding. Prentice Hall, Englewood Cliffs, NJ, 1997.
        </p>

        <p>
          [2] BeginnersBook. Static and dynamic binding in java. , 2014. [Online; accessed 11-July-2014].
        </p>

        <p>
          [3] Black, A., Ducasse, S., Nierstrasz, O., Pollet, D., Cassou, D., Denker, M., et al. Pharo by example. 2009.
        </p>

        <p>
          [4] Black, A., Ducasse, S., Nierstrasz, O., Pollet, D., Cassou, D., Denker, M., et al. Pharo by example. 2009.
        </p>

        <p>
          [5] Blog, Z. The web is becoming smalltalk — zack?s blog. http://zacharyvoase.com/2013/02/10/smallweb/,<br /> 2014. [Online; accessed 5-July-2014].
        </p>

        <p>
          [6] Bracha, G., and Ungar, D. Mirrors: design principles for meta-level facilities of object-oriented programming languages. ACM SIGPLAN Notices 39, 10 (2004), 331–344.
        </p>

        <p>
          [7] Cunningham & Cunningham, I. Polymorphism and inheritance. http: //c2.com/cgi/wiki?PolymorphismAndInheritance, 2014. [Online; accessed 12-July-2014].
        </p>

        <p>
          [8] Ducasse, S., Denker, M., and Lienhard, A. Evolving a reflective language: lessons learned from implementing traits. In Proceedings of the International Workshop on Smalltalk Technologies (2009), ACM, pp. 82–86.
        </p>

        <p>
          [9] Hebel, K. J., and Johnson, R. E. Arithmetic and double dispatching in smalltalk-80. Journal of Object-Oriented Programming 2, 6 (1990), 40–44.
        </p>

        <p>
          [10] Hobart, and Colleges, W. S. Javanotes 6.0, section 10.1 – generic programming. http://docs.oracle.com/javase/tutorial/extra/ generics/index.html, 2014. [Online; accessed 04-July-2014].
        </p>

        <p>
          [11] Knudsen, J., and Niemeyer, P. Learning Java. O’Reilly, 2005.
        </p>

        <p>
          [12] Madbean. Guy steele on variables-captured-by-anon-must-be-final (closures). http://madbean.com/2003/mb2003-49/, 2014. [Online; accessed 1-July-2014].
        </p>

        <p>
          [13] Martin, R. C. Agile software development: principles, patterns, and practices. Prentice Hall PTR, 2003.
        </p>

        <p>
          [14] Mihajlovski, N. Java 8 resources &#8211; introduction to java 8 lambda expressions. http://www.java8.org/introduction-to-java-8- lambda-expressions.html, 2014. [Online; accessed 1-July-2014].
        </p>

        <p>
          [15] Niemeyer, P., and Peck, J. Exploring java.
        </p>

        <p>
          [16] Oracle. Default methods (the java http://docs.oracle.com/javase/tutorial/java/ IandI/defaultmethods.html, 2014. [Online; accessed 28-June-2014].
        </p>

        <p>
          [17] Oracle. Java collections api design faq. http: //docs.oracle.com/javase/8/docs/technotes/guides/collections/designfaq.html#a14, 2014. [Online; accessed 10-July-2014].
        </p>

        <p>
          [18] Oracle. Lesson: Generics (the java tutorials). http://docs.oracle.com/javase/tutorial/extra/generics/index.html, 2014. [Online; accessed 3-July-2014].
        </p>

        <p>
          [19] Oracle. Lesson: Introduction to collections [Online; accessed 28-June-2014].
        </p>

        <p>
          [20] Oracle. Object (java platform se7). http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html, 2014. [Online; accessed 28-June-2014].
        </p>

        <p>
          [21] Oracle. Overview (java debug interface). http://docs.oracle.com/javase/1.5.0/docs/ guide/jpda/jdi/, 2014. [Online; accessed 1-July-2014].
        </p>

        <p>
          [22] Oracle. Primitive data types (the java http://docs.oracle.com/javase/tutorial/java/ nutsandbolts/datatypes.html, 2014. [Online; accessed 28-June-2014].
        </p>

        <p>
          [23] Oracle. Providing constructors for your classes [Online; accessed 10-July-2014].
        </p>

        <p>
          [24] Rivard, F. Smalltalk: a reflective language. In Proceedings of REFLECTION (1996), vol. 96, pp. 21–38.
        </p>

        <p>
          [25] Scha ̈rli, N., Ducasse, S., Nierstrasz, O., and Black, A. P. Traits: Composable units of behaviour. In ECOOP 2003–Object-Oriented Programming. Springer, 2003, pp. 248–274.
        </p>

        <p>
          [26] Scott, M. L. Programming language pragmatics. Morgan Kaufmann, 2000.
        </p>

        <p>
          [27] University, P. S. Smalltalk: A white paper overview. http://web.cecs.pdx.edu/~harry/ musings/SmalltalkOverview.html, 2014. [Online; accessed 3-July-2014].
        </p>

        <p>
          [28] Wikipedia. Ad hoc polymorphism — wikipedia, the free encyclopedia.<br /> http://en.wikipedia.org/w/index.php?title=Ad_ hoc_polymorphism&oldid=605733911, 2014. [Online; accessed 2-July-2014].
        </p>

        <p>
          [29] Wikipedia. Duck typing — wikipedia, the free encyclopedia. http://en.wikipedia.org/w/index. php?title=Duck_typing&oldid=615442832, 2014. [Online; accessed 26-June-2014].
        </p>

        <p>
          [30] Wikipedia. Smalltalk — wikipedia, the free encyclopedia. http://en.wikipedia.org/w/index. php?title=Smalltalk&oldid=615496590, 2014. [Online; accessed 26-June-2014].
        </p>

        <p>
          [31] Wikipedia. Smalltalk — wikipedia, the free encyclopedia. http://en.wikipedia.org/w/index. php?title=Smalltalk&oldid=615496590, 2014. [Online; accessed 11-July-2014].
        </p>
