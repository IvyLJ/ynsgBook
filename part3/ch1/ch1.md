# Functional-Light JavaScript

# Chapter 1: Why Functional Programming?

1. This first chapte:
   (1)  "Why should I use FP style with my code?"
   (2)  "How does Functional-Light JavaScript compare to what others say about FP?"

## At a Glance

Let's briefly illustrate the notion of "Functional-Light JavaScript" with a before-and-after snapshot of code. Consider:

```js
var numbers = [4,10,0,27,42,17,15,-6,58];
var faves = [];
var magicNumber = 0;

pickFavoriteNumbers();
calculateMagicNumber();
outputMsg();                // The magic number is: 42

// ***************

function calculateMagicNumber() {
    for (let fave of faves) {
        magicNumber = magicNumber + fave;
    }
}

function pickFavoriteNumbers() {
    for (let num of numbers) {
        if (num >= 10 && num <= 20) {
            faves.push( num );
        }
    }
}

function outputMsg() {
    var msg = `The magic number is: ${magicNumber}`;
    console.log( msg );
}
```

Now consider a very different style that accomplishes exactly the same outcome:

```js
var sumOnlyFavorites = FP.compose( [
    FP.filterReducer( FP.gte( 10 ) ),
    FP.filterReducer( FP.lte( 20 ) )
] )( sum );

var printMagicNumber = FP.pipe( [
    FP.reduce( sumOnlyFavorites, 0 ),
    constructMsg,
    console.log
] );

var numbers = [4,10,0,27,42,17,15,-6,58];

printMagicNumber( numbers );        // The magic number is: 42

// ***************

function sum(x,y) { return x + y; }
function constructMsg(v) { return `The magic number is: ${v}`; }
```

Once you understand FP and Functional-Light, this is likely how you'd *read* and mentally process that second snippet:

> We're first creating a function called `sumOnlyFavorites(..)` that's a combination of three other functions. We combine two filters, one checking if a value is greater-than-or-equal to 10 and one for less-than-or-equal to 20. Then we include the `sum(..)` reducer in the transducer composition. The resulting `sumOnlyFavorites(..)` function is a reducer that checks if a value passes both filters, and if so, adds the value to an accumulator value.
>
> Then we make another function called `printMagicNumber(..)` which first reduces a list of numbers using that `sumOnlyFavorites(..)` reducer we just defined, resulting in a sum of only numbers that passed the *favorite* checks. Then `printMagicNumber(..)` pipes that final sum into `constructMsg(..)`, which creates a string value that finally goes into `console.log(..)`.

## Confidence

1. a very simple premise(前提) :
   code that you cannot trust <=> code that you do not understand
   =>  if you cannot trust or understand your code, then you can't have any confidence whatsoever that the code you write is suitable to the task.
2. What do I mean by trust?
   I mean that you can verify, by reading and reasoning, not just executing, that you understand what a piece of code *will* do; you aren't just relying on what it *should* do.
3. The techniques that form the foundation of FP are designed from the mindset（思想） :
   having far more confidence over our programs just by reading them. Someone who understands FP, and who's disciplined enough to diligently use it throughout their programs, will write code that they **and others** can read and verify that the program will do what they want.
4. one of the biggest selling points of FP:
   FP programs often have fewer bugs, and the bugs that do exist are usually in more obvious places, so they're easier to find and fix.
   FP code tends to be more bug-resistant -- certainly not bug-proof, though.

## Communication

1. Why is Functional Programming important? <= why programming itself is important.
2. We need to focus a lot more on the readability of our code.
3. If we are going to spend our time concerned with making code that will be more readable and understandable, FP is central in that effort. The principles of FP are well established, deeply studied and vetted, and provably verifiable.
4. FP (at least, without all the terminology weighing it down) is one of the most effective tools for crafting readable code. *That* is why it's so important.

## Readability

*Imperative(命令性)* describes the code most of us probably already write naturally; it's focused on precisely instructing the computer *how* to do something.
Declarative(声明性)code -- the kind we'll be learning to write, which adheres to FP principles -- is code that's more focused on describing the *what* outcome.

Let's revisit the two code snippets presented earlier in this chapter（代码分析）.

The first snippet is imperative, focused almost entirely on *how* to do the tasks; it's littered with `if` statements, `for` loops, temporary variables, reassignments, value mutations, function calls with side effects, and implicit data flow between functions. You certainly *can* trace through its logic to see how the numbers flow and change to the end state, but it's not at all clear or straightforward.

The second snippet is more declarative; it does away with most of those aforementioned imperative techniques. Notice there's no explicit conditionals, loops, side effects, reassignments, or mutations; instead, it employs well-known (to the FP world, anyway!) and trustable patterns like filtering, reduction, transducing, and composition. The focus shifts from low-level *how* to higher level *what* outcomes.

Instead of messing with an `if` statement to test a number, we delegate that to a well-known FP utility like `gte(..)` (greater-than-or-equal-to), and then focus on the more important task of combining that filter with another filter and a summation function.

Moreover, the flow of data through the second program is explicit:

1. A list of numbers goes into `printMagicNumber(..)`.
2. One at a time those numbers are processed by `sumOnlyFavorites(..)`, resulting in a single number total of only our favorite kinds of numbers.
3. That total is converted to a message string with `constructMsg(..)`.
4. The message string is printed to the console with `console.log(..)`.

FP is a very different way of thinking about how code should be structured, to make the flow of data much more obvious and to help your reader follow your thinking. It will take time. This effort is eminently worthwhile, but it can be an arduous journey.

## Perspective

I call the less formal practice herein "Functional-Light Programming" because I think where the formalism (形式主义)of true FP suffers is that it can be quite overwhelming if you're not already accustomed to formal thought.rough much of it.

## How to Find Balance

"YAGNI" : "You Ain't Gonna Need It". This principle primarily comes from extreme programming, and stresses the high risk and cost of building a feature before it's needed.

YAGNI challenges us to remember: even if it's counterintuitive in a situation, we often should postpone building something until it's presently needed.

just because you *can* apply FP to something doesn't mean you *should* apply FP to it.(不需要为"future-proof"做过多复杂设计) 

The best code is the code that is most readable in the future because it strikes exactly the right balance between what it can/should be (idealism-理想主义) and what it must be (pragmatism-实用主义).

## Summary

The heart of Functional-Light ：JavaScriptFunctional programming is about writing code that is based on proven principles so we can gain a level of confidence and trust over the code we write and read.

The goal ：to learn to effectively communicate with our code but not have to suffocate under mountains of notation or terminology to get there.

~~~~
