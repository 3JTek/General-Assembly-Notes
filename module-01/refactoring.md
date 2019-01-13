# Refactoring

![](https://www.allaboutcircuits.com/uploads/articles/red-green-refactor.png)

Generally speaking, when attempting to solve a problem with code, there is a phase of discovery and trial and error. This phase is very important to help the developer understand the problem and to "sketch" out an initial solution.

Once a solution has been reached it is very tempting to move on to the next problem. This is especially the case when there is an imminent deadline approaching.

However the next phase in the development process should always be **refactoring**.

Refactoring is the process of improving a piece code without changing its functionality.

We can improve code is several ways. We can make it:

* easier to read
* more concise
* more reusable
* more efficient

Let's look at some common ways of refactoring our code:

## Using variables to _cache_ data

"Caching" means to store something in memory. Let's say for example that we want to use a DOM element in multiple places in a script. It's far better to store an element in a variable and access it from there, than to continually get it from the DOM with JavaScript.

When declaring our variables, we need to make sure that we use meaningful names. This helps our code to be more **readable**. `player1Choice` is much easier to understand than `p1c`.

## Large, anonymous functions are bad

An important concept when designing software is modularity and reusability. Having one long function is bad. It makes debugging difficult. It is also difficult for other developers to read, which makes it harder to work in teams.

Splitting large blocks of functionality into smaller functions means that the software is more flexible. **A good function does one thing and does it well.** Smaller functions can also be more easily tested and refactored.

## Write code as if someone else is going to read it

Sometimes when trying to solve a problem we can get tunnel vision, pushing on because we can sense the solution is so close we can almost taste it. When we finally reach a solution, it's important to take a step back and look at the code that we've written with a fresh pair of eyes.

Would a colleague be able to understand our code? Do the variable names make sense? Now that we've found the solution, is there a simpler way to get there?

**Remember**: the simpler the code, the easier it is to understand, and the easier it is to maintain, and the less likely it is to be buggy.

## Making code more efficient

Generally code efficiency should be low on the priority list when developing software. Improving efficiency can improve performance of our code, which can ultimately save money by serving smaller files to our users, and serving more users with less hardware.

It's important to understand that code efficiency should only be considered once a solution has been realised. Improving efficiency can take a huge amount of development time, so we must consider the cost implications. Generally speaking clearly written, flexible, concise code will also be reasonably efficient.

## Practice, practice, practice

Learning how to write good code takes time and experience. Always remember to take the time to refactor your code once you have something that works, and encourage your peers to review your code, and get comfortable with hearing constructive criticism. The more advise you take on board, the quicker you will improve.

Online resources like [https://www.codewars.com/](Code Wars) and [http://exercism.io/](Exercism) are very useful to see different approaches to problems from other developers.

## Further reading

* [Practical Tips for Improving Your Code](https://code.tutsplus.com/articles/practical-tips-for-improving-your-code--wp-28228)
* [9 Effective Tips for Improving Code Quality](http://www.xmcgraw.com/9-effective-tips-for-improving-code-quality/)
* [10 Tips for Improving the Readability of Your Code](https://dzone.com/articles/10-tips-how-to-improve-the-readability-of-your-sof)
