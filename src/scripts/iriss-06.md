# Tests

Tests. Are. Awesome.

They are my favourite bit of software engineering. 

I'm sure many of you can relate, but I don't tend to trust humans telling me I'm doing well at my job.

When a computer tells me I'm doing well at my job because I made a test pass though, oh that dopamine hits good.

A good test makes sure that the thing we're building does what we think it should do.

## Intro

Anecdotally I recently interviewed at a company where I professed my love of tests, and they told me flatly they don't write tests, they "move fast". 

Later in the interview they admitted they were having morale issues because their engineers were constantly getting called out of hours to fix things.

So, it begs the question: are you moving fast if you're writing software that doesn't work?

Software engineers are not paid to write software, we're paid to solve problems. 

Tests are what make sure we solved the problem and by automating our tests they make sure we don't accidentally "unsolve" it further down the line.

## The testing Pyramid

There are many ways to test software, these largely fall into three categories that make up what we call the testing pyramid.

It's a pyramid to indicate that, although all tests are important, those lower down the pyramid should be laying the foundation for the others. 

Not only should you have more of them, but they will provide the greatest feeling of safety and security.

### End-to-End Tests

E2E tests are designed to make sure that the user of the software you've created can complete full user journeys

For example, can the user open, edit and save a file in a desktop application or can a user add an item to a shopping cart and checkout of an ecommerce store.

End-to-End tests are the slowest form of test and lack depth, but they help add confidence that everything works together.

### Integration Tests

Integration tests check that the code you control works correctly with code that your program depends on that you don't control. 

This would include things like databases or other data stores, web apis, library apis, etc.

Integration tests are also used if your software produces a public API.

You write tests to check that people using your software in its more natural state.

Because of the communication component to these tests, these tests are also quite slow.

### Unit Tests

Unit tests test a single unit of functionality. 

These tests are the simplest, fastest and should make up the bulk of your testing.

These tests are so important, that it is best practice to write them before you write the code you're going to test.

For this book, we're only going to cover Unit Tests. 

That isn't to say that Integration Tests and End-to-End Tests aren't important, they absolutely are, and there are many good guides out there.

But, it is to say, that Unit Tests are _so_ important, that they significantly impact how we communicate about Rust code.

This is particularly important for documentation which we'll talk about in the next video.

## Introduction to Modules

Unlike many languages, in Rust, tests live with the code that they're testing.

To explain this we need to talk about how code in Rust is organised with Modules.

A Module is simply a container for other things, functions, type definitions, other modules, etc.

You could think of it like a physical container, though you can nest any number of containers together.

The contents of the module are private to that module unless explicitly marked as public with the `pub` keyword.

We define a module with the `mod` keyword and a name. There are then three ways to define what's inside that module:
1. With a directory named the same thing as the module which contains the file `mod.rs`, eg `my_module/mod.rs`
2. With a file in the same directory named the same thing as the module, eg `my_module.rs`
3. Inside curly brackets, eg `mod my_module { ... }`

If the module exposes anything publicly, you can then reference them with the path to the module and the name of the thing you're referencing separate by double colons. 

Sound familiar?

It should, this is how we've been accessing Rust's standard library. For example, the `stdin` function is inside the `io` module, which itself is available inside the `std` library.

We access that function using `std::io::stdin()`. 

We can also use the `use` keyword to simplify this a bit, for example, we can "use stud io standard-in"

Then we can use standard in without having to use the full path to the module each time.

## Test Modules

In Rust, we typically create a test module near the code that is being tested. 

Let's say we want to test some of the functions we wrote in the last video. 

I've renamed them a bit, so we can have all of them in one file.

First we start by creating a module to test these functions in the same file as the functions exist

As long as nothing in the `tests` module is used in your main program it shouldn't appear in your final binary, however, this isn't good enough. 

There's a risk we might make a mistake of course, but ever without that, the module will still be processed by the compiler in order to do things like type checking.

We only care about this module when we're running our tests and Rust provides us a way to tell the compiler that too, the `cfg` attribute.

Attributes are one of Rusts many meta programming tools which we'll cover more in the future at increasing difficulty levels.

For now, the `cfg` attribute allows us to tell the Rust Compiler (`rustc`) _when_ we want to compile something.

There are many, many ways to use conditional compilation, but for tests its pretty simple, we only want the module compiled when we're building tests.

The `cfg` has a "predicate" to identify this simply called `test`.

So by adding the `cfg` attribute with the `test` predicate, rustc will ignore the module unless its explicitly building the tests.

