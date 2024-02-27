Setting Up
==========

Preamble
--------

When you create a program, how does it remember things? Last time, we created a variable and put our name
inside it. Where was our name stored?

Jumping into memory already might seem unusual. A lot of programming tutorials in any language would like to get you
writing code before you have to worry about such things. However, I think right off the bat we have a wonderful
opportunity to talk about how memory works in Rust and, because memory is so important to how Rust works, learning this
will simplify everything else going forward.

Today, we're going to make an extremely simple program and use that as the basis to explain how Rust remembers.

This series has a free accompanying book, in which you can find all the details for this tutorial, check the
description below.

My name is Daniel, welcome to IRISS

The Guessing Game
-----------------

To learn about memory, we'll make a simplified version of the
[guessing game](https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html)
from the official Rust book. Start by opening your terminal and navigating to where-ever your creating you projects,
just like last time, then we'll create a new project with:

```shell
cargo new guessing-game
```

Then we'll open the project go to the file `src/main.rs`. As before we'll see the same "hello, world" program:

```rust
fn main() {
    println!("Hello, world!");
}
```

Let's quickly change our hello world message to something that welcomes us to the game.

```rust
#fn main() {
    println!("Welcome to the guessing game!");
#}
```

And add another print line that tells the user what to expect

```rust
#fn main() {
#     println!("Welcome to the guessing game!");
    println!("I have chosen a color red, green or blue, can you guess which?");
#}
```

Let's pick a color that the user has to guess, I'm going to pick "blue" but feel free to pick your favourite

Before we move on, I want to talk about the "type" of the data we've just created.

We're going to cover types properly in the next video, but I want to quickly point some things out. 

The type of data describes a number of things but mainly, how that data is represented in memory and how it can be used. 

For example, is the data a string (like "blue"), or a number, or a boolean that represent true or false.

As you can see, my editor has written colon and str after actual. This isn't part of the text, it's just there to show
me the "type" of the variable.

This means that the variable `actual` contains a "reference" (represented by the `&`) to a "string slice" (represented
by `str`). We'll talk about why the variable only contains a reference and not the data itself later. You could, if you
like, manually write this `let actual: &str = "blue"` yourself, however, it's generally better to let Rust infer the
type, and only write it yourself if either Rust is unable to decide (because it could be multiple things) or if you want
to use something different.

Anyway, back to the program: let's output the color so that we can see the program is working ok

Great... but not much of a game is it, you don't even have time to guess before you're told what it is.

Before we tell the user what the actual number was lets ask them to guess.

In order to get some user input, we need to read from the terminal.

When the program runs in the terminal, we can ask the user to type things, regardless of whether you are on Windows,
Mac or Linux, this input is passed into the program through a stream of data called `stdin` (standard in).

Rust comes with a "standard library", the name is unrelated to the stream, they both just happen to be "standard"

We can access the standard library as a module called `std`. I pronounce this S T D, but I think most people say "stud"
and I should really start doing that.

Modules in Rust are a way of grouping up other bits of code such as functions, data types and even other modules.

We'll talk about them more in a future video. 

Inside of this is another module called `io` that deals with input and output.

If we weren't using the `println!` macro, this is where we'd have to come to write things out to the terminal too, via
a stream called `stdout` (standard out). `println!` is a nice little wrapper that makes writing to stdout pretty
trivial.

> For completion's sake I should mention there is one more stream called `stderr` (standard error). This is also an
> output stream that we can use to separate "good" output that is relevant to the normal use of the program to really
> any other kind of output, whether that be errors or just information not directly relevant to the main output.
>
> `stderr` is really useful for things like logging, which we'll talk about in the future, but if you want to quickly
> write something to this stream you can use `eprintln!` and `eprint!` the same way we use `println!` and `print!`

So, we get stdin using `std::io::stdin()`, this is a function call (we'll talk about functions in a couple of chapters),
that returns something called a "handle" that we can use for temporary access to the input stream.


The double colons just tell Rust that we're looking for something inside a module. We'll cover modules in detail later,
including how, why and when to make our own, as well as better ways to access them, but since we only need to write
this line once, this is the easiest way to do it.

We could store the result of `stdin()` in a variable, however, we only use this once, and then we're done with it, so,
off the back of the function call, we can call immediately call `.lines()`. 

This is a method (a special type of function that belongs specifically to some other thing, in this case it belongs to
the handle for stdin).

`.lines()` returns an iterator, allowing us to iterate (or step through) each line one at a time. We get the next line
by calling `.next()` on the iterator.

Wait wait wait, what are those `expect`s about?!

`expect()` is, I would say, the second-worst way you could handle something going wrong in your program. 

This is not "idiomatic" and you absolutely should not use this in anything except the most throw away software as it
will immediately cause the program to stop and spew a lot of information at the user. In the future we'll talk about
things going wrong and how to better handle them, however, as this program is just for you, I think you'll cope for
now. 😊

That doesn't explain what these lines are doing, or why there are two of them though. The reason for this is that there
are two possible ways `.lines()` might not work.

The first expect then:

```rust,ignore
.expect("No input was read")
```

When we call `.next()` on any iterator, there either is something next or there isn't. In some languages this concept of
something or nothing might be represented as either the data you wanted or `null`.

For example, in another language we might ask for a string and get `"red"` or `null`. 

"red" is a string but null is not... what happens if you try to use the returned value as if it was a string?

In languages with this behaviour, at the point where the data is used, not where it was generated, so you make sure to
always check that your data is what it's supposed to be, which, the language isn't making an effort to remind you to do.

Obviously many people think this ambiguity is bad, including Tony Hoare, arguably the "inventor" of this  behavior, who
has called it his "billion-dollar mistake".

Rust does not allow you to use types like this interchangeably, i.e. data can not be a string or null as these types are
not compatible. In Rust, we use a kind of container type to get around this called `Option`. 

Importantly, when a function returns an `Option` type you, the programmer, must check to see if it contains something,
and then extract the thing if it's there.

There are a number of ways to do this and `.expect` is one of the worst ways to do this. It will attempt to extract the
data if its there, or stop the program abruptly with the provided error message if it's not. We'll talk about better
ways to handle this in the future.

For the time being we're going to just say we can't deal with it not being there, we don't want to continue running the
program, and we want the program to stop. 

We use `.expect("message")` to say, if this is nothing, we are giving up, stop the program and print our message 
(plus a few other useful bits) to the `stderr` (see above). You can see why this behaviour is typically undesirable but
that we can allow it for this example, at least until we learn how to deal with `Option` properly.

The second expect is a bit different:

If the Option we got from `.next()` contains something instead of nothing, it _still_ doesn't necessarily mean we read
the users input correctly, reading data from standard in is itself fallible.

Fallibility, the nature that something may go wrong, in programming is another thing we've traditionally handled very 
badly.

A common way to deal with this in other languages is to stop executing and "throw" an error.

The problem with throwing errors is that it breaks the flow of execution. 

Normally code is executed one line after another but once an error is thrown, the next thing to get executed after the
throw is not obvious, it's going to be wherever the error is "caught"... if it's "caught".

Sometimes you may not even realise that some code you've written could throw an error because you've called someone
else's code, and they didn't make it obvious that their code could fail.

Rust does away with this with another enum type called `Result`.

If your function can fail, it must return a Result type.

Like with `Option`, `Result` is its own type that contains either the data we wanted, or an error.

The idiomatic way to handle Result types depends on what you're trying to do; could you recover from an error and keep
the program running, how do we want to report the error back to the user, etc. 

Using `expect` will, again, cause the program to immediately stop with the error message sent to standard error, and 
again, you can see why this is typically extremely undesirable.

For now, we'll make do with the two expects, and we have a working program. 

There is an actual color, the user guesses a color, and we print both to the screen.

Memory
------

To the meat of our today's video then.

You might have spotted that `actual` and `input` have different types

Don't worry if you didn't, when we added all the code associated with getting the input, it added a lot of types, so
you could easily miss it.

Importantly though, `actual` is a reference to a string slice, however, `input` is a capital S `String`.

What's the difference?

To understand this, we have to talk a little bit about how our program runs.

### The Stack

In Rust (and many languages in fact), programs are made up of functions; small, independent bits of code. In our
program we only have one function for now, which is `main`, but this still applies. 

When we run our program, the operating system assigns some memory for the program to run in, called "the stack".

Functions can call other functions and each time we go into a new function we create a new block of memory on the stack
that's the exact size of everything that function needs to remember.

For example, imagine the following program with the functions main, f1 and f2. 

When the program is initialized the operating system assigns some memory for the stack, then, all the memory required to
store all the variables in `main` (which it in this example is 4 addresses) is blocked off.

Then, as `main` runs, it calls the `f1` function which requires two more addresses. A new block with space for those
variables is put on top of the stack

`f1` then calls another function, `f2`, which requires 3 addresses worth of memory which create another block on the
top of the stack.

Once `f2` has finished, it is removed from the stack, then `f1` finishes, and it too is removed from the stack. Finally
`main` comes to an end and our program closes and all memory is freed.

You can see that the amount of memory a function needs is very important, but... how long is a piece of string? *shake*
How much space does a string take in memory?

Our three colors, "red", "blue" and "green" are 3 characters, 4 characters and 5 characters respectively. 

How much memory should we assign on the stack? What about the users input?

Is that 3 characters, 4 characters or the complete works of Shakespeare?

How much memory each function will require is decided at build time, and we don't know what the user is going to write
until later.

Anything that lives inside a variable goes on the stack, and anything on the stack must be of known size when the
program is compiled. 

In Rust, we have a specific name for this "Sized". Things like numbers, characters, booleans and even tuples are Sized,
but a string of characters is not, it is "Unsized".

So where does our data live if not inside the variables?

Returning to the top, you'll remember that our `actual` variable has the type `&str`. The ampersand tells us the value
is a reference to another type, that type being the "str" which what we call a string slice.

A reference is something that points to where some actual data is,

this is very handy when you want to tell something about some data without having to give it the actual data.

When you manually write a string between double quotes `"like this"`, it's called a "string literal".

This applies to our variable as well as every time we've used in a `println!`. 

String literals are hard coded into the binary file that we create when we compile the program (in our case, that's the
executable file that gets run) 

A "string slice" can be any part of a string stored somewhere else, so it can be characters 0 to 3 (b, l, u, e) of our 
string literal wherever it is in the executable file.

### The Heap

Our `input`, however, is very different, we don't know what's going to be in here at all at compile time. 

In order to store the data that the user gives us, we still need to put it in memory somewhere, but it can't live on the
stack.

Instead, we use another kind of memory called "the Heap".

At any point while the program is running, we can ask the operating system to allow us to access some amount of memory. 

We still need to know how much memory we need, but now we only need to know how much memory at runtime.

This memory comes from the Heap.

In the heap, we can store "Unsized" data (as well as Sized which can be useful under certain circumstances) and, then
because everything about the location of that data is Sized, we can store it in a variable on the stack.

`String` does not contain the data, but does know where the data is.

Some other important differences between the Stack and the Heap. 

The Stack is more limited in size, you should try to avoid storing large amounts of data here (even if the size is
known). 

Creating memory on the Heap takes longer than creating it on the Stack, this is because you have to communicate your
requirements to the operating system and wait for it to get back to you with that allocation. 

This doesn't take so long that you're going to notice... unless you do it a lot.

This is all great, but why is one of them `&str` and one of them `String` if both of them are just references to data
stored elsewhere? 

This actually comes down to "ownership". We'll go into ownership in more detail later in a later chapter but at a very
high level a reference to a string slice does not own the string data.

Whereas a capital S string owns, and therefore has full control over the data. 

Most importantly, ownership tells the program when to clean up the data.

When an `&str` is no longer needed, the reference is cleaned up, but the data still exists.

When a `String` is no longer needed, not only is the reference cleaned up, but memory is returned to the operating
systems control, freeing it up for something else to use.

It's important to note though that `&str` and `String` are different types.

The data they reference is formatted the same way in memory, but they are not interchangeable.

Later, when we discuss functions, you'll see that if a function requires a `String` you can not pass it a `&str` and
vice versa.

For example, we could turn our `actual` variable into a String by creating a new String from that data.

And we can get a reference to a string slice from inside the string. 

====

```rust
#fn main() {
let name = "Daniel";                   // This is a &str pointing at a sequence of bytes in the executable
let name_on_heap = String::from(name); // This requests memory from the operating system then copies "Daniel" into it
let name_ref = name_on_heap.as_str();  // This is a &str pointing to a sequence of bytes on the heap
#}
```

Despite the type difference between actual and input, they can be compared to each other.

In fact, Rust lets you compare any two types so long as someone has told Rust how to do the comparison, which, for 
Strings and sting slices, someone has.

Let's return to our program one last time and we'll add a branch to our code.

`if`/`else` is a form of flow control that allows us to do one thing if the expression we put in the "if" evaluates to
true, and something else if it doesn't.

So "if" input is equal to actual, we'll print "you win", else, we'll print "you lose"

To Review:
----------

Today we learned specifically about the Stack and Heap:

- The Stack is where variables live, data must be of known size at compile time (Sized), but access is very fast.

- The Heap is where we can store things regardless of whether we know their size at compile time (Sized and Unsized).
  We can store large amounts of data here but getting access to this memory initially is a tiny be slower.

- You can reference data with `&` if you don't want ownership of the data, but you do want to access it.

We also learned a bit about `Option` and `Result` which we will go into more in the next chapter, and future chapters
too.

Finally, we touched on controlling the flow of execution with `if`, however, this isn't the only way we can effect
the flow, and we'll talk more about this in a couple of chapters.

Bonus
-----

This has nothing to do with today's topic and I'm not going to go deep on anything but... I couldn't leave you with a
guessing game that isn't really a game.

If we want to randomise the "actual" value we can create an array of all possible values.

An Array is a list of things of the same type, seperated by commas and surrounded with square brackets. 

Arrays are Sized, but they can not have their size changed at runtime (there are other types for that).

We can then fake randomness by getting a big number that will change every time we run the program... like the time.

The unix epoch was midnight on the first of January 1970, we can find out how much time has passed since then and then
get that time in milliseconds.

We need to do a quick type conversion, which we'll talk about next time, this just lets us use the number of 
microseconds to access the array. This conversion is lossy but it doesn't matter in this case.

Finally we can find the remainder after dividing by the length of the array by using the remainder operator

This gives us a random(ish) index into the array. 

Now when you run the program, you won't know what the actual color is!

Next time
---------

Next time we're going to look more at data types.

We'll look at the primitive or scalar types

How we can create complex types with enums and structs

How we can make more flexible types with gennerics

And I'll introduce some of the built in collection types

If that interests you, don't forget to like and subscribe

In the meantime, have a play with your program

And I'll see you next time

