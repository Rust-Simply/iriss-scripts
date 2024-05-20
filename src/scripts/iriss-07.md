# Documentation

Documentation. Is. Awesome... in Rust.

Ok, good documentation is awesome all the time but Rust provides tools that make documenting your project a truly magical experience.

This series has a free accompanying book, in which you can find all the details for this tutorial, check the description below.

My name is Daniel, welcome to IRISS.

## Intro

Documentation is important. 

You'll hear a lot of things in Software like "the code should document itself" and "the tests should document the code", and this is all true, but these aren't _substitutes_ for actual documentation.

Self explaining code is incredibly important when you work in a code base so that you can easily understand it and maintain it. 

And, as we spoke about in the last video, tests are important to show our expectations of what the code will do when used.

But code and tests are not there primarily to teach someone how to use the code. 

They can do that, but its secondary, meaning that someone looking at them for the purpose of learning how to consume the code, is having to deal with a lot of information they don't need.

As software engineers, a huge part of our role is communication, and good documentation conveys to others, how to use our work as quickly, and as painlessly as possible. 

Rust understands this and provides a full suite of tools that make producing the precious resource that is good documentation, not just trivial, but also fun!

## Libraries

In order to show the value of documentation, we're going to create our first Rust library.

Up to now, we've been building binaries. 

`rustc` compiles `main.rs`, and any other Rust code that it uses, into an executable, but our projects can link to, and use functionality from, libraries. 

This is particularly helpful when sharing and reusing code. 

We'll be talking much more about how we share code with each other when we cover the Rust ecosystem, but for now, we can leverage the power of libraries for code reuse within the same project.

Let's try it out. 

First, lets make a normal program, remember you can do this with something like: `cargo new iriss-documentation`

In `main.rs`, we'll make a single function and our main program will just check it works.

We can run this and, if it works, we expect no output!

Next, lets migrate the add function to a library. 

Create a new file called `lib.rs`. In the same way that `main.rs` is the entry to our binary, `lib.rs` is the entry point to our library. 

Our library acts like any other module, and the module is named the same thing as our project. 

Anything public in `lib.rs` will be available to anyone using the library.

We'll move our add function to the `lib.rs` and make it public so that we can access it from `main.rs`:

We can now use this function in our main function via the project module (eg, `iriss_documentation`).

Note that although our project name  is "iriss hyphen documentation" with a hyphen, hyphens can't be used in module names, so we get "iriss underscore documentation".

If we run our program right now, you can see that it still works but... we're not really taking advantage of the library though, what's the difference between this and what we had before?

The main way you're likely to share code is through Rusts package management system called Crates.

Were we to publish this project right now then theoretically other people could use our `add()` function, pointless as it may be. 

But, we aren't going to cover Crates until much later in the series, after we've got the basics of the language covered.

That doesn't mean this technique isn't useful though. To show how it can be useful, lets make another change to our project.

Now that we've got our `lib.rs`, lets move `main.rs` to a new folder called `bin`. 

The program still runs if you use `cargo run` but if you look closely there is a difference.

In the last line, you'll see the name of the binary has changed from `iriss-documentation.exe` to `main.exe`. 

This is because when the project was a single binary file, the executable was simply named after the project.

By using the `bin` directory, we're telling Rust we expect to produce multiple binaries from this project, so now it takes the name from the file name instead of the project.

To fix this, rename `bin/main.rs` to `bin/iriss-documentation.rs` and cargo will go back to creating `iriss-documentation.exe`.

You can now create more binaries in this directory called different things and all of them will have access to the library we've created!

Let's quickly try this out with a new bin file called five.rs, we'll simply print the output from adding 2 and 3.

Now we have more than one binary, when we use cargo run, we have to specify the binary file to use, in this case `cargo run --bin five`

## rustdoc

The Rust toolset includes a program called `rustdoc` which we can run via Cargo using `cargo doc`. 

It's builds and combines the documentation, not only from your code, but from any libraries and crates you're using.

Let's jump in and try it out with our code, when we run `cargo doc --open` it will build the documentation and then open it in our browser. 

As you can see, without us doing anything rustdoc has figured out about our add function.

You'll notice that it's only documented our library. This is because people consuming your code can't use code in the binary.

If you click the `add` function, it'll take you to the documentation page for that function. 

Rustdoc has already worked out the header for the function, but that's all the information there is for now.

So how do we actually give the consumers of our library the information they need? 

Comments! ... kinda.

## Doc Comments

Comments are used to communicate information to anyone reading the source code, but are completely ignored by the compiler. 

We usually talk about two styles of comments:

Line comments: starting at `//` and ending at the end of the line 

Block comments: starting at `/*` and ending at `*/`

Many languages like Java, JavaScript, PHP, Go, Elixir and many, many more, also support a special type of comment called a Doc Comment.

Like comments, these are to convey information about nearby code, and are ignored by the compiler.

However, you can use documentation tooling to read them and produce the documentation for you.

Rust takes this idea and turbocharges it... but we'll get to that.

For now, there are two subtypes of Doc Comments in Rust:

Outer Doc Comments 

Inner Doc Comments

It is possible to create these comments as either line comments or block comments _however_, the convention is to always use line comments, even if the comment spans many lines. 

The reason for this appears to be ambiguity in terminating block comments with */ enhanced by a desire for consistency.

Don't worry though, your IDE should help you deal with the line comments, and if you use a screen reader it should manage just fine too.

Outer Doc Comments are placed before the thing that's being documented and use `///` for each line of the comment, and we'll use them for most things like modules, type definitions, traits and functions... like our `add()` function.

We can re-run `cargo doc` and refresh the page in our browser and our `add` function now has a little explanation.

That's neat, but we could have worked that out from the name. 

Rust documentation uses markdown, which means we can use formatting and code snippets, as well as consecutive lines being treated as a single paragraph.

Let's add some more detail and explain how to use the function using the same technique we learned in the last chapter, assertions.

Doing it this way may seem odd, but all will be explained in a moment.

Rebuilding our documentation now gives us the code sample too, isn't that cute!

What's also cool, is that if we go back to the top level of our library where our `add` function was listed, you can see that `add` now has a nice little summary, it's the first line of our documentation.

This is worth bearing in mind as you write your documentation.

Try to keep this line terse but descriptive.

Before we move on, we should also provide some documentation for our library.

We can't put Outer Doc Comments before the library because the library _is_ the file.

Instead, we use Inner Doc Comments, which are prefixed with `//!`.

We do this at the top of the file. Let's provide a little

And all of this will render nicely into our documentation.

It's worth pointing out that you _can_ use Inner Doc Comments for all sorts of things... but you shouldn't. 

Again, the convention is to only use Inner Doc Comments for files, whether that's your main library file, or module files (eg, `my_module/mod.rs`, or `./my_module.rs`).

## Doc-Tests HERE

Some people say tests should be the documentation, but here we say the documentation should be the tests!

Well, ok, not _the_ test, you absolutely should still write your unit tests, etc. 😅

But without doing anything else to our project, try running `cargo test`.

![Doc-Tests](documentation/doc-tests.png)

There's a few things to notice here.

First, we seem to be getting three sets of tests. Second, we have two passing tests... but we didn't write any tests, did we?

The order of test suites, and individual tests you see may differ from the above, but you should have one test suite for our library (`lib.rs`), one test suite for our binary (`iriss-documentation.rs`), and one for Doc-tests.

The first two have no tests, but Doc-tests have two tests.

What's happened here is that `cargo test` is treating any code samples as tests (unless we ask it not to).

Let's quickly add a test to `lib.rs` to look more closely at the difference.

Now when we run again we get some interesting comparison.

Unit Tests:

![Unit Tests](documentation/doc-tests-comparison-unit.png)

Doc Tests:

![Doc Tests](documentation/doc-tests-comparison-doc.png)

In the unit test, the test name is the fully qualified path to the test function (`test::test_add`)

In the doc tests, it's the file, the name of the thing being documented (unless it _is_ the file) and the line of the
start of the code block.

This makes them slightly less useful _as_ tests. So why would we do it?

Well, the leading cause of bad documentation, and the reason why people so often don't bother writing it to start with is that it needs to be maintained.

One of the worst things we can do in documentation is tell someone they can achieve an outcome in a particular way, then change the code so that particular way no longer works as expected.

Writing our documentation as test ensures that our documentation is correct, and keeping our documentation right next to the thing being documented makes it trivial to update. 

Rust guarantees that if you said the code works in a particular way, so long as you also wrote an example with assertions, then the documentation is correct!

## Sharing Documentation

To paraphrase Dr Strangelove: Of course, the whole point of **documentation** is lost, if you keep it a secret!

How do we tell people about our documentation?

There's two ways documentation gets shared and neither of them require any work on your behalf.

We'll go into this a further in the ecosystem section of the book, however when you're ready to share your code, we use cargo to "publish" the code to [crates.io](https://crates.io). 

When someone downloads your code from there, they get the source code which includes your documentation.

This means when they build their documentation, they can also build your documentation.

The second way our documentation is shared is that when we publish to crates.io, our documentation is compiled and uploaded to another service, [docs.rs](https://docs.rs). 

On this service you can find every version of API documentation for every crate (library) ever published!

## Summary

So Rust documentation has some really compelling things going on:

1. Documentation tooling comes with the rust tool suite

2. Documentation is written with code, not separately from it

3. Code examples are run with tests, meaning it's very hard to produce documentation that's wrong

Because the prescribed method of writing documentation is so good, everyone ends up using the same tools and the same conventions, which means Rust documentation will always feel somewhat familiar, significantly reducing the burden of learning.

It's one of the many examples of how the Rust community feeds into itself, helping us all be better software engineers.
