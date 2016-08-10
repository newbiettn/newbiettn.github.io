---
id: 39
title: Better var_dump in PHP
date: 2014-10-27T01:07:26+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=39
permalink: /2014/10/better-var_dump-in-php/
categories:
  - Uncategorized
tags:
  - mamp
  - mavericks
  - pear
  - php
  - var_dump
---
In this workaround you can find how to have a better _var_dump_ in PHP (because the built-in one is ugly).

My current environment is as follows

  * PHP5.5.14
  * MAMP Server (Regular)
  * OS X Mavericks

1. Make _pear_ in MAMP accessible by adding the following code inside your _bash_profile_

<pre class="lang:default decode:true">alias mpear='/Applications/MAMP/bin/php/php5.5.14/bin/pear'</pre>

Normally, you need to restart Terminal to trigger the new command.

2. Set _pear_ to the current _php.ini._ It should be done to make sure your pear does not associate with another version of PHP (which you don&#8217;t want).

<pre class="lang:default decode:true ">mpear config-set php_ini /Applications/MAMP/bin/php/php5.5.14/conf/php.ini
</pre>

3. Install _var_dump_ extension via Pear

<pre class="lang:default decode:true ">mpear install Var_Dump-1.0.4</pre>

Notice that in my case, it should be <span style="color: #000000;"><b>mpear</b>. It depends on how you choose in your <em>bash_profile</em></span>

4. Verify if it works by some PHP codes

<pre class="lang:php decode:true ">include_once 'Var_Dump.php';
Var_Dump::display ( $dummyValue );</pre>