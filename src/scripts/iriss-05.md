# Functions

Functions are reusable blocks of code.

They have inputs, usually perform some sort of process, then have an output.

We've been using a function called `main` to run all of our programs and examples so far. 

This is a special function that is called as the program starts. 

We've also used a few other kinds of functions and methods (special functions attached to data types) that are built into Rust. 

We can make and use our own functions too though.

This series has a free accompanying book, in which you can find all the details for this tutorial, check the description below.

My name is Daniel, welcome to IRISS.

## Preamble

Functions can be pure, or impure. 

A pure function takes zero or more inputs, does some processing and returns zero or more outputs.

This makes the function extremely predictable. 

Given the same inputs they will always produce the same outputs, and nothing else within the system will change. 

An impure function, might not always produce the same outputs given the same inputs, or may have side effects within the system, either changing something else in the system or having some other part of the system change what the function does.

## A Quick Note on Mutability

Up to now, we haven't needed to change any data once it's been created.

By default, all variables in Rust are implicitly "immutable", meaning that the values inside of them can not be changed.

This, for example, won't compile, we get an error that we can assign an immutable variable, but we can't change it.

Immutability is good because it prevents data from being changed by mistake which, going on the name of the variable in this example, seems like it's what's happened here it doesn't seem like `one` should contain `2`.

So we should try to use immutable variables as much as possible, but software is all about processing data.

If we could never mutate values, we'd always have to depend on creating values, which is fine for small data but imagine we're processing large data like an image.

Do we want to copy it every time we make a single pixel change?

You can explicitly opt in to mutability by adding the keyword mute (M U T) in front of the variable name.

## Creating and calling functions

Functions are defined with the `fn` keyword (short for function), followed by a name, followed by brackets `()` which may or may not contain parameters, possibly followed by an arrow `->` and a return type.

If no return type is specified the return type is the Unit Type, see our video on data types to learn more about that.

This part of the function is called the "function header".

The function is completed by a code block, code between curly brackets, which is also called the function body.

So lets create the simplest possible function:

This function is called `say_hello`, it has no parameters and does not return anything.

Because it writes to the terminal, this function is considered to be impure.

We can call the function using its name and empty brackets inside our main function.

## Passing Parameters

Let's get a little more advanced and give our function an input.

Inputs to functions are called parameters, and they live between the brackets in the function header.

we'll add a parameter called name, and specify that its type is a reference to a string slice.

After that we can use it like it's a variable inside the function body, so we'll use it in our print line.

Now we just need to update where we call our function to pass the parameter, as ever we'll use my cats name.

We can have multiple parameters too.

Parameters are ordered so when you call the function, you need to match the order they're specified.

Let's make a new function that greets two people.

In Rust, all parameters must be used when calling the function.

## Returning from Functions

Next we're going to move the side effect out of our function.

Instead of directly printing to the terminal from the function, we'll return the string we want to display and move the side effect to main.

We'll rename the function to reflect the change in behaviour

We'll add an arrow and the return type ,in this case `String`, to the function header

We'll create the String using the `format!` macro and store it in a variable `message`

We'll return the `message` from the function, remember the code blocks can be expressions, we don't need to explicitly write `return` (though we can), we just need to make the thing we want to return the last bit of the block and forgo the semicolon 

The variable isn't really necessary either, I just wanted to show you that the result of the format! macro is a String, now I've done that, lets cut the variable and return from the macro directly

We'll update the call site of the function to receive the variable greeting to the result of calling the function

Then we can use that variable in a print line in the main function

Now we have a pure function that takes an input, processes it, and returns an output.

## Recursion

In Rust, functions can call other functions, like how our main function called our create_greeting function in the last example

A function that calls itself is described as recursive.

Take for example this function will find the nth number in the fibonacci sequence

A quick side note, this function uses a boolean OR in the IF so the larger expression evaluates to true if either the left or right parts of the expression evaluate to true.

We also use the if/else as an expression so the return value of the function is equal to the value from the if/else blocks.

If the expression in the IF is true, then we return N, otherwise we return the result of calling the same function with new parameters.

Because we call find_fibonacci inside of find_fibonacci, this is a recursion.

Each time we call the function in this way, we add another layer on the stack.

The stack is finite, so if we give the function a large enough number, it will eventually run out of space in the stack, causing a stack overflow, and you'll see something like:

thread 'main' has overflowed its stack

For the advanced programmers watching, Rust does support "tail recursion" which is technique for turning a recursive function into a loop at compile time.

This not only minimises stack usage to effectively a single function call but is also much faster.

However, I think this is an overrated feature.

In any language that supports tail recursion, it's hard to guarantee the compiler will optimise in this way, and it's easy to break.

My recommendation is if you need to recurse a _lot_, then consider whether you can manually rewrite your function as a loop instead of depending on a compiler optimization.

## Ownership - Memory Management Primer

Variables in Rust have to live somewhere in physical memory.

This primarily comes down to the Stack, the Heap and the binary

For a deeper explanation, see our video on memory

The Heap, specifically, can be thought of as managed.

You ask the operating system to "allocate" you a block of memory to use for writing to and reading from

Once you're finished using that block of memory you "free" that block and return the memory to the operating system.

In some programming languages, this process is manual.

You have special commands for asking for memory to be allocated, and special commands for freeing it

This leads to some problems:

What happens if you try to use memory that wasn't allocated?

What happens if you try to use memory that you already freed?

What happens if you try to store more data than fits in the allocated block?

Not only is it surprisingly easy to make mistakes here, we're all human, the consequences can be severe:

around 70% of all security vulnerabilities today are caused by accidental misuse of memory.

In order to get around these problems, some programming languages use an automated method of memory management called
garbage collection.

In this method, you, the software engineer, don't have to think about the actions required to allocate or free memory.

Instead, as memory is allocated, the garbage collector built into the language, will monitor to see which parts of your program are actively looking at that bit of memory, through a process called reference counting.

Once the number of places using that data (the reference count) has dropped to zero, the garbage collector can safely free the memory.

This is much safer than manually managing the memory yourself, but comes with some of its own problems:

The garbage collector requires additional resources to manage memory 

This includes CPU time to do the work but in some cases can also require significantly more memory

Managing memory by proxy is less efficient than managing it directly, meaning its slower

And you have limited to no control over what the garbage collector does or when it does it

This can have big negative impacts on performance at uncontrollable times

Rust's method of memory management is a little different.

It's low level, giving you the speed of manual memory management, but its mechanisms are hidden behind abstractions that mitigate its risks.

It's certainly not as easy to learn, but once you get your head around it, it makes a lot of sense.

## Ownership - Introducing Ownership

In Rust, all data is "owned".

When the variable that "owns" the data goes out of scope, the data is dropped.

This means that if the data was stored on the Heap, then that bit of memory is immediately freed.

Let's have a play with this, first, lets look at the scope aspect of ownership:

Here we create a variable `a` what we'll refer to as the outer scope

Then we start a new code block which creates a new scope we'll call the inner scope

We can use `a` in the inner scope just fine, as the inner cope is part of the outer scope

We can create and use a new variable `b` in the inner scope too.

However, when we close the inner scope, `a` will still work, as we're still in the outer scope but `b` is no longer available

Once a variable is out of scope, it can't be used.

If tried to compile this code, in the error output it tells you it "cannot find value `b` in this scope" and what
line we tried to use it on.

It tells us _exactly_ what's wrong!

Rust's compiler messages are generally amazing, especially when it comes to working with ownership, so it's worth getting used to how Rust presents its errors.

Next, lets look at how data can only be "owned" by one thing at a time:

We'll create a string and put it into  our `a` variable

Then we'll assign `a` to `b`

We can no longer use `a`, if we try to compile we see the error "value borrowed here after move"

The statement `let b = a` "moves" ownership of the string from `a` to `b`.

Normally we wouldn't "move" data in this way (spoilers: this is, after all, a video on functions), but it neatly shows that the data can't be owned by multiple variables at once.

What does Rust mean by "move" though?

## Ownership - Move Semantics

You might have noticed that we swapped from `char`s in the first ownership example to a `String` in the second.

This is because there are two mechanisms at play: "Move" and "Copy".

Let's try the same code with chars:

This time we set a to a char...

and the code works... 

why didn't the String?

The reason for this is that `char` is Copy, that is to say that it has the `Copy` trait.

We'll talk more about Traits in a future video, but essentially Traits provide behaviour to Data and Data Types.

Things can have the `Copy` trait applied to them if they can be trivially copied and this usually (always?) means the data exists on the stack.

The reason for this is all that "allocating" and "freeing" memory on the stack we talked about earlier requires a non-trivial amount of time and resources.

When data has the Copy trait, instead of being moved from one variable to another, it's copied.

This mechanism on data that has the Copy trait is implicit.

Data that does not or can not implement Copy may still be duplicated if it implements the trait `Clone`, which provides the `clone` method.

A method is a function that's attached to some data or data type, we'll talk more about methods when we talk about Traits in the future. 

This mechanism is explicit in that you have to call `clone` yourself.

String already implements the Clone trait, so lets use that to duplicate the data in our `a` variable instead of moving it.

This now works too!

You'll also note that we used a full fat `String` here, not a string slice reference.

Here's what that would look like:

Because `"hello"` exists inside the binaries data you can not "Own" it.

Ownership would imply that once its no longer used it can be freed, but as its part of the binary, that wouldn't make sense.

Instead, we just get a reference to where the value exists in memory.

This reference is also immutable, you can't change values in the binary.

Immutable references _are_ Copy though, so setting `b` to `a` is a copy instead of a move.

But, what do move semantics have to do with functions?

## Ownership - Functions and Ownership

When we pass data into functions through the use of parameters, the data follows the rules of move semantics.

Let's go back to our `create_greeting()` function.

Instead of passing in a string slice reference, what would happen if we passed in a full fat String?

In this example, we move ownership of the data stored in the variable `yuki` into the parameter `name`.

This means after the function the variable `yuki` can no longer be used.

Because the `format!` macro does not take ownership of the data in `name`, we _could_ return both the message _and_ the original `String` data using a tuple.

Let's change the `create_greeting` function to return a tuple that includes both strings.

When the function returns we destructure the tuple (see the control flow video for more on that) into two new variables, `message` and `yuki`.

A quick note, this new `yuki` is a different variable from the old `yuki`, this is called "shadowing". 

Unless you do the inside an inner scope, like we were doing earlier, this is functionally meaningless, just interesting.

Anyway, _this_ is obviously a terrible way to deal with ownership. 

We don't want to have to pass ownership back and forth just to use functions.

Going back to our original function, you can see that we are taking a reference to a string slice instead.

The `String` type can be "dereferenced" into a reference to a string slice

Remember the internal representation of a String is the same as a string slice

So we can create a reference to our full fat `yuki` String using an ampersand.

Let's say we wanted our function to modify the string instead.

Ideally we'd want to avoid modifying data that's passed to a function but sometimes that's not possible, if you need to do it you can pass a mutable reference.

In order to mutate greeting via a mutable reference, the variable itself must also be mutable

When we pass the reference, we are explicit that we are allowing the reference to be mutable too 

The function takes a mutable reference to a String, not a string slice... string slices are not mutable, but Strings are.

Before we move on, one last note about references: you can have as many immutable references to a value as you like, OR a single mutable reference.

Immutable references are Copy, but Mutable references are not.

## Ownership - Lifetimes

So far so clear, but it turns out that keeping track of those references is actually quite hard.

Let's create a function that takes a reference to a string and returns two parts to that string:

We'll take a point at which we will to split the string

We're going to make sure that point is never greater than the length of the string in bytes

Then we'll split the string in two at that byte (starting from zero)

To do this, we use a Range to take a sub slice. 

We discussed half open ranges last time, but this means starting from the beginning of input up to but not including the "up_to"th byte.

And this means starting from the "up_to"th byte and continuing to the end of input.

Also remember that because we're using bytes here there's a serious risk of splitting the string in the middle of a character... ideally, never do this. 

The left and right parts of the split are returned in the tuple which we immediately destructure

The cool thing here is that the string isn't duplicated, the values `left` and `right` are references that point to the inside of our original input string! 

How does Rust know that though? 

Let's confuse it a bit.

Instead of splitting at a particular point, we'll find a sub string inside the input string, then split around that:

This example won't compile, we get the error: `expected named lifetime parameter`, what does that mean?

We also get guidance from the Rust compiler on how to fix our problem, and what it suggests will work, but once you understand the problem, you'll see there's a better way to solve it.

A lifetime is Rust's way of tracking reference usage.

A reference needs to be tied back to its owning variable and this relationship must be understood at compile time.

Let's tie what we know about ownership to what we know about the stack and heap.

Some data is stored on the stack, some is stored on the heap

Heap data is tied back to variables on the stack that "own" the heap data

As we enter a function, all the space required for the stack data is added to the top of the stack

Further functions go up the stack

As functions come to an end they are removed from the top of the stack

Owned data that is not returned down the stack is freed

When it comes to references, we need to make sure that a reference to owned data does not out live that owned data.

For example, lets create a string on the heap called `my_string`.

We'll then return a reference to that string, but not the string itself.

This would mean that when the function comes to an end, the variable `my_string` goes out of scope,
so the data that it owns on the Heap is freed...

What would happen if we used the returned reference now the data that it references has been freed?

Luckily Rust won't let us do this

In order to prevent this, Rust tracks the lifetimes of each reference and its owned data.

It can often do this automatically.

In the first version of our function, Rust can see that only one reference enters the function, no other references are in play, so Rust knows that the two references that are returned _must_ be tied to the same data as the incoming reference.

In the second version of our function, we pass two references in, now Rust is now less sure about which data the returned references should be tied to.

The suggestion the Rust compiler gives us is to tie all the references to the same lifetime.

This actually is a valid approach as Rust will use the shortest living bit of data to tie the references to.

Let's do what it says.

Rust lifetimes are annotated with generic labels, remember labels from the last video?

If we're being lazy we might use a single lettered label like `'a`, though if a lot of lifetimes are at play it's definitely better to use longer, clearer labels.

We'll change the header of our function to include the lifetime `'a`.

First we tell Rust we want to use a lifetime by putting it inside triangle brackets after the function name.

Then we mark each reference with that lifetime.

You can run the above and see this example works just fine, however, it's not ideal.

We've tied all the references to the same lifetime, which Rust interprets as "whatever dies first".

Using a contrived inner scope again, we can create code that doesn't work.

We've confined our sub string to the inner scope.

When the inner scope ends, that data is freed.

You and I know that the left and right references are tied to the input variable in the outer scope, but we told Rust that it should tie all the lifetimes together.

Since the sub string lasts the least amount of time, that's the lifetime it used.

So when we try to compile we get "borrowed value does not live long enough"

To fix this problem we can either give sub_string a different lifetime annotation, for example "b" or just remove the lifetime annotation from the sub_string parameter.

Now everything works as we'd expect:

References and lifetimes get really tricky.

Here's how I like to think about them.

Imagine a person flying a kite.

The person is the owner of the data, regardless of where the data is actually stored

The kite is the reference, and the string (or kite line) tying them together is the lifetime.

The kite can go up and down the stack freely, but it can't go below where the person is standing, that's our equivalent of the ground.

Multiple people can fly their kites through the same functions, and each one has its own kite string (lifetime).

Its even possible to entwine the kite lines.

Imagine a function that takes two string references and returns the longest. 

This is like a function that accepts two kites but only the largest is returned.

Because you don't know who that kite belongs to, it can't go below the person standing highest on the stack.

Lifetimes can be incredibly powerful, there's no need to fear them.

One amazing example is that say we have a string that contains some complex data, for example it could be a serialized format like JSON or YAML, or it could be something like an Advent of Code input string.

Lifetimes mean that you bring that String into your program, allocating the memory for it once, then turn it into a complex data structure.

So long as the original String exists, the data structure can need not allocate any further memory, which is incredibly efficient.

Here's an example with a User struct that has two properties, both of which are references to string slices.

The data for this struct is in a String.

When we parse the String, it can return a new User, where the references are both tied back to the original string.

## Best Practices

So here's my list of best practices when it comes to working with functions:

Create a function whenever a section of code can be described in a few words
 
The function name should describe what that code is doing

Functions should only do one thing, avoid big branches inside functions

Keep functions short, but not too short.

Functions should be set of instructions grouped together, too few, and it may not be worth the function, too many, and it may need to be broken down into more functions

Do not take ownership unless you expressly need to own the data

_Try_ to avoid mutable parameters
 
Be specific about your lifetimes, if using more than one or two, try naming them

## Homework

How did you get on with your control flow homework?

Here's my Fizz Buzz program

I created a Range, 1 to 100 (note the equals sign to make sure 100 is included)

I used that with a For In loop

I then used a match statement with match guards to check if the number was divisible by 3, 5 or both

And printed the relevant output

Did you manage it a different way? 

If so, let me know in the comments.

This time I'd like you to create a function that splits our string like before, but instead of returning a left and right side it splits every part of the string.

There is already a split method on string slices, but I'd like you to only use find, and create a recursive function.

You'll need to use a collection type to store all the chunks created by the split, I suggest using `Vec` which is built into Rusts standard library.

A Vec is a variably sized array that you can push new items on to the end of using the `push` method.

To check whether your Vec contains the correct information after the program has run, use the `debug` macro (we'll talk more about how this works another time, all you need to know is it will work for a Vec of string slice references)

The header of your function might look something like this... but I'm not going to tell you what the life times should be

If your main function looks like this 

then your output should look something like this

This isn't an easy task, and will require thinking about the lifetimes carefully. 

If you get stuck, feel free to ask for help in the comments below

## Next time

Next time we're going to look at another of my favourite Rust features, the built-in test tools.

We'll also take our first little peak at modules and conditional compilation (which is I'm afraid is both way easier and way more boring than it sounds)... testing in Rust is awesome though I swear.

After the next video, we're also going to change the way we talk about Rust, so don't miss out!

If that sounds interesting, don't forget to like and subscribe

Do let me know how you get on with the homework!

And I'll see you next time
