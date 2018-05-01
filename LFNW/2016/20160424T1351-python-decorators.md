Steve Hastings

    def foo(x):
      'docstring'
      return x

    foo.__name__
    foo.__doc__
    help(foo)
    alias = add5
    alias.__name__
# add5

    @my_decorator
    def add5(x):
      return x + 5

    equivalent to 
    add5 = my_decorator(add5)

no-op decorator
===============
    def my_decorartor(fn):
      return fn

record types of functions
=========================

# Web frameworks, unit-test frameworks
    lst_type_a = []

    def type_a(fn):
      lst_type_a.append(fn)

any callable object (ie object with a .__call__() method, could be a decorator

wrap a function
===============
    def adder_factory(y):
      def add_y(x):
        return x + y
      return add_y

"The y you passed in is saved - this is a closure"

args
===

    def example(*args, **kwargs)
      print("pos : {}".format(args))
      print("named : {}".format(kwargs))

a prepararoty helper
=========

    def print_name(fn):
      def print_name_wrapper(*args, **kwargs):
        'wrap fn to print name'

        name = fn.__name__
        print("calling: {}()".format(name))


     return fn  

decorate your decorator
=========
      @wraps(fn)  #overwrite __doc__ and __name__ so they are kept from the wrapped fun

decorator with args
===================

    @my_dec(1,2,3)
    my_fun(*args)

equiv to:

    my_fun = @my_dec(1,2,3)(my_fun(*args))

So my_dec(1,2,3) must return a function object

    def wrap_to_log_on_call(fn, logger, level):
      name = fn.__name
      mesg = "calling: {}()".format(name)

      @wraps(fn)
      def wrapper(fn):
        def wrapped(*args, **kwargs):


use decorators to make cost-free conditional logging

    if debug:
        log_on_call = something.log_on_call
    else:
        log_on_call = something.do_nothing




    def log_on_ll

Use good wrapper names! Not "wrapper"

fibonnacci again
=================
decorate with @lru_cache: cache previous results, so we o

extras
======

https://www.reddit.com/r/Python/comments/4enqb7/sudo_decorator_an_evil_hack/

multiple decorators on single fun are possible, or you can create a decorator
with multiple-decorators inside

a decorator takes one argument = a function reference. To get around that,
we make a decorator which closes over the args to create a new decorator.

https://linuxfestnorthwest.org/sites/default/files/slides/python_decorators_inception.odp

context.lib.context manager
