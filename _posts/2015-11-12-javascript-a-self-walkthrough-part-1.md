---
id: 198
title: 'Javascript: a self walkthrough (part 1)'
date: 2015-11-12T10:07:41+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=198
permalink: /2015/11/javascript-a-self-walkthrough-part-1/
categories:
  - Uncategorized
tags:
  - functional
  - javascript
  - programming
  - walkthrough
---
<p style="text-align: justify;">
  I have been well-acquainted with Javascript since 2011, a bit late compared to some of my friends who were seemingly keen on creating animated web pages for school projects using supporting plugins like jQuery. Before 2011, I had no formal concept about Javascript, except the name. At that time, the language seemed to be extremely difficult to me, and that sort of though discouraged me to learn it.
</p>

<p style="text-align: justify;">
  Nevertheless, after nearly 3 years being a developer and 2 years having formal learning in VUB, I now have chance to observe the beauty of the language with supportive formal knowledge in my spare time. But still I retain my old opinion that <em>Javascript ain&#8217;t an language that most people think they can learn in quick and dirty ways</em>. Of course it is particularly easy to use jQuery and make it run like a charm, but to understand mechanisms under the hood of the language, there exists a harsh line. I think to become a good Javascript developer, at least we should grasp a firm knowledge about programming language, like <em>first-class object, </em><em>polymorphism, inheritance, delegation, asynchronous vs synchronous </em>and so on.
</p>

<h3 style="text-align: justify;">
  <strong>Javascript is a functional programming language</strong>
</h3>

<p style="text-align: justify;">
  First thing first, Javascript ain&#8217;t object-oriented, it is functional like Haskell. In functional programming language, we treat functions as first-class objects where we can:
</p>

  * Declare them using literals as we often do (e.g., <span class="lang:js decode:true crayon-inline">function</span>) <pre class="lang:js decode:true" title="Declare a function foo()">function foo() {};</pre>

  * Pass them as arguments to other functions. This characteristic is exclusive to Javascript and other functional programming languages. In Java, we do not have this feature until Java 7, but still it is minimal. <pre class="lang:js decode:true" title="Pass function as an argument">//In jQuery, too loop through a list, we often do
//To execute a function for each list item, we pass the function
//as an argument
$('li').each(function{});
</pre>

  * Assign them to variables. It is unlikely that we can do the same thing on Java <pre class="lang:js decode:true" title="Assign a function to a variable">//As often as declaring a standalone function, we can
//also declare a function and assign it to a variable
var foo = function bar() {};</pre>

  * Return them as results of other functions <pre class="lang:js decode:true " title="Return a function as a result of other function">//We declare a function bar() nested inside the function foo()
//As a result of foo(), we return function bar() without invocation
function foo(){
   function bar(){
      //doing something here
   }
   return bar;
}

//To invoke bar() from foo()
foo()();</pre>

### Different ways to invoke functions

Of course we can invoke a function easily using <span class="lang:default decode:true crayon-inline ">()</span> operator. But an additional thing we need to pay attention is _function context_, aka <span class="lang:default decode:true crayon-inline ">this</span> , a familiar thing from Java. When closely observe, we can list 3 different methods to invoke a function.

  * Invoke a function as a global function whose context is <span class="lang:default decode:true crayon-inline ">window</span> <pre class="lang:js decode:true" title="Declare a global function">//Declare a function and return its context
function foo(){
   return this;
};

//Examine if the context of the function is window
//Tips: using === to compare if two values have the same reference
console.log( foo() === window);</pre>

  * Invoke a function as a method <pre class="lang:js decode:true">//Create a variable with an property, and assign a function to the property
//The function is now a method property of the given object
function foo(){
   bar: function(){
      return this;
   }
}

//verify if the function context is the method owner
console.log(foo.bar() === foo);
</pre>

  * Invoke functions as constructors <pre class="lang:js decode:true">//Here we define the constructor of Foo object
//Note that we capitalised the name to help distinguish constructor 
//from other kinds of functions
function Foo(){
   this.bar = function () {return this;}
}

//Use the 'new' operator to create an instance of the object
var foo1 = new Foo();
var foo2 = new Foo();

//When we check, the context is our new instances
console.log(foo1.bar() == foo1);
console.log(foo2.bar() == foo2);</pre>
    
    Additionally to make it look like object-oriented languages, we can add _getter/setter_ using either nested or prototype-based functions about which we will talk later.</li> </ul>