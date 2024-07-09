# Getting Started

[Jelly](https://github.com/DennisMitchell/jelly) is a "golfing language" created by [DennisMitchell](https://github.com/DennisMitchell). It is a programming language whose specific purpose is to be as short as possible when completing a task. This means that it completely abandons normal standards of "good programming" in favour of brevity. Contructs that you might be familiar with, such as `while` or `for` are implemented as single characters (`¿` and `¡` respectively) in order to save bytes, and Jelly's builtin library is much more expansive than most production languages, but in different areas. For example, Jelly has no file reading or writing capabilities, but can test if an integer is prime in one byte: `Ż`

In order to fully expand the number of builtins in it's library, Jelly uses a [custom code page](https://en.wikipedia.org/wiki/Code_page) to encode it's programs. At the base of it, all Jelly programs are collections of hex bytes. For example, the shortest `Hello, World!` program is expressed as the following hex: `fe 33 d4 61 ea 3b 1e fb`. We could encode this using Unicode, but it uses a few unprintable characters: `þ3Ôaê;û`. Instead, we encode them using [Jelly's code page](https://github.com/DennisMitchell/jellylanguage/wiki/Code-page) so that programs look "nicer": `“3ḅaė;œ»`

You can run Jelly in two ways. To run it locally, follow the [instructions in the `README` page of the repository](https://github.com/DennisMitchell/jellylanguage/blob/master/README.md). Use the `u` flag when running locally, unless you are using a hex editor to change the program byte by byte. Alternatively, you can run it on [Try it online!](https://tio.run/#jelly), an online testing environment also created and hosted by DennisMitchell.

# The Basics

In all likelihood, Jelly will be unlike most programming languages you've used before. The weird characters will start to make sense very quickly. However, the way you program in Jelly is different to most languages. There is no such thing as "variables", and the typical imperative style of Python or Java doesn't work for most Jelly programs. Instead, Jelly uses a "tacit" memory model, similar to APL's trains.

Tacit programming works by keeping track of 3 values throughout the program: the **left** argument, the **right** argument and the **running value**. However, the program itself very rarely refers to these values explicitly. Instead, it *tacitly* passes the appropriate values to the next part of the program, and so on until the end. By composing **links** into **chains**, which then operate on these values, you create a Jelly program.

There are some crucial concepts and terms that need to be introduced first though.
 
## Atoms
 
These are, as their name suggests, the fundamental building blocks of a Jelly program. Each atom takes either 0, 1 or 2 arguments and returns a single value based on those arguments. For example, `H` (halve) is an atom that takes one argument on its left side and returns that argument halved, and `+` is one that takes 2 arguments (one on the left and one on the right) and returns their sum.
 
## Arity

Each atom has a fixed *arity*, defined as the number of arguments it takes. The arity of `H` for instance is 1. Jelly parses programs depending on the patterns of the arities of each part of the program, and by changing the patterns, we can change the program. Something with an arity of 0 is said to be "niladic" or a "nilad", an arity of 1 is "monadic" or a "monad" ([no relation](https://en.wikipedia.org/wiki/Monad_(functional_programming))) and an arity of 2 is "dyadic" or a "dyad"

## Quicks

Atoms are "in-out" style functions - they pass values in and get a value out. Aside from a few specific ones, they are completely determinitic and fixed state. However, that means it's difficult to do a lot of things. For example, there's no "atomic" way to do a while loop. Atoms take in values as arguments, but loops use *code*. This is what **quicks** are for. A quick takes a number of atoms as argument and modifies their behaviour. The most basic of these is `€` or Each. `H` halves a number, but what if we want to halve each number in a list? We use `H€` (read as "halve each"). The `€` modifies the behaviour of the atom directly before it to map over its argument instead.

## Links

By introducing quicks, we now have a bit of a problem when referring to parts of a program. For example, in `H€`, we know that `H` is an atom, and `€` is a quick, but what are they together? They are a **link**. Links are what make up a Jelly program, and are one of the following:

- A lone atom. For example, `H` by itself is a link. `H+²` is a program made up of 3 links, `H`, `+` and `²` (square)
- An atom and a quick. `H€` is also a link. So `²€H€` is 2 links. Square each, then halve each
- A quick and multiple atoms. Some quicks take more than one atom. For example, `?` (if) takes the previous 3 atoms. `ABC?` is `if { C } then { A } else { B }`, and is one link.

Links, like atoms, also have an arity. Typically, this is defined as the maximum arity of the atoms in each link, but this changes sometimes.

## Chains

A chain is a collection of links. For example, `H+²`, an example from above, is a *chain*. We describe chains by the arities of the links that make it up. `H+²` is a **1,2,1-chain**, as it is made up of a monad, then a dyad, then a monad. The parser has specific rules on how to break down chains, depending on their arities.

Chains, like links and atoms, also have an arity. However, here the arity is determined in one of two ways:

- If the chain is the main program, it is *variadic*. That is, its arity is unknown, and depends on the number of arguments passed to it by the user. Zero arguments makes it a niladic chain, 1 monadic and 2 or more dyadic.
- The chain can also start with *chain separators*. These are specific characters that tells the parser that the next chain has a certain arity. For example, `µ` tells the parser that the next chain is a *monad*, no matter what.

## Links (again)

Jelly uses the term *link* to refer to two different things: links as defined above, and the individual lines of a program. For example, the following is made up of 2 links (i.e. lines):

    +
    H
    
 By default, Jelly only executes the last line of a program, called the *main link*. Throughtout this tutorial, these such links will be referred to as either *main link*, *helper link* or *link (line)* to avoid confusion.

 ---

 [How does a Jelly program work?](https://github.com/cairdcoinheringaahing/RecipesForJelly/blob/main/howprogramswork.md)

 [Contents](https://github.com/cairdcoinheringaahing/RecipesForJelly/blob/main/README.md)
