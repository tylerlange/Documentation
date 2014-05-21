SmartThings Sandbox Groovy Limitations
======================================

For reasons of security and simplicity, SmartThings runs within a
sandboxed environment. Due to this, there are a few things that you can
typically do in Groovy that we don't support.

Classes
-------

SmartThings doesn't allow you to define your own classes within our
platform. You cannot do this:

::

    class foo {
      def list
    }

Within SmartThings you're limited to define a private method like this,
which would get list by reference and set a value to it.

::

    def foo(list, value) {
        list << value
    }

    x = []
    foo(x, 1)
    foo(x, 2)

Example amended from the Groovy Documentation User Guide. Learn more
`here <http://groovy.codehaus.org/Scripts+and+Classes>`__.

What Else?
----------

Here are a few other things you can't do within your SmartApps.

-  Define closures outside of methods
-  Define or modify MetaClasses
-  Import your own classes; you can only use select SmartThings
   supported libraries
-  Use the sleep() method (you can however use a SmartThings method
   called pause) TODO Link Method
-  Create and use new threads
-  Use System level methods, such as System.out
