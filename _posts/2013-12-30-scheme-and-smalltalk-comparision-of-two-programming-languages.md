---
id: 172
title: 'Scheme and Smalltalk: Comparision of Two Programming Languages'
date: 2013-12-30T10:58:12+00:00
author: newbiettn
layout: post
categories:
  - Uncategorized
tags:
  - binding
  - functional
  - object
  - oop
  - programming
  - scheme
  - smalltalk
---
This article introduces my comparison between Scheme &#8211; a functional programming language, and Smalltalk &#8211; an object-oriented programming language. The comparison is to help me understand simplicity and beauty of both languages I have learnt.

**1. Common characteristics**

**1.2 Minimal language**

Firstly, both languages offer a very simple syntax. While Scheme programs mostly contain functions and macros with parentheses, Smalltalk programs are comprised of messages and objects. Moreover, in general, ev- erything in Scheme is lambda expression with returning values, and in Smalltalk is object with methods and variables.

However, Smalltalk language is verbose compared to Scheme, but it is a good tradeoff because Smalltalk is more easy-reading than Scheme.

**1.2 Type systems**

**1.2.1 Dynamic typing**

In general, a _dynamic typing_ language is the language that checks types of variables at runtime. In Scheme, we have both global and local variables, and similar to Smalltalk, it is dynamic typing. Suppose we want to define a global variable <span class="lang:default decode:true  crayon-inline ">uglyNum</span> , assign it value <span class="lang:default decode:true  crayon-inline ">99</span> , and then make an arithmetic computation with it.

<pre class="lang:scheme decode:true ">&gt; (define uglyNum 99)
&gt; (+ uglyNum 1)
100</pre>

In the code above, we did not specific any data type for <span class="lang:default decode:true  crayon-inline ">uglyNum</span>  but still can make computation with the variable. However, if we try to combine <span class="lang:default decode:true  crayon-inline ">uglyNumber</span>  with a string, an error will be throwed. Or we even can create the function <span class="lang:default decode:true  crayon-inline ">foo</span>  without any problems as long as we should never _invoke_ the function.

<pre class="lang:default decode:true ">uglyNum 99
uglyNum. -&gt; 99
uglyNum ’Hello Smalltalk’</pre>

Nevertheless, Smalltalk actually does not has types. Remember that _everything in Smalltalk is object_, and in our case, we simply refer variable <span class="lang:default decode:true  crayon-inline ">uglyNum</span>  to the integer <span class="lang:default decode:true  crayon-inline ">99</span>  or the string <span class="lang:default decode:true  crayon-inline ">&#8216;Hello Smalltalk&#8217;</span> . Thus, we can say Smalltalk is dynamic typing or untyped in terms of the variables.

**1.2.2 Strong typing**

Scheme is a strongly typed language, which means we can not convert from one type to another. Therefore, error is prevented at runtime.

<pre class="lang:scheme decode:true">;;; It will cause an error in Scheme
&gt; (+ 1 "1")</pre>

In case of Smalltalk, variables can have dif- ferent types at different moments, but objects do not. Types of objects is always strong. <span class="lang:default decode:true  crayon-inline ">’Hello World’</span> , for example, is always an instance of class <span class="lang:default decode:true  crayon-inline ">String</span> ; or <span class="lang:default decode:true  crayon-inline ">100</span>  is always an instance of class <span class="lang:default decode:true  crayon-inline">SmallInteger</span>.

**1.2.3 Duck typing**

Smalltalk provides duck typing happening at runtime. The behaviors of objects are called by sending any messages to the objects. Thus, if the object understand the message we send to it, we can assume its type even we do not know what class is involved.

**1.3 Lexical binding**

Like most modern languages, Scheme and Smalltalk both use lexical scope. The language supports lexical scoping, which means scoping organized in a structured hierarchy, and the innermost will be used in case there are several bindings in the environment.

In Scheme, we can use <span class="lang:default decode:true  crayon-inline ">let</span> , <span class="lang:default decode:true  crayon-inline ">let*</span> , <span class="lang:default decode:true  crayon-inline ">letrec</span>  to declare and bind local variables.

<pre class="lang:scheme decode:true ">;;; bind x to separated scopes
&gt; (let ((x 1))
    (let ((x 2))
      (print x))
(print x)) 21</pre>

** 1.4 Closure**

_Scheme_. Procedures, called closures, can record its surrounded environment to use it in later computations.

<pre class="lang:scheme decode:true">&gt;(define (plus x)
(define (plus-x y)
     (+ x y))
   plus-x)
&gt; (define plus1 (plus 1))
&gt; (plus1 2) 3</pre>

As you see, somehow the plus1 automatically stay around the return of the function <span class="lang:default decode:true  crayon-inline ">plus</span> . Thus, when we pass the integer <span class="lang:default decode:true  crayon-inline ">2</span>  to the function <span class="lang:default decode:true  crayon-inline ">plus1</span> , it will gather up and make a final computation.

_Smalltalk_. Each block in Smalltalk is a full closure and also an object because every block is an instance of the <span class="lang:default decode:true  crayon-inline ">Block</span>  class. So we can always refer to the environment recorded by the block object as long as the block object exists.

Suppose we create a class <span class="lang:default decode:true  crayon-inline ">MakeAdd</span>  that creates instances have two variables <span class="lang:default decode:true  crayon-inline ">x</span>  and <span class="lang:default decode:true  crayon-inline ">y</span> . We will also define a method that will return the block of making sum of <span class="lang:default decode:true  crayon-inline ">x</span>  and <span class="lang:default decode:true  crayon-inline">y</span>.

<pre class="lang:default decode:true ">￼￼￼theplus &lt;- MakeAdd x: 1 y:2.
theblock &lt;- theplus getBlock.
blockvalue &lt;- theblock value. 3
theplus x: 10 y: 20.
newblockvalue &lt;- theblock value. 20</pre>

What we have done in the code above is creating an instance <span class="lang:default decode:true  crayon-inline ">theplus</span>  of the class <span class="lang:default decode:true  crayon-inline ">MakeAdd</span>  and initializing its variables <span class="lang:default decode:true  crayon-inline ">x</span> , <span class="lang:default decode:true  crayon-inline ">y</span>  with 1 and 2 correspondingly. Next, we assign the returned block to new variable <span class="lang:default decode:true  crayon-inline ">theblock</span> , and its value to <span class="lang:default decode:true  crayon-inline ">blockvalue</span> , then we modify the values of <span class="lang:default decode:true  crayon-inline ">x</span>  and <span class="lang:default decode:true  crayon-inline ">y</span> . Finally we assign the value of <span class="lang:default decode:true  crayon-inline ">blockvalue</span>  to new variable <span class="lang:default decode:true  crayon-inline ">newblockvalue</span> . When comparing the return values of <span class="lang:default decode:true  crayon-inline ">blockvalue</span>  and <span class="lang:default decode:true  crayon-inline ">newblockvalue</span> , we see that the object <span class="lang:default decode:true  crayon-inline ">blockvalue</span>  is able to update changes to its captured environment and then assign it to <span class="lang:default decode:true  crayon-inline ">newblockvalue</span> .

**2. What’s different between Scheme and Smalltalk? **

In general, there exist many differences between Scheme and Smalltalk.

**2.1 Programming paradigm**

While Smallltalk is the pure object-oriented programming language, Scheme is functional-oriented.

_Smalltalk_. In Smalltalk, all variables are objects, which basically contains methods and variables. To invoke a method of an object, the programmer sends corresponding message to it with all necessary parameters. Strictly speaking, the only way to interact with objects in Smalltalk is through their sets of messages, called the interfaces.

Variables contain data of the object, which is private and only accessible through object’s protocol. They have the same attributes as variables in Scheme. For example, they have names and contain values, and are dynamically typed.

Objects are instantiated from classes. The class has its own methods and variables, which should be distinguished from methods and variables of instances. Moreover, instance variables are not shared among instances; instead they are strictly private to the corresponding instance.

_Scheme_. Scheme is a minimalist language based on Lisp. It is mostly functional. In a nutshell, there are two types of data in Scheme, _primitive_ and _list_. Primitives are the simplest type, and cannot be divided. Some primitive data types are numbers, symbols, Booleans, strings and characters. Lists are far more complicated, and considered as the most important data type in Scheme. We can create a list from primitive data types and use it as specific unit for further implementations.

**2.2 First-class citizen**

In programming language, first-class citizens are things we can manipulate in all general ways. In most cases, we can pass first-class citizens as arguments of functions, or set the return them from the functions. As Scheme is object-oriented programming language, and Scheme functional-oriented, they has different kinds of first-class

_Smalltalk_. Both blocks and classes are first-class objects in Smalltalk. As said, we can store blocks in variables, pass them around or send message to them.

<pre class="lang:default decode:true ">￼”send aBlockMessage to a block of sending aSelfMessage to self”
[self aSelfMessage] aBlockMessage</pre>

In some cases &#8211; such as enumeration &#8211; we can pass more than one block to a method.

<pre class="lang:default decode:true ">￼” the method detect: ifNone: takes two blocks”
user detect: [ :user | user name = ’Ngoc</pre>

Similar to the block, Smalltalk classes can receive messages. We can say one of the most frequent message sent to classes is new. However, classes requires specific messages, which we call class methods.

<pre class="lang:default decode:true ">”Create a new instance of the class Banana”
banana Banana new</pre>

_Scheme_. In Scheme, functions are considered as first-class, which means we can pass functions as parameters to other functions.

<pre class="lang:scheme decode:true ">&gt; (map abs '(1 2 3 -1 -5))
(1 2 3 1 5)</pre>

**2.3 Lambda calculus and block **

Scheme leverages lambda expression to create anonymous function. It is powerful and dominant when people can do most of the things in Scheme only by using lambda function. In Smalltalk, the blocks are closely equivalent to the lambda functions.

Smalltalk: <span class="lang:default decode:true  crayon-inline ">[:a :b :c | &#8230; expression&#8230;]</span>

Scheme: <span class="lang:scheme decode:true  crayon-inline ">(lambda (a b c) (&#8230; expression&#8230;))</span>

As said, we can pass lambda functions as arguments in Scheme, and in Smalltalk, we can almost do the same. For example, map- like operation such as <span class="lang:default decode:true  crayon-inline ">select:</span>  as below.

Smalltalk: <span class="lang:default decode:true  crayon-inline ">(1 to: 3) collect: [:x | x + 1].</span>

Scheme: <span class="lang:scheme decode:true  crayon-inline ">(map (lambda (x) (* x 2)) (list 1 2 3))</span>

Finally, compared Smalltalk’s block to lambda function of Scheme, lambda function is dominant as we are able to use it to create recursion functions.

**3. Unique features of the languages **

**3.1 Smalltalk**

**3.1.1 Smalltalk is reflective**

Smalltalk is a reflective programming language and &#8220;_has one of the most complete sets of reflective facilities_&#8221; (Rivard, 1996). In a nutshell, reflection is &#8220;_the ability of a program to manipulate as data something representing the state of the program during its own execution_&#8221; (Ducasse et al., 2009).This capability, as a result, allows Smalltalk programs examine and modify their runtime stacks and redefine their methods. As said, we can only access instance variables by the instance accessors. However, Smalltalk _introspection_ allows us to inspect the object, modify its instances and send message to it. In the example, we firstly use the message class to ask the class of declared instance student. Next, we use the message <span class="lang:default decode:true  crayon-inline ">isKindOf: aClass</span>  to verify if student is an instance of the classes <span class="lang:default decode:true  crayon-inline ">Student</span> , <span class="lang:default decode:true  crayon-inline ">Person</span>  and <span class="lang:default decode:true  crayon-inline ">Float</span> .

<pre class="lang:default decode:true ">student Student name: ’Ngoc Tran’ address: Brussels.
student class -&gt; Student
student isKindOf: Student -&gt; true student isKindOf: Person &gt; true student isKindOf: Float -&gt; false</pre>

We can also send the message to the class <span class="lang:default decode:true  crayon-inline ">Person</span>  to ask its method names defined locally by sending message selectors to it, or ask for the class locally defined and inherited methods by the message <span class="lang:default decode:true  crayon-inline">allSelectors</span>.

<pre class="lang:default decode:true ">Person selectors.
Person allSelectors.</pre>

In addition to inspecting objects and classes, Smalltalk intersection provides the capability of modifying objects and meta-objects at runtime, or adding and removing methods. For example, suppose we create a class whose instances act as proxies for other objects. Any messages sent to the real receiver have to pass through the proxy instance, in which necessary implementations, e.g., logging, will be executed before messages are delegated.

In practice, reflection provides a framework to develop programming tools such as debuggers, or to change the programming paradigm.

**3.1.2 Smalltalk has metaclasses**

In reality, the developers not only need to access the values of application data at runtime but also inspect their types. For instance, in Java, when using cast, the developers mean to ask the type of the variable. With Smalltalk, there is such a beautiful way to solve this problem &#8211; using _metaclasses_.

Before continue this part, there are several important rules we have to make clear.

_1. Everything is an object._

_2. Every object has class._

By following the rules, we can say that every class is an object, and has its corresponding class. However, what is the class of class?

The answer is _metaclass_, which holds all methods of its class and will be created automatically behind the scenes whenever a class is created. For example, the metaclass of the class <span class="lang:default decode:true  crayon-inline ">Person</span>  is <span class="lang:default decode:true  crayon-inline ">Person class</span> . Furthermore, inheritance is also possible in metaclass. It is significantly important because all methods in super metaclass will be inherited by sub-metaclass. Suppose that the <span class="lang:default decode:true  crayon-inline ">Person class</span>  provide method <span class="lang:default decode:true  crayon-inline ">name:</span>  for the class <span class="lang:default decode:true  crayon-inline ">Person</span> , then the <span class="lang:default decode:true  crayon-inline ">Student</span>  class will inherit this method as the <span class="lang:default decode:true  crayon-inline ">Student</span>  class extends from the <span class="lang:default decode:true  crayon-inline ">Person</span>  class. More interestingly, if we want to find the super class of the <span class="lang:default decode:true  crayon-inline ">Object</span>  class, we will get an endless query.

<pre class="lang:default decode:true ">￼Object class.
￼￼￼Object class superclass.
Object class superclass superclass.
Object class superclass superclass sup</pre>

So, what is the class of _metaclass_? The answer is <span class="lang:default decode:true  crayon-inline ">Metaclass</span> . As said, every object in Smalltalk is class, so <span class="lang:default decode:true  crayon-inline ">Metaclass</span>  is also an object, and thus it must have a class itself. What is the metaclass of <span class="lang:default decode:true  crayon-inline ">Metaclass</span>  class? The answer is <span class="lang:default decode:true  crayon-inline ">Metaclass</span>  class, and here is the interesting part, <span class="lang:default decode:true  crayon-inline ">Metaclass class</span>  is an instance of <span class="lang:default decode:true  crayon-inline ">Metaclass</span> . In short, it is an endless loop if we want to try to find out.

Indeed, metaclasses is dicult to grasp the whole concept but programmers do not have to understand it to use Smalltalk. And the parallel of metaclass hierarchy and the class hierarchy seems to be confusing but in real world, it helps creating elegant applications.

**3.2 Scheme**

**3.2.1 Scheme macros**

Macros is a beauty of Scheme by which programmer can construct new syntax. The macros are useful for many purposes. Using macros, we can hide complexity and avoid repeating ourselves, by which we can concentrate on high-level problems.

<pre class="lang:scheme decode:true">￼&gt; (define (zpn-x
         value
         z-exp
         p-exp
         n-exp)
(cond
  ((zero? value) z-exp)
  ((positive? value) p-exp)
  ((negative? value) n-exp)))
&gt; (zpn-x 1 (display "Z") (display "P")
   (display "N"))
ZPN</pre>

The result is not as we expected because three expressions are called even before the function <span class="lang:default decode:true  crayon-inline ">zpn-x</span>  is called. So, the function displays ZPN and returns <span class="lang:default decode:true  crayon-inline ">null</span> . The solution is to define a macro <span class="lang:default decode:true  crayon-inline ">zpn-x</span>  as follows.

<pre class="lang:scheme decode:true ">&gt; (define-syntax zpn-x
  (syntax-rules ()
((zpn-x
value z-exp p-exp n-exp)
(cond
  ((zero? value) z-exp)
  ((positive? value) p-exp)
  (negative? value) n-exp))))
&gt; (zpn-x 1 (display "Z") (display "P")
   (display "N"))
P</pre>

** 3.2.2 Scheme call/cc **

Scheme offers first-class continuation by <span class="lang:default decode:true  crayon-inline ">call/cc</span>  to &#8220;_control the execution order of the instruction_&#8221; (Wikipedia, 2013). In a nutshell, a continuation is such a lexical closure waiting for returned value of the current execution, and will resume the execution with the environment it wraps inside.

<pre class="lang:scheme decode:true">&gt; (define x 0)
;;; use call/cc
&gt; (+ 1  (call/cc
(lambda (cc)
(set x cc)3 )))</pre>

In short, the code above adds two numbers together &#8211; the integer <span class="lang:default decode:true  crayon-inline ">1</span>  and the <span class="lang:default decode:true  crayon-inline ">call/cc</span>  &#8211; in the first parenthesis. Here we have a <span class="lang:default decode:true  crayon-inline ">call/cc</span>  takes one argument, a lambda. Simultaneously, the <span class="lang:default decode:true  crayon-inline ">call/cc</span>  captures the context in the first parenthesis, say <span class="lang:default decode:true  crayon-inline ">(+ 1 [])</span> , which is the _continuation_.

Here the lambda will be triggered for the continuation <span class="lang:default decode:true  crayon-inline ">(+ 1 [])</span> . In the lambda, we bound the continuation to <span class="lang:default decode:true  crayon-inline ">x</span>  and return <span class="lang:default decode:true  crayon-inline ">3</span> . Therefore, it yields the result <span class="lang:default decode:true  crayon-inline ">4</span> .

<pre class="lang:scheme decode:true ">;;; x now holds the continuation
;;; and we pass the value 10 to x
&gt; (x 10)
11</pre>

After we bound <span class="lang:default decode:true  crayon-inline ">x</span>  to the continuation, we pass the value <span class="lang:default decode:true  crayon-inline ">10</span>  to <span class="lang:default decode:true  crayon-inline ">x</span> . This value will be use as the return value off the continuation. The return value is added to <span class="lang:default decode:true  crayon-inline ">1</span> . Therefore, we get the value <span class="lang:default decode:true  crayon-inline ">11</span> .

**References**

Beck, K. (1997). Smalltalk Best Practice Pat- terns. Volume 1: Coding. Prentice Hall, Englewood Cliffs, NJ.

Ducasse, S., Denker, M., and Lienhard, A. (2009). Evolving a reflective language: Lessons learned from implementing traits. In Proceedings of the International Workshop on Smalltalk Technologies, IWST ’09, pages 82–86, New York, NY, USA. ACM.

Rivard, F. (1996). Smalltalk: a reflective lan- guage. In Proceedings of REFLECTION, volume 96, pages 21–38.

Scharli, N., Ducasse, S., Nierstrasz, O., and Black, A. P. (2003). Traits: Compos- able units of behaviour. In ECOOP 2003– Object-Oriented Programming, pages 248– 274. Springer.

Wikipedia (2013). Continuation &#8211; wikipedia, the free encyclopedia.
