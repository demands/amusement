**What is a closure?**

A closure is an excellent feature of many programming languages that have first-class functions. Notable languages that support closures include:

* JavaScript
* Ruby
* C#
* Scheme
* Lisp

According to Wikipedia, a closure is a *function* together with its *referencing environment*. Before I can really explain what a referencing environment is, let me carefully define the term "function":

At its heart, a function is a set of instructions that came from a block of code somewhere. This is a simple function written in ruby:

    def greet
      puts "Hello world!"
    end

A function on its own is kind of like a recipe. It doesn't actually do anything, but it describes a process that does something we may want to accomplish -- like making a batch of brownies, or printing a friendly greeting to the screen. To actually run the function, you need to call it.

    greet # => "Hello world!"

A language that has first-class functions gives you the ability to treat functions as variables. In ruby, first-class functions are called *procs* (short for "procedures") and they look like this:

    greet = Proc.new do
      puts "Hello world!"
    end

Calling a proc works like this:

    greet.call # => Hello world!

Since we can treat first-class functions as variables, that means we can pass them around as arguments to other functions:

    def twice( proc )
      proc.call
      proc.call
    end
    
    twice( greet ) 

    # => Hello world!
    # => Hello world!

It also means that we can create functions that return other functions, like for example:

    def create_greeter
      Proc.new do
        puts "Hello world!"
      end
    end

    greet = create_greeter
    greet.call # => Hello world!

The code in the previous example is our first explicit example of a closure. In this case, the proc called `greet` is the function, and the scope defined by `create_greeter` is `greet`'s *referencing environment* -- the environment that declared `greet`.

Unfortunately, the example gives us nothing that we didn't already have in the simpler definition of `greet`… so why don't we get a bit fancier, to see what this concept can do for us:

    def create_greeter( greeting )
      Proc.new do
        puts greeting
      end
    end

    greet_basic = create_greeter("Hello world!")
    greet_cheerful = create_greeter("It's a wonderful day today, isn't it?")

    greet_basic.call # => Hello world!
    greet_cheerful.call # => It's a wonderful day today, isn't it?

In this example, the referencing environment stores the argument "greeting", which is then accessed from within the function `greet`. This is one of the biggest powers of closures -- even though we ran `create_greeter` fully, and reached the end of the function, the functions it returned (`greet_basic` and `greet_cheerful`) still have access to the referencing environment that created them; the context of `create_greeter` when it built each greeting function.