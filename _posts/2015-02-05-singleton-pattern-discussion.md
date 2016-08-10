---
id: 166
title: Singleton pattern discussion
date: 2015-02-05T11:47:07+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=166
permalink: /2015/02/singleton-pattern-discussion/
categories:
  - Uncategorized
tags:
  - design pattern
  - java
  - singleton
---
The **Singleton **pattern was used widely across in my projects. Back in time, I was confused when it came to an issue: if I should define <span class="lang:default decode:true  crayon-inline ">getInstance()</span>  method with supplied parameters.

My $0.02 is **definitely not**, since you intend to pass arguments to <span class="lang:default decode:true  crayon-inline ">getInstance()</span>  method, singleton is no longer itself. I think it would be much more like a factory by which you can instantiate many sorts of instances depending on passing arguments.

Let&#8217;s get hands dirty by making ourselves a singleton class.

<pre class="lang:default decode:true">public class SingletonDemo {
	private static SingletonDemo instance = null;
	
	private SingletonDemo () {}
	
	public static synchronized SingletonDemo getInstance(){
		if (instance == null) {
			instance = new SingletonDemo();
		}
		return instance;
	}
}
</pre>

Here we have a SingletonDemo class which will return a singleton by invoking the method <span class="lang:default decode:true  crayon-inline ">getInstance()</span> . The class seems to be fine, however, we can still optimize it by using **Initialization-on-demand holder idiom**.

<pre class="lang:java decode:true">public class SingletonDemo {
	private SingletonDemo () {}
	
	private static class SingletonDemoHolder {
		private static final SingletonDemo INSTANCE = new SingletonDemo();
	}
	public static synchronized SingletonDemo getInstance(){
		return SingletonDemoHolder.INSTANCE;
	}
}
</pre>

The refactored code is better in the sense that the class <span class="lang:default decode:true  crayon-inline ">SingletonDemo</span>  has no static variables and the static inner class is not loaded until the JVM executes it. It provides thread safety.

Now suppose the class has properties and of course we need to initialize the instance after instantiating. We have two ways to make it. We either add parameters to <span class="lang:default decode:true  crayon-inline ">getInstance()</span>  method or define a new <span class="lang:default decode:true  crayon-inline ">init()</span>  method with mandatory parameters. From my point of view, both the former and the latter have pros and cons:

1. The former perhaps _destroys _the **Singleton **pattern in the sense that the singleton now serves like a factory by which we can instantiate many different instances by supplying different arguments.

2. The latter, which exploits additional initialization method <span class="lang:default decode:true  crayon-inline ">init()</span> , preserves the **Singleton **pattern, however, it is not consistency since _developers may forget to invoke_ <span class="lang:default decode:true  crayon-inline ">init()</span> , which results in **incomplete instantiation**. It&#8217;s considered as bad coding practice I think.

<pre class="lang:java decode:true ">public class SingletonDemo {
	private SingletonDemo () {}
	
	private Foo foo;
	private Bar bar;
	
	private static class SingletonDemoHolder {
		private static final SingletonDemo INSTANCE = new SingletonDemo();
	}
	public static synchronized SingletonDemo getInstance(){
		return SingletonDemoHolder.INSTANCE;
	}
	public void init(Foo f, Bar b) {
		this.foo = f;
		this.bar = b;
	}
}</pre>

&nbsp;