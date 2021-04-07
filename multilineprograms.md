# Multi-line programs in Jelly

As previously mentioned, Jelly has two different things called *links*: the lines in a program and the individual "units" in a program that make up chains. In this section, we'll be talking about the former.

Lines are separated from one another with either a newline or a pilcrow (`¶`), which are completely interchangable in Jelly programs. By default, only the last line in the code is executed. We call this our *main link* and every other line *helper links*. These links are always variadic, with the number of arguments determined at runtime.

Jelly has 9 quicks for calling other links:

- `ß`. This calls the same link it's on (recursion), with the same arity it was initially run. [For example](https://tio.run/##y0rNyan8///hzpZHDTMOzz/WpXBop4KNoYGK/f///40B). This checks if its argument is less than 10, and if so, it prints it, increments it then calls the link again, with the incremented argument as its new argument
- `¢`: "Last link as nilad". This calls the previous link (on the line above) as a nilad i.e. with no arguments. 99% of the time you'll use this is because you need to calculate or build a nilad that's just too long to fit in the main link
