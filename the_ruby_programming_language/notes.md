### Chapter 6. Methods, Procs, Lambdas, and Closures

* To return multiple values from methods, the return statement “return a, b, c” is equivalent to [a, b, c] without using the “return” statement

* Return multiple values [a, b, c] is for parallel assignment

* Methods are always invoked on “self”. Methods that are defined outside of a class are invoked on the “main” object, which is an instance of the Object class.

* Singleton methods are methods that always exists on an object instance. The syntax is “def o.method_name … end”

( Class and module methods are actually singleton methods since a class is in fact an instance of the Class class. And therefore one way they can be defined is “def self.class_method .. end”

* Methods can be undefined using “undef method_name” with their defined scopes.

* “undef” can be used to undefine inherited methods, without affecting the definition of the method in the super classes.

* With a class or module, “undef_method” can also be used to undefine a method.

* “alias new_method_name old_method_name” and redefine the old method to argument the method

* The default value [] produces a new empty array on each invocation.

* Variable-length argument lists and arrays are defined by prefixing * in the last argument, e.g. def max(first, *rest). Within the definition, *rest is an array

* The “*” parameter must appear after all parameters with default values, it may be followed by additional ordinary parameters. It must appear before any “&” paraemter.

* The splat, again ‘*’, operator is used to explode the elements of an array in method invocation so that each element becomes a separate method argument.

* Enumerators are splattable, e.g. *‘hello world’.each_char

* Use hashes for named arguments.

* A bare hash, i.e. a hash without curly braces, can be used if it’s the last argument, except for the block argument if there is one.

* If curly braces follow the method name outside of parentheses, ruby thinks you’re passing a block.

Use “yield” with anonymous block argument

``` ruby

def just_yield(args)
  yield #
end

just_yield() { do stuff }

```

* Use ‘&’ to get explicit block argument to get block as a Proc. Use “call” to invoke the Proc. This will convert a block argument to a Proc object. But yield will still work.

* Use ‘&’ to pass a Proc as a block argument, but it needs to be passed in as an argument. This is used to create re-useable block code.

* Define a Proc object: Proc.new { |total, x| total + x } or lambda ->(total, x) { total + x }

* & is allowed to any object with a to_proc method. It will send to_proc to the object and the ampersand turns the resulting proc into a block argument

* Symbols’ to_proc convert them to an instance method proc of the same name. So [1, 2, 3].map(&:to_s) ⇔ [1, 2, 3].map { |x| x.to_s }

``` ruby

s = :to_s.to_proc #
s.call(10)        # => “10”

words.map &:upcase #
Convert a block to a proc by prefixing & to methods’ parameters

def makeproc(&p)
  p
end

``` ruby

```

Or p = Proc.new { |x,y| x + y } to create a proc

Or use lambda p = ->(x, y) { x + y ) to create a proc

Or in ruby 1,8, lambda { |x, y| x + y }

Lambda with local vars f = ->(x,y; i,j,k) { … }

Minimal lambda ->{} returns nil

Lambdas behave like methods: return control to yield

Proc objects behave like blocks: return control to the calling method

Lambdas and procs are closures

Method objects are less efficient than invoking methods directly

Use o.method to get a method object

6.8 Functional Programming - skipped in the first pass

### Chapter 7. Classes and Modules

“class” is an expression. The value of the expression is the value of the last expression with the class body

A “class” definition creates an instance of the Class class and a constant of the class name to refer to the class. In ruby, a class is an instance of the Class class and is referred by a constant

Use const_defined? to check if a constant exists

The value of a def statement is always nil

“is_a?” and “instance_of?”

The intialize method is automatically invoked by Point.new and is automatically made private so that it cannot be called to re-intialize an object.

Setters must be invoked explicitly through self, e.g. self.x = 2

attr (ruby 1.8), attr_reader, attr_writer, and attr_accessor (reader and writer)

Use “def -@” to define a unity operator

Use “def []” to define Array and Hash access

Define an “each” method to include Enumerable

Use “def ==(o)” to define equality. Type checking?

The default of eql? compares two objects

Use “def <=>” to define ordering behaviour. <=> returns nil if comparison cannot be performed

Struct.new(“Point”, :x, :y) or Point = Struct.new(:x, :y) create a new Point class with two mutable attributes, :x and ;y

Struct also creates [] for array and hash access and each and each_pair

Such class can be made immutable by opening it:

class Point
  Undef x=, y=, []=
end

Use self.method_name to define class method

Use class << self to define multiple class methods

Class variables (@@) and subclasses will alter them if redefined

Class instance variable (@) can be inherited

“private” are inheritable and only available within the owning class or subclasses.

“private” are implicitly invoked on self. It throws NoMethodError if it’s explicitly invoked on an object, including self

“protected” can be invoked on objects of the same class or its subclasses within the same class scope. Effectively, protected methods are only available to all objects of the same class and its subclasses

“private_class_method” and “public_class_method”

Method access control can by bypassed:
  o.send :method
  o.instance_eval { method }
  o.instance_eval { @x }

Use public_send for metaprogramming without method access bypassing

In ruby (or any other programming), you should only subclass when you are familiar with the implementation of the superclass.

Chaining: calling super to invoke the inheritance hierarchy.

super() is to invoke parent method with no arguments

super is to invoke with exactly the same arguments as the calling method

< Inheritance of class methods, instance varialbes > skipped in the first pass
