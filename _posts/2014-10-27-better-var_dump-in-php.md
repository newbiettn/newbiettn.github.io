---
id: 39
title: Better var_dump in PHP
date: 2014-10-27T01:07:26+00:00
author: newbiettn
layout: post
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

``` php
alias mpear='/Applications/MAMP/bin/php/php5.5.14/bin/pear'
```

Normally, you need to restart Terminal to trigger the new command.

2. Set _pear_ to the current _php.ini._ It should be done to make sure your pear does not associate with another version of PHP (which you don&#8217;t want).

```
/Applications/MAMP/bin/php/php5.5.14/conf/php.ini
```

3. Install `var_dump` extension via `Pear`

```
mpear install Var_Dump-1.0.4
```

Notice that in my case, it should be `mpear`. It depends on how you choose in your `bash_profile``

4. Verify if it works by some PHP codes

``` php
include_once 'Var_Dump.php';
Var_Dump::display ( $dummyValue );
```
