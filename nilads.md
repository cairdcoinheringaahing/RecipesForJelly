# Types of nilad

Jelly has multiple different types of nilad:

- Integers. These are strings of digits (`1234567890`), not beginning with `0`, and potentially being preceded by a `-` to denote negative numbers.

  A leading `0` is parsed as a separate token: [Try it online!](https://tio.run/##y0rNyan8/9/awFD7////ZgA "Jelly – Try It Online"). Examples of integers:
  
  - `1`
  - `127046`
  - `0`
  - `-14`

- Floating points. These are made up of two integers, separated by a `.`. The second cannot begin with a `-`. If the first is omitted, a `0` is filled in in replacement. If the second is omitted, then a `5` is filled in: [Try it online!](https://tio.run/##y0rNyan8/99Qz1DBWgFMGOqBWECsCxHUhZBgYV29//8B "Jelly – Try It Online")

- Scientific notation. These are made up of two parts, the *multiplier* and the *exponent*, separated by `ȷ`. This represents the exponential syntax `2e6` (`2000000`). If the multiplier if left out, it defaults to 1 and if the exponent is omitted, it defaults to `3` (i.e. thousands). Therefore, `ȷ` by itself is 1000

- Complex numbers. These are made up of two parts - real and imaginary - separated by `ı`. Each part may be either an integer, a float or a scientific notation number. An omitted real part is taken to be `0` and an omitted imaginary part as `1j`: [Try it online!](https://tio.run/##y0rNyan8/9/oyEYjBWsFCKl3ZCOYDSR0gRJ6IBoo9v8/AA "Jelly – Try It Online").

  The [longest single atom](https://tio.run/##y0rNyan8/19X78R2Xb0jGyH0//8A) in Jelly without using arbitrarily extendable syntax (e.g. digits or strings) uses all of these last 3
  
- Strings. Strings begin with `“` and have 1 of 4 different terminators. Additionally, you can use `“` to split the string to a list of strings. Internally, strings are implemented as lists of characters, which can lead to to [unexpected results](https://tio.run/##y0rNyan8//9Rw5zERw1zFVIVQKyk5BQg5/9/AA)

  - `”` terminates a regular string. Additionally, with no leading `“`, it denotes a [character literal](https://tio.run/##y0rNyan8//9Rw9zE//8B). This can be omitted if it is the last character in a program
  - `»` terminates a dictionary-compressed string. Take a look at [this post](https://codegolf.stackexchange.com/q/221428/66833) for a description of how the engine breaks down a string.
  - `‘` terminates a list of code-page indexes. Each character in the string is mapped to it's [Jelly codepoint](https://github.com/DennisMitchell/jellylanguage/wiki/Code-page)
  - `’` terminates a base-250 number. The result is the evaluation of the characters as digits of a base 250 number using the 1-based code page indices without the last six characters - as such `¡` represents 1, `ẏ` represents 249 and `ż` represents 250. As an example, `“©ṭBF’` represents the digits `[7, 225, 67, 71]` which evaluates to `123454321`

- Two-char literals. These begin with `⁾` and consume the next two characters in the program, turning them into a string: [Try it online!](https://tio.run/##y0rNyan8//9R477EpP//AQ "Jelly – Try It Online")
- Two-digit base-250 literals. These begin with `⁽` and are followed by two characters. These two characters are interpreted as a base-250 integer `x`. If `x > 31500`, then subtract 62850 from `x`. Otherwise, add 750. This allows you to express any integer between -31349 and -100, or between 1001 and 32250 in 3 bytes.

- Lists. You can create a list of this nilads by separating them with a `,` which will cause them to be parsed as a single link, rather than as a use of the pair atom. Note that this only works with these "literal" nilads, and without spaces. Using a `,` with a nilad atom (e.g. `³`) turns the comma into the pair atom. Lists can optionally be delimited with `[` and `]`.
