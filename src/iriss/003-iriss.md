Data Types
==========

Preamble
--------

We lightly covered types in the last video, but only really to show the importance of where things live in memory.

Rust's types system is perhaps one of its most rigorous and powerful features, understanding it is key to
understanding why the language is the way it is.

Today we'll cover the basic Types (also known as Primitive Types) that exist within Rust,
and what structures we can use to create our own, more complex types (also known as Compound types).

This series has a free accompanying book, in which you can find all the details for this tutorial, check the
description below.

My name is Daniel, welcome to IRISS.

What is a Type
--------------

In programming a Type describes what kind of data or values can be used in a particular circumstance

In Rust, we use Types to describe variables, parameters, properties or anywhere else you can have a value, so that the
compiler knows what the data at that point will look like inside the running program, and how it will behave. 

This can include where in memory the data lives, how it's represented in memory, and what functionality can be assigned
to the data (though we'll cover that last part in a few videos time).

Primitive Types
---------------

Primitive types are effectively the building blocks of all other types.

I think this is an early point in learning Rust that scares off a lot of potential new Rust engineers. You see, Rust has
a _lot_ of primitive types.

I'm going to show this to you now, but I don't want you to worry about it.

You, whoever you are my dear rustacean, have already achieved things more complicated than learning this, don't panic

So, are you ready to see something terrifying that long before the end of the video you're going to have a complete
handle on?

Here we go (((Show chart)))

This is how many primitive types there are in Rust! And yes, as scary as it is, you will completely understand this in
just a few minutes! 

First and most importantly, forget this chart for now, there's really only five categories that we actually care about 
(((Show chart)))

integers, floating points, characters, booleans and string slices   

We'll go over each of these individually, explain how they work, their variations and what you might use them for.

Before we do, in order to understand those variations, lets very quickly cover binary.

### Binary Primer

Don't panic! No one is expecting you to learn to count in binary. Counting in binary is fun, but pretty useless.

All I want to do is show you how things are represented in memory because it's going to make all those Rust types make a
lot of sense!

Humans (mostly) count in base 10. That's numbers going from 0 to 9. You can imagine numbers as a series of columns,
where each column represents how many 1s, 10s, 100s, etc. there are in the number.

For example, the number one hundred and twenty-three contains one lot of 100, two lots of 10, and three lots of 1

When we add numbers to the columns, if the column goes over 9, then we add to the next column along.

So, if we add 1 to 9, it goes to 10, 19 goes to 20, and 99 goes to 100 (because the roll-over from the right most 9 adds
to the next 9 also causing it to roll over).

This counting system is called base 10 as each of those columns is 10 raised to the power of which column it is,
starting at 0:

- ten to the power of zero is one
- ten to the power of one is ten
- ten to the power of two is a hundred
- ten to the power of three is a thousand
- etcetera

Electronics, and by extension computers, can only really cope reliably with things that are `on` or `off` though. 

How do you count with only on or off? Well, what if instead of having ten possible values in each column (0-9 or base 
10), we only have two (0-1 or base 2). This is binary.

In binary our columns are a bit different:
- two to the power of zero is still one but
- two to the power of one is two
- two to the power of two is four
- two to the power of three is eight
- etcetera

So if we want to represent the number 13 in base 2, we can see it contains one 8, one 4, and one 1 (8 + 4 + 1 = 13).

If we mark those columns as one's and the others as zeros we get one one zero one

Sometimes when we want to write something in binary and be explicit that that is the system we're using we might write:
oh B one one oh one.

The oh B at the start makes it clear that a number like ob B one one oh one represents "thirteen" and not "one thousand
one hundred and one".

Each 1 or 0 is a ***b***inary dig***it***, which is where we get the term "bit".

Eight bits is a byte, and can represent the numbers from zero to two hundred and fifty-five, again, I'm not expecting
anyone to be able to _read_ this.

The reason why a byte is eight bits has a lot of history, but it basically comes down to character encoding: with 7
bits, you can represent 127 characters which covers english lowercase, uppercase, numbers 0-9, various whitespace and
punctuation, and still have 1 bit left over for simple error checking.

As a quick aside, as a software engineer, you're very likely to also see numbers written in hexadecimal (base 16).

This is because hexadecimal, is really nice when working with bytes. One byte (8 bits) perfectly maps to two
hexadecimal digits.

Hexadecimal digits go from 0 to 15, but are represented as zero to F (ie: zero, one, two, three, four, five, six, seven,
eight, nine, A, B, C, D, E, F).

oh X F is 15, and so is oh B one one one one. The number two hundred and fifty-five is much easier to write as oh X F F
than oh B one one one one - one one one one.

Both the oh B and oh X notation work in Rust if you want to write numbers as binary or hexadecimal respectively. You can
also use underscores to space numbers for legibility.

### Integers

Now that you've had that primer on binary, I bet those twelve different integer types are starting to make a lot more
sense!

The most basic number type in Rust is the `u8`. 

This is an _unsigned_ integer (represented by the `u`) that is 8 bits in length. 

Unsigned means that the number can only be positive (it does not have a negative sign). 

You might have already guessed, but this is one byte, and can hold the numbers 0 to two hundred and fifty-five. 

A byte like this can be used for all sorts of things, though one common example is as part of a color. 

We often represent colors as 8 bits of red, 8 bits of green, 8 bits of blue and sometimes 8 bits of transparency.

`i8` is an integer that can represent both positive and negative numbers (i.e. it's signed).

It also only uses 8 bits of data but instead of going from 0 to two hundred and fifty-five, it goes from negative
one hundred and twenty-eight to positive one hundred and twenty-seven.

You never need to know this, _but_, if you're interested in the mathematics of how it does this, it uses a method called
two's complement.

Two's complement, however, is complicated, and we don't think like computers.

The easiest way to visualise it is the left most column is the negative version of itself, and all other numbers are the
same. 

So, the number negative one hundred and twenty-five can be represented as one zero zero zero - zero zero one one. 
((Show table))

ie, the number contains one lot of negative one hundred and twenty-eight, one positive two and one positive one.

Negative one hundred and twenty-eight, plus two, plus one gives us negative one hundred and twenty-five.

So that's `u8` and `i8`, and now you've probably guessed that for all the other integer types;

- `u` means it can only be positive
- `i` means it can be positive or negative
- the number after is how many bits are available to the number

Now we can build up a little table to show the minimum and maximum of these types:
((Show table))

----

Wow, those numbers get big fast!

There's still two types missing though; `usize` and `isize`.

In this case, the `size` is also acting as the number of bits, however, unlike the other integer types, the size of
`size` is variable.

Rust is a compiled language, meaning that the code you write in Rust is transformed into instructions that a CPU can
understand. 

CPUs are all different, but they typically follow some given "architecture". 

For example, if you're watching this on a Windows or Linux desktop, the architecture of your CPU is _probably_ `x86_64`. 

If you're watching this on a Mac or a mobile phone, then the architecture is _probably_ `arm64`.

When you compile Rust it will compile into an instruction set for the architecture your machine uses, though you can
also tell it what instruction set to compile for, if you want to build it on one architecture but run it on another.

`x86_64` and `arm64` are both 64bit architectures, so when you build for these machines, the `size` in `usize` and
`isize` becomes `64`. 

However, if you were to compile for, say, a Cortex M zero microprocessor, then the instruction set would likely be `Thumb-1`
which is 16bit so the `size` in `usize` and `isize` becomes `16`.

#### Which integer is right for you?

You might think the obvious thing to do would be to use the largest possible integer types.

For example, you can fit pretty much every whole number you could possibly need into `i128`, so why use anything else?

There's two things to think about, first, what is the intended use of the number and, second, what is the architecture
of the machine you're running on?

In software engineering, a number is never just a number, it represents something.

As we mentioned earlier, colors are often (but not always), represented as 0 to 255 for each of red, green and blue.

This means that a `u8` is the best way to store these.

If you combine those three colors with another 8 bits for transparency, then you have four lots of `u8` which
can be represented as a `u32`.

`u8` is also a good size for representing a stream of unicode characters, which is where we get `UTF-8`, the default
encoding for Rust strings.

For larger, more arbitrary, numbers though, you still may not want to use the largest integer types.

While you can use integers that are wider than the architecture that you're running your program on, like using a
`i128` on a 64 bit machine, mathematics with those numbers will be slower.

The CPU can only process so many bits at once, so when it has numbers larger than that, it has to do multiple rounds of
processing to achieve the same results as it might have done if those numbers were stored in smaller integers.

You might then think that the best thing to do is use a `usize` or `isize` if you don't really care about how big a
number can get, and that's fine, and I often do this, but now you have to bear in mind that the behaviour of your
program may no longer be consistent on different machines!

A `usize` on a Mac is going to be cap out at over eighteen quintillion, but on a Cortex M zero it's a mere thirty-two
thousand!

By default, when you write an integer and store it in a variable, Rust will play it safe and use an `i32` as it doesn't
know what you might want to do with the number.

An `i32` will fit inside most CPU architectures without needing extra work and allows negative numbers.

However, it is more idiomatic to be intentional about the types you use.

My methodology here is roughly:

- does this number represent something of a specific size like a color or ascii character, in which case, use that size
- is this number going to be used to access an array, in which case it really ought to be a `usize`
- do I need something huge, computational cost be damned, then `u128` or `i128` is the way to go
- am I more concerned with speed than consistency, in which case `usize` or `isize`
- otherwise 32 or 64bit integers are probably fine, especially if I know what my code will run on

---

You can specify what type a number is either by annotating the variable you are putting it inside

Or, if that's not possible because you are, for example, passing the number straight to a function that could take many
number types, you can write the type at the end of a number

---

A brief note on Type Conversion: You can convert between integer types in several ways, which we'll talk about more
later, but I wanted to quickly go over some code from the last video.

In the bonus section of the last video, we got the number of milliseconds that had passed since midnight on the 1st
of January 1970, and then immediately used `as usize`:
((Show code))

The reason for this is the number of milliseconds since that date is approximately one trillion, seven hundred billion 
and is returned as a `u128`.

We wanted to use this as part of a calculation to work out an index into an array and indexes in arrays are always
`usize`.

If you were to compile this program on a 32bit architecture, then the number of milliseconds
is greater than what would fit into a `usize` which would be just over four billion.

When we use `as` it simply takes the number, whatever it is and tries to cram it into the size of the type in 
`as <type>`.

When going from a larger size to a smaller size (in this case, from `u128` to the equivalent of `u32`) it simply cuts
off the front of the data, leaving the least significant bits. 
((Show program))

### Floating Points

We've covered twelve different ways of storing whole numbers in Rust, but there are only two ways of storing numbers
with decimal points: `f32` and `f64`.

Floating point numbers are things like:
- zero point one two three
- one point two three
- one hundred and twenty-three point nothing

They're called floating point because the decimal point can move around.

Your immediate thought here might be that you should use `f32` on 32bit systems, and `f64` on 64bit systems, but
actually this isn't the way to think about these numbers, and I literally only discovered this while writing this 
chapter of the book!

To explain, floating points are not perfectly accurate. The bits of a floating point number are broken into parts:

- a sign, positive or negative
- an exponent
- and a fraction

Without going into too much detail on floating points this gives us a way of expressing very large numbers and very
small numbers but not every number in between 

(after all, there are an infinite number of numbers between zero and one).

Imagine using a floating point number to represent money. 

Someone comes into a store to buy an item that costs five hundred and twenty dollars and four cents.

Somehow they have a coupon for five hundred and twenty dollars and two cents.

The remainder that they need to pay is 2 cents, right?
((Show code))

---

If we expressed this in code using floating point numbers we don't get a perfect two cents!

Instead, if the currency you're representing uses "hundredths" for its minor currency like USD or GBP, then you can
(and maybe should) represent the total number of that, eg of cents for dollars or pennies for pounds, using integers 
instead.

So when should you use floats?

Floating point numbers are great for more abstract mathematics where perfect precisions isn't strictly necessary, for
example, vectors, matrices and quaternions which are often used in applications like video games and scientific models.

As to which you should use, you might think that it comes down to architecture again, for example, a program targeting
a 32bit architecture should use an `f32` and a 64bit architecture should prefer an `f64`... but if that's the case,
where is the `fsize`?

Actually, 32bit architectures are usually designed to support 64bit floating point numbers just fine, the difference
between `f32` and `f64` is that regardless of architecture, `f32` is about 30% faster, and `f64` is more "fine grain".

To decide which to use you just need to decide between speed and accuracy

### Characters

In Rust, we have a special type that represents a single character called `char` 

I say char, some people say car, unlike stud though I'm not changing my ways on this one, I'm on old man dying on a 
weird hill, deal with it

Char is always 4 bytes (32bits) in size and can be any valid "unicode scalar value" (which is to say, any character in 
unicode that's not a control character). 

In Rust a character is always written between single quotes, whereas string literals are always written between double
quotes.
((Show Code))

You can use any valid unicode character whether that's the upper or lowercase english letters A-Z, numbers 0-9, white
space characters, word characters from languages like Chinese and Japanese, emoji, or anything else that's a "unicode
scalar value".

We usually use characters in relation to finding things inside strings. 

You can also turn strings into a collection of characters and vice versa, however it's important to note that a
character inside a string may not take up 4 bytes (for example, english letters and numbers only take 1 byte) because
of how strings are encoded, however, once turned into a character, they will take up four bytes.

### Boolean

There is only one boolean type in Rust: `bool`. It represents `true` or `false`.

We often get boolean values through the use of comparisons between values. For example is a equal to b, or is c greater
than d, etcetera.

Boolean values are usually reserved for `if` statements, and this is a good thing to look out for as finding it else
where _might_ be a sign that the code isn't written in the best way. 

For example, functions almost never need a boolean parameter, this is usually a sign that you should have written two 
functions.

### String slices

Our old friend the string slice!

The type for a string slice is `str`, but you'll never see anything with the `str` type, you will usually see this
as a reference to a string slice ampersand str, which makes it unique amongst the primitive types.

`str` _should_ always be a UTF-8 string, which means that the length of a string in bytes may not necessarily be the
same as its length in characters.

For example and don't worry about the code yet ((Show Code))

The character that represents my cat's name has a length of 3 bytes when encoded as a str, but the str is only one
character long.

Its also worth remembering that when you turn a string into characters, each of those characters will take up 4 bytes
of memory, even though inside the string they might have only taken up one byte ((Show code)) 

(again, don't worry about the code here its just to show the size):

The size of a string slice depends on what's in it, which is why you won't see it on the stack (string slices live in
either the compiled output as string literals, or on the Heap inside a String).

A string slice reference is made up of two pieces of data, a pointer to where the string slice starts, and a length,
both of which are of known size but depend on the system architecture.

Fun fact about that reference though: you might wonder if it's just a pointer and a length, does that mean you can
have a reference to a string slice that exists inside a string slice, and the answer is: yes! ((Show code))

Just be careful when taking a slice inside a slice to make sure that the sub slice is a valid UTF-8 string.

Compound Types
--------------

### Arrays

Arrays are a collection of a single type. You might see arrays in two forms, either as a sized array on the stack, or
as a reference to another collection (also called an array slice).

When sized, arrays are annotated with the type open square bracket T semicolon N close square bracket where `T` is the
type of every item in the array and `N` is its size. ((Show code))

For example here's an array of exactly 5 chars. It will exist on the stack

((Show code))
When referenced as an array slice, you do not need to specify the size because, just like with references to string
slices, the reference not only contains a pointer to the underlying data, but also contains the size. We write this in
the form ampersand square brackets T where `T` is the type of every item in the array.

((Show code))
You can access elements inside the array directly by using an index value between square brackets. 

In Rust, indexing starts at 0 and the type of the index is always a `usize`.

### Tuples

Tuples are similar to arrays in that they are a collection of items, however, each item in the collection can be a
different type. 

This adds some flexibility but also some restrictions. 

For example, you can iterate over each item in an array, but not a tuple.

((Show code))
Tuples are written between brackets, and are only considered the same type if the types inside the tuple match.

((Show code))
Another difference from arrays is how you access a single item in the tuple, which you do with a dot, followed by
the number element you want. Again, this starts from 0.

There is a zero length tuple that is Rust's way to represent nothing, we call it the Unit Type. 

It's zero bytes, does not exist on the stack at runtime, and unlike other languages with types like `null` or `void`,
can not be used interchangeably with other types.

You might use this type in conjunction with generics which we'll come to in a bit.

### Structs

Structs are similar to tuples in that they are a type made up of other types.

Unlike tuples they are named though, and they must be defined outside of functions.

There are three types of structs:
- structs with named fields
- tuple structs
- and unit structs.

As we just covered tuples, lets quickly talk about tuple structs.

((Show code))
They look a bit like they're simply "named" tuples, and indeed they can be accessed the same way:

Similar to tuples, this kind of struct can be accessed with a `.` and a numbered value, _however_ unlike tuples,
structs have a concept of "visibility". 

Unless explicitly marked as public the fields of a struct are only accessible in the module in which it is defined, or 
its descendents. 

((Show code))
We'll talk more about modules later, however, to make the fields of a struct public, you can simply mark them as `pub`.

You don't have to make every field public though, if you'd like some parts of the struct to be public and others to be
private.

((Show code))
Named fields work pretty much the same as tuple structs except instead of having a numbered field, its named.

You can access the named field with a dot and the name.

Similar to tuple structs, fields must be explicitly marked as public to be accessible outside the module they were
defined in.

((Show code))
Unit structs are an interesting case that you probably won't find much use for until we get into more advanced Rust and
some of the cooler patterns and idioms that we use. 

Like the Unit Type, a Unit Struct has no value and no size. They're most commonly used as a marker, but you can also add
functionality to them via traits.

### Enums

((Show code))
Enums are for when you want to represent one of a finite number of possible values. 

For example say we want to represent the colors of a traffic light which can only be red, amber or green.

Many programing languages have this concept of enums, but what makes Rust enums especially awesome is that the variants
can additionally contain values. 

We've already talked about two such enums `Option` and `Result` which are two of the most important and widely used
types in the entire ecosystem, and we'll talk more about them when we talk about Generic Types shortly. 

((Show code))
As an example though, enums variants can be structured in either a tuple stype or a struct style:

In terms of memory usage, on the stack an enum will take up as much space as its largest variant, regardless of which
variant it actually is.

Generic Types
-------------

Generics in Rust allow the creation of entirely new types at compile time by combining types together. 

We've talked a bit about Option and how Rust uses it to represent Some value or None. 

((Show code))
Option is an enum with two variants, it is literally just this:

Note that after the name of the enum we have `T` in triangle brackets.

The triangle brackets express that this enum has a type (or types) that can be decided later, the `T` is a marker for
that type.

((Show code))
For example, say we want to create a type that represents either a single character, or nothing.

We can use the generic Option type to build Option char.

When we specify the value as some character, Rust can infer that type.

If it's None though, we'd have to specifically tell the compiler what the type is either through a type annotation or
other means.

Normally when accessing the variants of an enum, you must use the name followed by the variant eg Option colon colon
Some R, however Option and Result are so ubiquitous that their variants are globally accessible in any Rust code.

Result is another generic we've covered before. which usually represents either the success or failure of a function.

((Show code))
It has two types that can be decided later `T`, which should represent what type of data you expected to get back, and
`E`, which will be the type of the Error.

((Show code))
We'll talk more about functions in a later video, but in order to explain Result in context, the following example shows
the fully described Result type as the return type of the function, which is how we'd typically use this enum...

Though, you wouldn't typically use a String as an Error type, and we'll talk more about that when we get to Error 
handling later.

When we start talking about adding functionality to types, we'll also talk about how you can restrict what types are
allowed to be used in generics through the use of trait bounds.

Conclusion
----------

That is (almost) everything you need to know about types! 

The main thing we're still missing is ownership and traits, but we'll come to those when we talk about functions.

The main things to remember are:
- We have our primitive types that represent binary data. There's a lot of choice here, but that's a good thing!
- We can represent more complex types with compound types
- We can do the type equivalent "fill in the blank" using generics
- We talked a bit about two of the most common generics
  - Option (representing something or nothing)
  - and Result (representing a successful value or an error)

In the next video we're going to talk about controlling the flow of our program with branches and loops as well as
pattern matching and expressions, two of my favourite features of this language!

If that interests you, don't forget to like and subscribe 

Between now and then, why not try experimenting with your own types!

And I'll see you next time
