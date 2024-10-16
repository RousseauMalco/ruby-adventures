# Programming Languages: Ruby Adventures

This assignment asks you to do several programming tasks designed to give you a feel for the Ruby language, and explore the possibilities it creates.

As you work through these tasks, consider: How would you implement this same task in Java? In Python? How does your development experience differ from those other languages? Get beyond the immediate shock of seeing unfamiliar syntax. Where do you find your attention going as you code in this language vs. the others? Why? What language features shape these differences?

(You **do not need to write up answers** to these questions. I am, however, curious to hear your thoughts!)


## Part 0: Getting oriented and set up

This project depends on several third-party libraries, which Ruby calls “[gems](https://rubygems.org).” Like most Ruby projects, this project uses [Bundler](http://bundler.io) to manage its gem dependencies and their versions. Together, Gems and Bundler are a “package manager.” (You may be familiar with other package managers: pip in Python and npm in Javascript. Both of those were directly influenced by RubyGems and Bundler.)

Take a look at `Gemfile`. It specifies which gems this project depends on, where to install them from, and optionally constraints on which version of the gem to use. (Note that `Gemfile` is itself Ruby code!)

Now take a look at `Gemfile.lock`. It specifies which gems bundler actually installed (including indirect dependencies not listed in the gemfile), where bundler got the gems from, and which exact version it installed. Checking this file into git helps ensure that an entire development team is using the same version of all the dependencies, and we don’t get into the situation where the code works for some people but not others.

To install the gems listed in `Gemfile`:

```bash
gem install bundler  # in case you don’t already have it
bundle install       # or you can just type `bundle`
```

Now test your installation:

```bash
bundle exec rake test
```

(The `bundle exec` prefix ensures that Ruby is using the exact version of each gem specified in `Gemfile.lock`. `rake` is a utility for running tasks from the command line. Common uses include running tests, setting up databases, creating test data, etc.)

The tests should run with some skips, but no failures or errors. Look for this at the end of the test output:

```bash
... 0 failures, 0 errors ...
```

If you see that, you’re up and running.


## Part 1: Desugaring

Remember from class that “syntactic sugar” refers to features of a language’s syntax that make code easier to read and write, but do not provide any additional functionality. Ruby uses a lot of syntactic sugar.

Look at `lib/desugaring.rb`. The first method, `all_the_sugar`, contains a tiny snippet of somewhat realistic code from an imaginary web invitation system. (Actual email generation in Ruby on Rails looks very similar to this.)

That snippet of code uses many kinds of syntactic sugar. **Your task is to remove the sugar,** one step at a time. For each method below, remove `implement_me` and replace it with the contents of the previous method in the file minus the particular kind of sugar the comment describes.

Please note that **the desugaring is cumulative**. You should copy the answer for each step forward to the next step, in the order the steps appear in the file. By the end, you should end up with a very different-looking implementation!

You can test that all your desugared versions work correctly by running the project tests, `bundle exec rake test`. I strongly recommend that you **run the tests after each step of the desugaring** before copying your code forward to the next step.


## Part 2: Going Loopless

The methods in `lib/loopless.rb` all work with data about people working on movies. They are all correct, but they all take a very Java-like, loop-based approach to their jobs.

Convert each of the three methods in this file to use Ruby’s functional-stye list processing instead of loops.

When you are done:

- There should be no more calls to `each`.
- Each method should consist of a **single** statement, no variable assignments and no list mutations.
- Each method should have less code than it started with.

You **do not need to worry about efficiency** for this problem. If you get it working, that is sufficient!

Some of the following Ruby methods might help you:

- `map`
- `select`
- `include?`
- `any?`
- `sort_by`
- `group_by`
- `uniq`
- `find_index`
- `with_index`

You will probably find it helpful to experiment with movie test data using Ruby’s interactive mode:

- Open a terminal / Powershell / command line **in the project directory** (cd to it if necessary).
- Type `irb`. You should see a prompt something like `3.0.2 :001 >`. You can type Ruby here and see what it does.
- Now copy and paste these two lines into irb:

      require_relative 'test/movie_test_data'
      include MovieTestData

  The first line reads the Ruby source code containing movie test data, and the second line includes that test code directly in your irb environment.

  (If the first line fails, you are probably in the wrong directory.)
- You can now refer to specific pieces of test data directly in irb. Try typing `people`, `movies`, `maurine`, and `electric_tears` in irb. Look at `test/movie_test_data.rb` to see all the test data.
- Experiment with the test data to help you solve the problems. For example, try:

      movies.map(&:title)
      movies.map(&:year)
      people.select { |person| person.name.include?("y") }
      deane.roles
      deane.roles.map(&:movie)
      deane.roles.map(&:movie).uniq

Once these experiments start to feel comfortable, work on the puzzles in `lib/loopless.rb`. **Remember to run the tests!** You will probably want to keep one terminal window open for your irb experiments, and a second window open for running the tests.

Feeling a bit stumped? Here are hints about each specific problem:

<details>
  <summary>Click for a hint about find_all_in_role:</summary>
  
  > You can do this one with `select`, `map`, and `include?`.
  >
  > <details>
  >  <summary>Click for a more specific hint</summary>
  >
  >  1. Figure out how to get the array of roles one specific person has been in.
  >  2. Figure out how to get the array of _names_ of roles a person has been in.
  >  3. Figure out how to check whether that contains a specific role name.
  >  4. Now apply that test to each person, and select the ones who match.
  > </details>
</details>

<details>
  <summary>Click for a hint about list_movies:</summary>

  > For this one, read about:
  > - `map`
  > - `uniq`
  > - `sort_by`
</details>

<details>
  <summary>Click for a hint about build_credits:</summary>

  > There are several ways to solve this one! Here are methods that could help — and note that while all of them are useful in _some_ possible solution, you will not need all of them in any one _particular_ solution:
  > - `include?`
  > - `map`
  > - `find_index`
  > - `flat_map`
  > - `select`
  > - `sort_by`
</details>
<br>

Ask me for more hints! The purpose is for you to get the feel of this style of working with collections, not for you to puzzle out every detail of an unfamiliar API all alone.

Remember to run the tests early and often.


## Part 3 (optional): DIY Metaprogramming

If the first two parts of the assignment were enough for you to chew on, you can stop here. If you are look for something extra creative and fun and uniquely Ruby-esque, venture onward!

Take a look at at the examples in `lib/metaprogramming`:

- `property.rb`: A simple example that reproduces Ruby’s built-in `attr_accessor` using `define_method`.
- `animal.rb`: A more complex example, also using `define_method`.
- `number_speller.rb`: A different sort of beast that uses `method_missing`. (It’s a bit diabolical. Can you figure out how it works?)

Run each one, and study the code alongside the output. You can run one by typing, for example:

    ruby lib/metaprogramming/property.rb

With each of these examples, it may be easier to start with the bottom section of the code: look at how the metaprogramming is _used_ first, then look at how it is implemented.

Now, add your own example of metaprogramming to `lib/metaprogramming`, in a similar spirit as the existing ones. Feel free to steal any of the existing code for your own effort.

What should your example do? Fool around! Be inventive!
