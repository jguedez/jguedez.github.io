---
layout: post
title: "Hy: Lisp syntax for Python"
date: 2013-10-25 20:20:05 +1100
comments: false
categories: [hy, lisp, python]
---
You got parens in your Python...
--------------------------------

Every now and then you find a project that is simply **awesome**. I have played
with [Clojure](http://clojure.org/) several times in the past and while I like
it a lot, the **_slow_** start-up of the JVM turns me off.

If I was doing a bigger project the startup time wouldn't be so bad, but I would
like to start using the language for small stuff first while I get used to it -
but we are talking waiting many seconds to have a working repl versus an
immediate and powerful [IPython](http://ipython.org/) prompt.

Enter [Hy](http://hylang.org/), a Lisp that compiles directly to Python ASTs. It
is completely transparent to invoke Python code from Python and vice versa. It's
clear that it's heavily inspired by Clojure, and that's a big plus in my book
(you even get the threading macros!). Best of all it gives you access to write
your own macros within Python.

<!-- more -->

I found out about Hy from a post in [Hacker News](https://news.ycombinator.com/item?id=6700103),
leading to a [online interpreter](http://try-hy.appspot.com/) with an excellent
presentation.

I think Hy will let me play more with a Lisp syntax, get more comfortable, and
then over time use Clojure more as well.

The following is an example macro from the Hy documentation, enabling infix
notation in Hy, which typically uses prefix notation as is typical of a Lisp.

{% codeblock lang:clojure sample.hy %}
(defmacro infix [code]
  (quasiquote (
    (unquote (get code 1))
    (unquote (get code 0))
    (unquote (get code 2)))))

=> (infix (1 + 1))
2
{% endcodeblock %}
