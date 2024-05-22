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

---

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

---

First, lets make a normal program, remember you can do this with something like: `cargo new iriss-documentation`

In `main.rs`, we'll make a single add function and our main function will just check it works.

We can run this and, if it works, we see no output!

Next, lets migrate the add function to a library. 

---

Create a new file called `lib.rs`. 

In the same way that `main.rs` is the entry to our binary, `lib.rs` is the entry to our library. 

Our library acts like any other module, and the module is named the same thing as our project. 

Anything public in `lib.rs` will be available to anyone using the library.

We'll move our add function to the `lib.rs` and make it public so that we can access it from `main.rs`:

We can now use this function in our main function via the project module (eg, `iriss_documentation`).

---

Note that although our project name  is "iriss hyphen documentation", hyphens can't be used in module names, so we get "iriss underscore documentation".

If we run our program right now, you can see that it still works but... we're not really taking advantage of the library, what's the difference between this and what we had before?

---

The main way you're likely to share code is through Rusts package management system called Crates.

Were we to publish this project right now then theoretically other people could use our `add()` function, pointless as it may be. 

But, we aren't going to cover Crates until much later in the series, after we've got the basics of the language covered.

That doesn't mean this technique isn't useful though. 

To show how it can be useful, lets make another change to our project.

---

Now that we've got our `lib.rs`, lets move `main.rs` to a new folder called `bin`. 

The program still runs if you use `cargo run` but if you look closely there is a difference.

In the last line, you'll see the name of the binary has changed from `iriss-documentation.exe` to `main.exe`. 

This is because when the project was a single binary file, the executable was simply named after the project.

By using the `bin` directory, we're telling Rust we expect to produce multiple binaries from this project, so now it takes the name from the file name instead of the project.

To fix this, rename `main.rs` to `iriss-documentation.rs` and cargo will go back to creating `iriss-documentation.exe`.

You can now create more binaries in this directory called different things and all of them will have access to the library we've created!

Let's quickly try this out with a new bin file called `five.rs`, we'll simply print the output from adding 2 and 3.

Now we have more than one binary, when we use cargo run, we have to specify the binary file to use, in this case `cargo run --bin five`

## rustdoc

The Rust toolset includes a program called `rustdoc` which we can run via Cargo using `cargo doc`. 

It builds and combines the documentation, not only from your code, but from any libraries and crates that you're using.

Let's jump in and try it out with our code...

---

when we run `cargo doc --open` it will build the documentation and then open it in our browser. 

As you can see, without us doing anything rustdoc has figured out about our add function.

If you click the `add` function, it'll take you to the documentation page for that function. 

Rustdoc has already worked out the header for the function, but that's all the information there is for now.

So how do we actually give the consumers of our library the information they need? 

Comments! ... kinda.

## Doc Comments

Comments are used to communicate information to anyone reading the source code, but are completely ignored by the compiler. 

We usually talk about two styles of comments:

Line comments: starting at `//` and ending at the end of the line 

Block comments: starting at `/*` and continuing until `*/`

---

Many languages like Java, JavaScript, PHP, Go, Elixir and many, many more, also support a special type of comment called a Doc Comment.

Like comments, these are used to convey information about nearby code, and are ignored by the compiler.

However, you can use documentation tooling to read them and produce the documentation for you.

Rust takes this idea and turbocharges it... but we'll get to that.

For now, there are two subtypes of Doc Comments in Rust:

Outer Doc Comments and Inner Doc Comments

It is possible to create these comments as either line comments or block comments _however_, the convention is to always use line comments, even if the comment spans many lines. 

The reason for this appears to be ambiguity in terminating block comments with `*/` enhanced by a desire for consistency.

Don't worry though, your IDE should help you deal with the line comments, and if you use a screen reader it should manage just fine too.

Outer Doc Comments are placed before the thing that's being documented and use `///` for each line of the comment, and we'll use them for most things like modules, type definitions, traits and functions... like our `add()` function.

---

Lets add a little description, re-run `cargo doc` and refresh the page in our browser, and our `add` function now has a little explanation.

That's neat, but we could have worked that out from the name. 

Rust documentation uses markdown, which means we can use formatting and code snippets, as well as consecutive lines being treated as a single paragraph.

Let's add some more detail and explain how to use the function using the same technique we learned in the last chapter, assertions.

Doing it this way may seem odd, but all will be explained in a moment.

Rebuilding our documentation now gives us the code sample too, isn't that cute!

What's also cool, is that if we go back to the top level of our library where our `add` function was listed, you can see that `add` now has a nice little summary, it's the first line of our documentation.

This is worth bearing in mind as you write your documentation; try to keep this line terse but descriptive.

---

Before we move on, we should also provide some documentation for our library.

We can't put Outer Doc Comments before the library because the library _is_ the file.

Instead, we use Inner Doc Comments, which are prefixed with `//!`.

We do this at the top of the file. 

Let's just put some random info in for now to show off some other features.

And all of this will render nicely into our documentation.

It's worth pointing out that you _can_ use Inner Doc Comments for all sorts of things... but you shouldn't. 

Again, the convention is to only use Inner Doc Comments for files, whether that's your root library file, or module files like `my_module/mod.rs`, or `./my_module.rs`.

## Doc-Tests

Some people say tests should be the documentation, but here we say the documentation should be the tests!

Well, ok, not _the_ tests, you absolutely should still write your unit tests, etc. 😅

But without doing anything else to our project, lets try running `cargo test`.

There's a few things to notice here.

First, we seem to have four suites of tests. 

Second, we have two passing tests... but we didn't write any tests, did we?

The order of test suites, and individual tests you see may differ from this, but you should have one test suite for our library, two for our binaries, and one for Doc-tests.

The first three have no tests, but Doc-tests have two tests.

What's happened here is that `cargo test` is treating any code samples as tests (though we can ask it not to on a case by case basis).

Let's quickly add a test to `lib.rs` to look more closely at the difference.

Now when we run again we get some interesting comparison.

In the unit test, the test name is the fully qualified path to the test function: `test::test_add`.

In the doc tests, it's the file, the name of the thing being documented (unless it _is_ the file) and the line of the start of the code block.

This makes them slightly less useful _as_ tests, so why do we do it?

---

Well, the main reason we end up with bad documentation, and the reason why people so often don't bother writing it to start with, is that it needs to be maintained.

One of the worst things we can do in documentation is tell someone they can achieve an outcome in a particular way, then later change the code, and not the documentation, so that it no longer works as expected.

Writing our documentation examples with assertions ensures that our documentation is correct, and keeping our documentation right next to the thing being documented makes it trivial to update. 

Rust guarantees that if you said the code works in a particular way, so long as you also wrote an example with assertions that demonstrates that, then the documentation is correct!

Why don't we do this in every language!

## Sharing Documentation

To paraphrase Dr Strangelove: Of course, the whole point of **documentation** is lost, if you keep it a secret!

How do we tell the world?

There's two ways documentation gets shared and neither of them require any work on your behalf.

We'll go into this further when we talk about the Rust ecosystem, however when you're ready to share your code, we use cargo to "publish" the code to crates.io. 

When someone downloads your code from there, they get the source code which includes your documentation.

This means when they build their documentation, they can also build your documentation.

The second way our documentation is shared is that when we publish to crates.io, our documentation is compiled and uploaded to another service, docs.rs. 

On this service you can find every version of API documentation for every crate (library) ever published!

## Summary

So Rust documentation has some really compelling things going on:

1. Documentation tooling comes with the rust tool suite

2. Documentation is written with code, right next to the thing being documented, not separately from it, making it trivial to maintain

3. Code examples are run with tests, meaning it's very hard to produce documentation that's wrong

Furthermore, because the prescribed method of writing documentation is so good, everyone ends up using the same tools and the same conventions.

This means Rust documentation will always feel somewhat familiar, significantly reducing the burden of learning.

It's one of the many examples of how the Rust community feeds into itself, helping us all be better software engineers.

## Homework

Last time I asked you to do fours things, one at a time, starting with the test and then writing the implementation.

I mucked up, and all I can say is I'm sorry.

Literally the second requirement was far harder than I expected. I've taken it out of the book, and mentioned it in the erratum of the last video.

However... let's do this.

I'm going to move fast so pause the video if you need a closer look at something.

First thing we need to do is create a dummy function and use the todo macro so that it panics.

We then create our first test.

Our first requirement doesn't tell us too much, so we'll keep it simple; all lowercase, no punctuation.

When we run the tests they obviously fail. 

Lets implement the code.

I'm going to use our split_around_many function.

If you used the built-in split function instead, well done, bonus points to you!

It's actually more efficient that way, and you can in fact reduce the memory allocations down to just one.

For the purposes of demonstration, I decided to go for simplicity (boy that's going to look silly in a minute).

I made `words` mutable so that I could call `reverse` on it.

This reverses the vector in place rather than returning a new vector.

Finally, we join the vector of strings together with spaces, and return it inside an Ok.

Running the tests show this technique works.

The next requirement was to capitalise the first character if the first character was previously capitalised.

Our initial test already checks the negative of this, so we only need to check that when there is a capital its maintained.

When we run, as expected, the tests fail.

Ok, again, I'm really sorry about this next bit, here we go.

First we have to detect if the first character is uppercase.

We'll use the chars method again to get an iterator, we'll take a single character from it and check that its uppercase.

This on its own would return true if the string was empty so, even though it doesn't matter in our use-case, lets also check the string isn't empty so the variable name is accurate.

Finally, after we've joined the word together, if the first letter was a capital we'll get the first character with get_mut and a range that represents a single byte.

Unlike how we used ranges in our old split function, get_mut is safe it will not split characters, instead it returns an option.

If the first character is a single byte we get a mutable slice reference to that character in the string, we can now make it uppercase.

Running the test shows us that worked.

However, we've now dipped into using iterators and sub slices of the final string, now I want to know if this works on an empty string.

Don't worry if you didn't do this, but I decided to add a null test, one that shouldn't change anything.

Even though we only just added this test, we can see it already passes, phew!

Next lets look at trimming the string.

In the requirements I did say "any whitespace" so lets test with a space on one end and a tab on the other.

When we run the test, we get our expected fail.

This one's easy to fix, string slices have a trim function that returns a subslice inside the string that doesn't include any outer whitespace.

We can shadow the input variable which effectively just replaces it for the rest of the function.

That was all we needed to do (see this is the level of effort I'd anticipated for the homework), so now our tests pass again.

The final requirement was to return an error if there was more than one sentence.

We haven't dealt with punctuation yet so let's create an assertion that an ending dot is fine, and a second assertion that one in the middle of a sentence causes an error

When we run the tests now, its worth noting that the first assertion already passes and the test dies on the second!

I actually wanted the first assertion to make sure I don't _break_ that functionality as I fix the second assertion.

To do this, I'm going to go through the words vector and take all but the last element. If any of these words end in a dot, then we can assume that's the end of a sentence.

If we find that we'll return an Error containing the unit type.

Were we doing this properly, of course we'd use proper errors... but proper errors are for another time 😉

---

This time the homework is much more chill 😅

I'd like you to create a library that contains our three split functions from the last few videos, check the book for sample code to copy if you need to.

You can make a library directly with no executable with `cargo new --lib` and whatever you want to call it. 

This saves you moving things around too much.

Write documentation for the functions that explains how they work with code samples and assertions.

If you use the `split_around_many_recurse` function, remember that it shouldn't be public, so you won't see it in your library documentation.

And that's it!

I don't think we'll even need to go over this one in the next video, it's more for you to practice.

## Next Time

Next time we're going to look at two more tools that come with the Rust tool suite that help us write consistent and high quality Rust code; Rust format and Clippy.

I think these tools are really going to surprise and delight you, so if you're enjoying this series, don't forget to like and subscribe.

I also have some channel news coming up that won't be part of the IRISS series, so keep an eye out for that.

And I'll see you, next time.

## Outro

The whole point of documentation is lost IF YOU KEEP IT A SECRET, WHY DIDN'T YOU TELL THE WORLD EH?
