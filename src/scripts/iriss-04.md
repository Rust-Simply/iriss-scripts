# Control Flow

Ok, I know I said learning Rust wasn't hard, then threw two videos of how memory works and how data is represented,
but you've got that down now.

Today, we're on to actually writing some code, there's even some homework to look forward to!

Programs are typically executed one line at a time, this is called flow, but we can alter what the next line is with
control flow.

This series has a free accompanying book, in which you can find all the details for this tutorial, check the
description below.

My name is Daniel, welcome to IRISS.

## Intro

There are two main ways of controlling flow: branching and looping

Before we get into that though, lets talk about two of Rusts coolest features, which will come up a lot later,
patterns, and how blocks are also expressions.

## Patterns

Last time we talked about compound types. 

Tuples, Structs, and Enums allow the construction of more complex data from less complex data. 

However, if we want to break the data back into its component parts we can do that!

Patterns can be used to "destructure" compound data types like tuples fairly trivially: [[]]

---

Destructuring with patterns also works for Tuple Structs, however, you need to specify the name of the struct like
you're doing a weird backwards struct instantiation. [[]]

The same thing also works for Structs with Named Fields: [[]]

Here we extract the structs named fields straight into variables of the same name as its easy and the
names were appropriate. 

However, it might be better in the context of your program to name them something else. 

We can rename the variables using colons, like naming x to width and y to height: [[]]

Unfortunately, you can't extract data from Enums this way as the value of an Enum is one of a set of, not only values,
but potentially subtypes or shapes or however you'd like to describe them.

Take for example the humble Option: [[]]

How can we extract a char from Option char if 5we don't know whether the variable is Some or None... 

well, actually, we'll come to that soon.

## Blocks are Expressions

One of my favourite features in Rust is that code blocks (that's any code between curly brackets) can be used as
expressions

An expression in Rust is anything that could have a value.

So, for example, a + b is an expression where we're adding a to b which results in a value. 

You will also use expressions like a equals equals b to compare whether the values of a and b are the same, this
results in a value of true or false.

---

Usually you might use an expression as part of an assignment or an evaluation, for example "let c = a + b" or
"if a equals equals b", however, Rust also allows you to use a block (code between curly brackets) as an expression
_and_ the final value of that block can itself be an expression.

In this admittedly contrived example, we create the variables a and b and add them together.

---

Some cool things to note:
- a and b only exist within the code block, they're not accessible after
- the lines with let have semicolons
- the line with the expression a + b does not
- c will be equal to the evaluation of the code block, which itself is equal to the result of a + b
- the code block which c is equal to is also terminated with an exclamation because let can not be part of an expression

Why is this so cool? Because branches, loops and even functions all use code blocks!

## If

The most basic form of branching is the if statement.

In its most simple form it's an IF followed by an expression followed by a code block.

Two differences to most other languages, the expression being evaluated by the if is not in brackets and code to be
executed must always be in a code block, even if its only one line. 

The expression must evaluate to a boolean, either `true` or `false`.

If the expression evaluates to `true`, then the code in the block will be run, otherwise it won't be:

For example, we could create an expression that evaluates to a boolean by comparing if two numbers are the same, using
double equals: [[]]

If you want to run some code if the expression is `true`, but some different code if its `false`, then you can extend
`if` with `else`. 

Here we compare if the first number is greater than the second number. [[]]

You can chain `if`/`else` statements to create more complex branches. [[]]

Remember though, code blocks, including those in `if` and `else` are themselves expressions. This means they can
effectively return their own values [[]]

Some important things to note:
1. To make the code block an expression, the last line must have no semicolon
2. When we use let to set a value to the result of an expression, the expression must be terminated by a semicolon
3. All branches must evaluate to the same _Type_, even if they don't evaluate to the same _value_
4. Doing big blocks of `if`/`else if`/`else` is a mess, there's a better way which we'll talk about in a moment!

### Pattern Matching inside `if` and `else`

There is another way you can branch with `if` that doesn't require a boolean expression, pattern matching.

Pattern matching is similar to our destructuring earlier, but now we can use it as a conditional!

There are two ways to do this `if let ...` and `let ... else`.

Let's go back to that Option from earlier: [[]]

In the line `if let Some(c) = maybe_yuki` we are pattern matching on the Option, if it matches the pattern of
`Some(<variable>)`, then we extract the contents of the `Some` into the `<variable>`. 

Within the block (and only within the block), the variable C has the value from inside the Some variant of the Option.

This may be easier to observe with our own enum type: [[]]

Again, this example is contrived, there are better ways to do this.
 
You can also do the opposite, branch if the pattern does not match, using `let ... else`.

The important thing to note here is that execution can not continue after the code block, you must exit the current
flow, whether that's returning from a function or breaking from a loop [[]]

## Match

This pattern matching stuff is really handy, right?!

Well the creators of Rust thought so too, so much in fact, they added a whole control flow mechanism around it!

`match` is a bit like `if` in that it can branch, and act as an expression. 

However, `match` can do a lot more than `if`, it will match against multiple possibilities, allows match guards for fine
grain control of pattern matching, and its exhaustive, meaning that a match _must_ deal with every possibility.

Let's look at our Vector example again: [[]]

---

First of all, you can see that this pattern is _much_ cleaner than having a lot of `if let`s. 

We could also use match as an expression: [[]]

What happens if we add another variant to the enum though? Well, that `match` statement will see that not every case is
handled, and cause an error. [[]]

---

We can deal with this by either adding the missing case, or using underscore, which is a special variable that
immediately discards whatever is put into it and will match anything. [[]]

Patterns on match arms are tested from top to bottom, and you can also match on more specific patterns, like values:
[[]]

There's one more trick up match's sleeve which is match guards. Say we want to do something similar to the above, but
instead of matching on exactly zero, we want to match on values less than 10.

We could make an arm for every variant, or we could use a match guard which is like a mini if statement: [[]]

## Loop

The most basic loop is, well, `loop`.

When you enter a loop, the code inside it will run until it's explicitly told to stop. [[]]

---

This might seem a little bit unhelpful, surely you never want to get trapped inside a loop forever, but actually, we
often want to keep a program or process running inside a loop.

You can manually exit the loop using the `break` keyword. 

Like other languages, you can simply break from a loop, but remember that blocks can be expressions, and this applies to
loops too!

That means we can have a loop that does some work, and once the work is done, break with the value we want to take from
the loop.

---

In this example, we run a loop until we find some cool number (note the use of if let), then break with that value.

The Type of found is a `u64` and by breaking with that value, the Type of the whole loop becomes `u64` too! [[]]

---

Another useful keyword when looping is `continue`. 

Imagine you have a series of things that need to be processed, but you can skip over _some_ of those things.

Here, we'll continuously get images, and run a time-consuming `process_image` function, unless the image is an SVG,
in which can it will skip it. [[]]

---

There's one more neat trick up Rust's sleeve. 

As with most languages, Rust of course supports nested loops, but to aid with things like `break` and `continue` it
also supports labels.

Labels start with a single quote and mark the loop they are for with a colon.

This very contrived example steps through a set of instructions.

Check the book for the code and see if you can work out what will be output

## While

`loop` is great for programs that actually do want to try to keep running forever (or perhaps have many exit conditions)

We often only want to loop over something `while` something is true though.

The `while` loop takes an expression that evaluates to true or false.

The expression is checked at the start of each iteration through the loop, if it's true, the loop will execute. [[]]

---

This is actually not a great way to loop over numbers, imagine if we forgot to add to counter!

Here's a different example where we call a function until we're happy with the result. [[]]

What's really cool though is that you can do all the tricks we've learned above, including pattern matching with
`while let`. [[]]

`while let` is extremely useful, and we'll see it more in the future, particularly when we get to async await.

## For In

A very common reason for looping in software is because we want to loop over every item in a collection and perform the
same set of instructions for each. 

This is where we'd use `for ... in ...`.

For In allows you to step through an `Iterator`, or anything that implements `IntoIterator`, both of which we'll talk
more about in a later video. 

Simply put though, this lets us step over each item in a collection, stream or series of data

Here's an example where we step over every item in an array [[]]

### Range

Another neat Rust type that works really well here is the Range. 

We haven't specifically covered Range yet but if you've been paying attention to the code samples throughout the last 
few videos, you might have spotted a few!

Range's allow you to specify a "closed" or "half open" range of numbers... well...

Actually, Range's allow you to specify a range of anything so long as it implements the traits `PartialEq` and
`PartialOrd`. 

I've personally never seen this done for anything except numbers and characters, but its worth pointing
out. We'll talk more about PartialEq and PartialOrd in a later video.

We write Ranges in the form start dot dot end, where `start` is inclusive and `end` is `exclusive`. 

---

This means that 2 dot dot 5 includes 2 but not 5.

If you want to create a range that includes the final number, prefix that number with `=`, eg 2 dot dot equals 5: [[]]

As mentioned, Range's can be "half open" which means you can get away with specifying only the start or the end.

---

This is where the Type of the start and end really start to matter though. [[]]

A big warning here though: half open Ranges are dangerous when it comes to `for ... in ... ` loops.

Ranges with no start can't be used at all, and Ranges with no end will continue to try to produce numbers beyond the
upper limits of the type being used at which point your program will crash.

---

They're great though, if we just want to do something 10 times. [[]]

## Homework

The best way to learn anything is to practice it. For this section, I'd like you create a program call Fizz Buzz.

In Fizz Buzz we want to run through a series of numbers (say 1 to 100 inclusive). For each number:
- if the number is divisible by 3, print the word Fizz
- if the number is divisible by 5, print the word Buzz
- if the number is divisible by both 3 and 5, print FizzBuzz
- otherwise, just print the number

You can do this a few ways, but you'll need to loop over each number and then choose what to do with it with those
numbers. 

As a starting point, you could use a range to generate the numbers, then use a `for ... in ...` loop to get
each number one at a time, then some `if`/`else` statements to get the output.

Can you work out any other ways to do it?

## Next Time

Next time we're going to look at functions.

We're also going to deal with what many people consider the hardest bit of Rust to deal with, Ownership, move semantics
and lifetimes.

Don't panic, I think... hope... I've got a great way to explain this one to you.

If that sounds interesting, don't forget to like and subscribe

And don't forget to do your homework, if you have any questions, drop me a comment!

And I'll see you next time.
