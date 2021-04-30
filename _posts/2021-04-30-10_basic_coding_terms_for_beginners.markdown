---
layout: post
title:      " 10 Basic Coding Terms For Beginners"
date:       2021-04-30 23:04:36 +0000
permalink:  10_basic_coding_terms_for_beginners
---


A really super beginner list of vocab words for code, because sometimes as I'm learning, I really just want a brief 10 second synopsis on a term. (When language matters, this list is specific to Ruby.) So here goes:

**1. Variable:** a placeholder for a value. This is represented by a "bare word" data type. It allows you to access a value (which could be an extremely long bit of information) via your (typically short) *variable name*. In addition, as the name implies, a variable's value can *vary,* though the specific rules on *how*  it can vary, depend on the language. You can think of a variable as a labeled drawer holding some value. 

**2. Constant:** this is like a variable, in that it is also a placeholder for a value. However, unlike a variable which can vary, a constant will be set equal to a value once in the course of your program and then it should not be reassigned. (You technically *could* reassign the constant. The language won't physically stop you, but you shouldn't reassign it!) Anytime you call the constant's name, it will (or should!) always point to the value to which it was originally assigned. In Ruby, a constant starts with a capital letter.

**3. Method:** a set of instructions for something you want your code to do. Methods must be "defined" before they can be "called" or "invoked" at one (or many!) points later. 

```ruby
#this is the method definition:
def do_something
	puts "Hello"
end

#this is the method invocation:
do_something
=> Hello
nil
```

**4. Parameters:** a method can be defined to take information. The parameters are the placeholders in the method definition for that information. For example, we could make our above method take an argument containing some bit of data, and `puts` out the value of that data, rather than always `put`ting "Hello":

```ruby
def do_something(data)
	puts data
end
```

**5. Arguments:** these are very similar to parameters. These are the actual bits of data that are passed into a method's parameter spot(s) when the method is called. An argument can be raw data (like a "string") or it can be the name of a variable, where the variable name points to the raw data:

```ruby
#passing raw data in as an argument:
do_something("hi")
//=> "hi"
nil

#assigning a value to a variable and then passing that variable in as the argument:
greeting = "hola"

do_something(greeting)
//=> "hola"
nil 
```

**6. Return Value:** In Ruby, a method implicitly (meaning without you having to say so) returns the value of the last line of code - unless you specifically return at some other point. In all of our examples above,  `nil` is the return value of calling the `do_something` method. (Our `do_something` method is itself invoking the `puts` method on its last line. The return value of `puts` is `nil`, so our method `do_something` returns `nil` or the value of the last evaluated line of code. If we wanted `do_something` to simply return `hello`, we could type hello on the last line inside the function (we would still need to use `puts` if we also wanted the method to output hello):

```ruby
def do_something
	puts "hello"
	"hello"
end

do_something
//=> hello #this is the output 'hello'
hello #this is the return from the last line inside the method
```

**7. Terminal:** the terminal is where code is "run" and evaluated. This allows you to run a program to "test" it with special tests (written to compare expected code results with actual code results) or see the output, especially in the case of a 'command line program.'

**8. Text Editor** This is the place where you create and edit code projects, similar to using Microsoft Word or Google Docs to write a paper. Text editors are loaded up with code languages; they allow you to format code documents based on the language you are writing, as well as package up multiple files into a larger project. Some examples of text editors include Atom and VS Code. You can choose to run a terminal (where you actually run the code) from within a text editor or separately. Rehardless of whether you run your terminal separately or inside your text editor, you generally operate the text editor and the terminal hand in hand while you are desiging code. You'll type in the text editor to write or edit code then run that code in the terminal to see what happens.

**9. Keywords:** These are reserved words in a code language that have already been assigned meaning by the writers of the language. Because they are reserved (and already have definitions), they should not be used to name variables or methods. Doing so would cause confusion as you would be over-writing existing definitions with special, temporary definitions. Some examples of Ruby keywords are: `if`, `else`, `def`, `yield`, `do`, `puts`, `print`, etc

**10. Program:** this is a collection of code that can be compiled and run together. A program can be as simple as one file, or as complicated as you'd like. Ultimately this is a blanket term for a collection of code that performs some (or many) action(s).

And there you have it! These are some quick definitions to a few of the vocabulary words that I regularly found myself Googling at the beginning of my coding journey. This is by no means comprehensive, even for the vocabulary words above. As with everything in code, you could go much (much) deeper into these words, their meaning, and how they interact with so many other aspects of code.






 


