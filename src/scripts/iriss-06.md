# Tests

Tests. Are. Awesome.

They are my favourite bit of software engineering. 

I'm sure many of you can relate, but I don't tend to trust humans telling me I'm doing well at my job.

When a computer tells me I'm doing well at my job because I made a test pass though, oh that dopamine hits good.

A good test makes sure that the thing we're building does what we think it should do.

This series has a free accompanying book, in which you can find all the details for this tutorial, check the description below.

My name is Daniel, welcome to IRISS.

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

End to End tests are designed to make sure that the user of the software you've created can complete full user journeys

For example, can the user open, edit and save a file in a desktop application or can a user add an item to a shopping cart and checkout of an ecommerce store.

End-to-End tests are the slowest form of test and lack depth, but they help add confidence that everything works together.

### Integration Tests

Integration tests check that the code you control works correctly with code that your program depends on that you don't control. 

This would include things like databases or other data stores, web apis, library apis, etc.

Integration tests are also used if your software produces a public API.

Because of the communication component to these tests, these tests are also quite slow but you should use more of them than end-to-end tests.

### Unit Tests

Unit tests test a single unit of functionality. 

These tests are the simplest and the fastest and should make up the bulk of your testing.

These tests are so important, that it is best practice to write them before you write the code you're going to test.

For this video series, we're only going to cover Unit Tests. 

That isn't to say that Integration Tests and End-to-End Tests aren't important, they absolutely are, and there are many good guides out there.

But, it is to say, that Unit Tests are _so_ important, that they significantly impact how we communicate about Rust code.

This is particularly important for documentation which we'll talk more about that in the next video.

## Introduction to Modules

Unlike many languages, in Rust, tests live with the code that they're testing.

To explain this we need to talk about how code in Rust is organised with Modules.

A Module is simply a container for other things, functions, type definitions, other modules, etc.

You could think of it like a physical container, though you can nest any number of containers together.

The contents of the module are private to that module unless explicitly marked as public with the `pub` keyword.

We define a module with the `mod` keyword and a name. There are then three ways to define what's inside that module:
1. With a directory named the same thing as the module which contains the file `mod.rs`
2. With a file in the same directory named the same thing as the module
3. Inside curly brackets

If the module exposes anything publicly, you can then reference those things with the path to the module and the name of the thing you're referencing separate by double colons. 

Sound familiar?

It should, this is how we've been accessing Rust's standard library. For example, the `stdin` function is inside the `io` module, which itself is available inside the `std` library module.

We access that function using stud :: io:: standard in. 

We can also use the `use` keyword to simplify this a bit, for example, we can "use stud io standard-in"

Then we can use standard in without having to use the full path to the module each time.

## Test Modules

In Rust, we typically create a test module near the code that is being tested. 

Let's say we want to test some of the functions we wrote in the last video. 

I've renamed them a bit, so we can have all of them in one file.

First we start by creating a module to test these functions in the same file as the functions exist

As long as nothing in the `tests` module is used in your main program it shouldn't appear in your final binary, however, this isn't good enough. 

There's a risk we might make a mistake of course, but even without that, the module will still be processed by the compiler in order to do things like type checking.

We only care about this module when we're running our tests and Rust provides us a way to tell the compiler that too, the configuration attribute `cfg`.

Attributes are one of Rusts many meta programming tools which we'll cover more in the future at increasing difficulty levels.

For now, the `cfg` attribute allows us to tell the Rust Compiler (`rustc`) _when_ we want to compile something.

There are many, many ways to use conditional compilation, but for tests its pretty simple, we only want the module compiled when we're building tests.

The `cfg` has a "predicate" to identify this simply called `test`.

So by adding the `cfg` attribute with the `test` predicate, rustc will ignore the module unless its explicitly building the tests.

## Writing Tests

Now we're ready to write our first test.

A test is simply a function that we mark with another attribute `#[test]`.

This attribute tells Rust to run this function as part of the test suite when we run `cargo test`.

---

Here's a test that won't pass.

The `assert!()` macro takes either one or two parameters. 

The first parameter which is not optional is a boolean value, or something that evaluates to a boolean.

If this boolean is false, then the assertion will cause a panic, and the test will fail.

The second, optional parameter allows us to annotate the assertion, which can help us more easily determine which (if any) assertion failed in a test.

This is awesome when we create more thorough tests with more assertions. 

You will find that people don't use this as much, I'm guilty of this too, but I do recommend making an effort to describe each specific assertion. 

The people you work with, as well as future you, will appreciate the effort.

---

There are three main assert macros:
- `assert` asserts value is true or panics with optional message
- `assert_eq` asserts left is equal to right or panics with optional message
- `assert_ne` asserts left is NOT equal to right or panics with optional message

There are a couple of restrictions with the assert macros. 

Values used must implement `PartialEq` and `Debug`. 

Most built in types already implement both, and we'll talk about how to implement them for your own types in the Traits video.

To run tests in our project we use `cargo test`. 

When we do that for this test, we should see the this.

----

This top bit is a summary of all the tests, then we get a breakdown of each test that failed.

You can see that we get both the line where the test failed in addition to our failure message.

The line number is why a lot of people skip the message, but it can still be useful to help provide context before we need to look up the line.

## Testing our code

Let's move on to writing our actual tests, we'll start with `split_at`.

Before we can write a test, `split_at` is not part of the `tests` module, so we need to make it available inside.

We can do that with the `use` statement in one of two ways, either `use super::split_at` or `use super::*`.

The `super` keyword simply means the module above this one, which for your unit tests should be the module you're writing tests for. 

We can either bring just the one function in, or we can bring in everything available in that scope.

The idiom here is to `use super::*` as it will bring in everything that needs testing, and hopefully even if it brings in things you aren't using it won't pollute the test module too much. 

---

With our test lets use "Hello world!" as the input string,

and we'll split at 3.

So we can expect the left side of the split to be "Hel",

and the right side to be "lo world!"

I've added some context to our asserts, so we can quickly see if anything went wrong.

But when we run `cargo test`, everything passes, which is great!

But because we're working with strings, we should check strings that contain more than just ascii characters.

Let's write another test for `split_at`. 

It's exactly the same but we've used "Hello World" in Japanese... hopefully, let me know if I got it wrong in the comments.

Now when we run the tests, this test fails.

This is where testing gets really nuanced, we need to test our expectations of the behaviour, not necessarily that the code did what we programmed it to.

In the test, I wrote that I expect the result to be the first 3 characters, but last week, I mentioned this would return a length in bytes.

The Japanese character "ko" is 3 bytes in length, which is why its the only character thats returned.

If we asked for two bytes instead, the split would fall inside a character and this function would panic and crash our program.

Regardless, from the name `split_at` I think number of characters is the right behaviour here, so lets fix our function.

Don't worry too much about this too much yet, we're going to cover iterators in a future video but to explain the fix:

`.chars()` returns an iterator over each character in the string.

We then use `.take()` to only take (at most) the number of characters we wanted to split at.

We then `.map()` over each character and get its size in bytes, which turns the character into a usize number.

Finally we `.sum()` the iterator which adds every number in the iterator together.

This turns the length in chars into a new length in bytes and will be correct regardless of what size in bytes those characters are.

This is now safe to use to create our sub slices as the byte count won't fall inside a character.

We no longer need to check the bounds of `split_at` either, because the maximum size will be the full length of the string even if split_at was longer than the string.

Let's rerun our tests...and now this test works too!

---

Let's quickly write the tests for `split_around` and `split_around_many`.

You'll notice that these functions already work with multibyte strings.

This is because we're looking for the start and end of substrings so we already know that the substrings won't happen in the middle of a character.

Now all our tests are working, doesn't that feel great!

But... what if I told you: we just did all of this backwards 😲

## Test Driven Development

Hopefully, you're eager to write a load of code and then write a load of tests, but wait!

As we alluded to at the beginning of the video, and again halfway through, the point of tests isn't to test that your code does what you think it does, it's to make sure it does what it's supposed to do.

As our `split_at` function showed, its a good idea to decide what you think that function should do before you write it.

The best way to achieve this is to work out what your code is supposed to do, then write the test, then write the code.

This is called Test Driven Development. Let's try it out!

We'll create a function that checks if a given string is a palindrome (a word that's the same forwards and backwards).

We'll start by writing our test.

We'll check whether the function flags "kayak" and "racecar" as palindromes, and flags "wood" as not a palindrome.

Before we can run the test, Rusts type system won't let us compile the program without the function existing.

We don't want to write the functionality yet so we'll create the function header and use the `todo!()` macro

`todo()!` is a handy little marker that we use to say we are intending to deal with this soon.

It always panics so Rust knows our function doesn't need to return anything.

We can run our tests now and see that they fail, lets write the code.

---

Like earlier, we'll get an iterator for each character in the string.

We'll clone this iterator and reverse it to get a second iterator that goes backwards through the string.

Again, don't worry too much about the hows and whys of iterators yet, just go with it.

We can zip the two iterators together, and ccompare each element inside them.

If every element in each iterator is equal to the other, then the word is the same forwards and backwards.

---

Now when we run our tests they pass.

We can now submit our code to a colleague for review.

[Exit left]

---

[Enter right]

Hi Indra, sorry to interrupt, could you review my code?

Looks good to me but while race car is a palindrome, its two words, not one.

Ah, right you are. Thanks for that.

[Exit right]

---

[Enter left]

So lets update our test.

All we need to do is add a space to race car.

Now the test fails again, lets fix it.

We'll update our first iterator to filter out anything thats not alphanumeric. 

That'll deal with whitespace and any other punctuation too.

We only need to update the forward iterator because the effect of the filter is cloned.

Our tests pass again.

Lets go back to the review

[Exit left]

---

[Enter right]

Hi Indra, how does it look now?

I should have thought of this earlier, but names like Anna are also Palindromes, we should check for that.

Good idea, be right back.

[Looks gag?]

[Exit right]

---

[Enter left]

Lets update our test again. And again our tests fail, that's good! Lets update our code.

Now the obvious thing to do here is to lowercase all of the letters BUT in some languages, an uppercase letter might have multiple lowercase variants depending on how its used.

If we map over each character in the string and lowercase it, we actually get another iterator that contains each possible lowercase letter.

This means we now have an iterator of iterators. 

We can get back to an iterator of characters by using the flatten method.

And our tests pass again.

This function still isn't perfect, honestly, but we're going to leave it here.

If you want to keep working on it, its worth looking at diacritics as well as the difference between balanced and unbalanced palindromes.

You may need to rely on external crates for more nuanced control of unicode characters.

For Test Driven Development, the main thing I want to emphasize is that at each stage:
- we think about what we want to achieve
- write or modify a test to represent that expectation
- then work on the functionality until the test passes

## How many tests?

The perennial of questions when it comes to testing: How many tests should a good test writer write if a good test writer could write good tests?

The important thing here is "coverage".

We usually measure coverage by lines.

Each time we run a test, we can look to see if a line was run, if it was we call that "covered".

---

Take for example this really silly function.

If we only test this function by giving it a number greater than 0, we'll only "cover" the line first branch of the `if`, we miss the fact there is a mistake in the second branch.

---

So what percent coverage should you aim for?

Anecdotally, when I was creating my own API framework in PHP, I decided I wanted to get 100% coverage, that is, every line should have a test that hits it. 

---

The very last line that was uncovered was, in it entirety, this.

I wondered if it was worth the effort, decided it was.

The reason this was the last uncovered line was that it was part of a nested `if`.

I'd tested what happens if you went into both `if`s, what happens if you didn't go into the first, but not what happens if you went into the first, but not the second.

---

I wrote the test and... found a bug so severe that I had to rewrite almost a third of the framework.

Should have written the tests first, right?

My personal feelings are that you as an engineer should strive for 100% coverage of your code.

As a manager or engineering lead though, test coverage is a terrible metric. 

Test coverage doesn't tell you if the test was any good.

If you make arbitrary metrics like this, you're not improving code quality, engineers will write tests that meet that metric, but don't for-fill the reason we want tests in the first place which is to answer: "does this code do what we want it to do?".

As an engineer, it's a matter of pride. Get your code right now, and you won't have to deal with it later.

As a leader, make sure your engineers are encouraged to be the best they can.

That means giving them the time to write tests first, giving them access to the resources they need to learn how to write the best tests, etc.

Homework
--------

We've already let on how you can solve last chapters homework

We only needed one lifetime to represent the link between the input string slice and the string slices inside our vector.

---

For a cleaner API though you could keep our recursive function private and expose a public function that creates the
vector for us. 

Because we have control of the vector in this example we can make sure we create a vector with a capacity that is at least as large as the maximum possible entries in it. 

This is useful as when you create a new Vector in Rust it has a default size, and any time you try to add an item to a vector that is already full, Rust will allocate the memory for a new larger vector in the background, copy the data from the old location to the new location, then free the memory in the old location.

This is very time-consuming.

---

For the homework this time, I would like you to write the tests for, and then implement the code for the following requirements in this order:

1. Create a function that will reverse the words in an English sentence.
2. If the string started with a capital letter, it should still start with a capital after the words are reversed.
3. If the string starts or ends with whitespace, it should be removed (trimmed) from the returned String.
4. If the string contains more than one sentence, the function should return an error, though for now, that error can be the unit type.

---

Your function will need to allocate memory and should probably have a header like this.

I've linked to the documentation for String and Result in the description of the video which should help.

# Going forward

As I mentioned, after this chapter we will be talking about Rust differently. 

This will primarily be by using the assert macros to state what a value _should_ be rather than printing it to the screen.

For example, one of our earlier code examples looks like this.

We printed the values of x and y to the screen.

From now on, we'll write our examples like this, which, now you've seen the assert macros is cleaner and easier to understand.

# Next time

This time we've covered one of my favourite Rust features, next time we're going to cover my number one favourite: Documentation.

The Documentation tooling in Rust is so good, it's going to make you wish every language did the same thing!

If that's exciting for you, don't forget to like and subscribe.

If you need any help with the homework, please do comment! 

I'll also be setting up a Discord server in the near future so keep an eye out for that!

And I'll see you next time.


