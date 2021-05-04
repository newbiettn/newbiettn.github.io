---
title: "Prelude to Python (Part 1 - pyenv)"
author: "Ngoc Tran Trung"
date: "03 March 2021"
comments: yes
layout: post
published: false
---

I did used python around 2013-2015 in some of my previous projects but I never actually took it seriously. In 2016 when I first arrived at Swinburne to pursue my PhD, it occured to me that I had to decide between R and Python as my primary language for data science. Since I had never used R before, I chose it instead of Python. The decision at that moment has taken me to a path on which I rarely got a chance to use Python much over the last few years.

Python is not a tricky language to learn. In fact, it is one of the easiest programming languages that even non-technical persons are still able to learn in a relatively short time. That explains why Python is the chosen language in many ``learn to code'' courses/bootcamps that we can see all over the places.

In this post, I will definitely not try to convey fundamental programming aspects of Python. I instead would like to focus on little things of Python that ones should know in order to make programming and testing Python apps easier for them.

The first topic I would like to concentrate on is how to manage multiple python versions, specifically in macOS Big Sure 11.2.

## Mac does come with multiple Python vers
It is noticeable that macOS BigSure 11.2 is shipped with both Python 2 and 3 vers. We can verify this.

```
% which python
/usr/bin/python
% python --version
Python 2.7.16
% python python3
/usr/local/bin/python3
% python3 --version
Python 3.9.2
```

## Install pyenv
Although it is not required, `pyenv' document recommends us to install some dependencies prior to installing `pyenv' itself in order to avoid weird failures when we try to install new Python version in the future.

First, we install Xcode Command Line Tools.

```
% xcode-select --install
```

Then, we install some necessary dependencies. Again, these dependencies are optional but still recommended to have sane build environment for `pyenv'.

```
% brew install openssl readline sqlite3 xz zlib
```

After that, we proceed to install `pyenv' itself.

```
% brew update
% brew install pyenv
Updating Homebrew...
==> Downloading https://homebrew.bintray.com/bottles/autoconf-2.69.big_sur.bottle.4.tar.gz
==> Downloading from https://d29vzk4ow07wi7.cloudfront.net/4a05c5734bd99dc0adca0160e1ca79a291f2bd7fb8d52d
######################################################################## 100.0%
==> Downloading https://homebrew.bintray.com/bottles/pkg-config-0.29.2_3.big_sur.bottle.tar.gz
==> Downloading from https://d29vzk4ow07wi7.cloudfront.net/0040b6ebe07f60549800b211343fd5fb3cf83c866d9f62
######################################################################## 100.0%
==> Downloading https://homebrew.bintray.com/bottles/pyenv-1.2.23.big_sur.bottle.tar.gz
==> Downloading from https://d29vzk4ow07wi7.cloudfront.net/bc22faf3875668a018ad240d1752f9523fd138972c113e
######################################################################## 100.0%
==> Installing dependencies for pyenv: autoconf and pkg-config
==> Installing pyenv dependency: autoconf
==> Pouring autoconf-2.69.big_sur.bottle.4.tar.gz
🍺  /usr/local/Cellar/autoconf/2.69: 68 files, 3.0MB
==> Installing pyenv dependency: pkg-config
==> Pouring pkg-config-0.29.2_3.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/pkg-config/0.29.2_3: 11 files, 656.6KB
==> Installing pyenv
==> Pouring pyenv-1.2.23.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/pyenv/1.2.23: 738 files, 2.6MB
```

It looks like `brew` has installed `pyenv` 1.2.23. We can verify this.

```
% pyenv --version
pyenv 1.2.23
```

Now it is necessary to define environment variable for `pyenv'. To do that, add the following to your `~/.zshprofile' (I use `zsh', for others who use `bash', please refer to the [official document](https://github.com/pyenv/pyenv) for further information.

```
export PYENV_ROOT="$HOME/.pyenv" 
export PATH="$PYENV_ROOT/bin:$PATH"
# Add pyenv init to enable shims 
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

After that, restart your cell using the following command.

```
exec "$SHELL"
```
