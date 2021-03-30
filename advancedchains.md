# Advanced Chains (monadic)

This is a single challenge, rather than a tutorial page. In this page, we're going to work through 25 example programs in order to understand the chain separators to a better degree, when used in a monadic program. All 25 of these programs have the following in common:

- The program has the structure of `H{sep1},{sep2}²`, where `{sep1}` and `{sep2}` are (not necessarily distinct) members of `øµ)ðɓ`
- The program has the single command line argument of `6`

So, let's begin

## The programs

Each program is seaprated by a newline

```
Hø,ø²
Hø,µ²
Hø,)²
Hø,ð²
Hø,ɓ²
Hµ,ø²
Hµ,µ²
Hµ,)²
Hµ,ð²
Hµ,ɓ²
H),ø²
H),µ²
H),)²
H),ð²
H),ɓ²
Hð,ø²
Hð,µ²
Hð,)²
Hð,ð²
Hð,ɓ²
Hɓ,ø²
Hɓ,µ²
Hɓ,)²
Hɓ,ð²
Hɓ,ɓ²
```

[Try it online!][TIO-kmv3ydth]

[TIO-kmv3ydth]: https://tio.run/##PY6xCcAwDATrZJOHDJJF0gQvkDm8iAgYXFuVp1IUo1d38Hfw91XKY3ZqP7SPd9t/Go0Eggpp1kWjMXCKwAk5CikC0Ad1hA3KoKuSbyTfCN9IvhEGszJwisAJOQppBWYf "Jelly – Try It Online"

## The task

For each of these 25 programs, you should do the following:

- Analyse it, with the argument `6`, and guess what you think it outputs
- Make a note of this
- Run the program. If you were right, well done! If not, try to understand *why* and *where* you went wrong. If you still don't know why, check the answers below.

## The answers

**From here on are the answers. Don't look past here if you don't want to be spoiled**

<details>
  <summary><code>Hø,ø²</code></summary>
  
  Output: `3[0, 0]0`
  
  The two `ø`s here make this one have 1 of two possible outputs when thinking about how it might work:
  
  - `0`, because it discards the return values of the previous 2 links
  - `3[0, 0]0`, because it discards and outputs the return values of the previous 2 links

  The answer is the second one, as even in the context of chain separators, `ø` is an unparseable nilad when it's parsed without any dyadic chains

</details>

<details>
  <summary><code>Hø,µ²</code></summary>
  
  Output: `3[0, 0]`
  
  As there is no dyadic chain to go with it, the `ø` once again created an unparseable nilad, and so outputs `3` and begins again with `0`

</details>

<details>
  <summary><code>Hø,)²</code></summary>
  
  Output: `[[0, 0], [0, 0], [0, 0]]`
  
  This time, `ø` doesn't cause an unparseable nilad, because `)` consumes the result of `H` (**3**) and creates a *variadic* chain from the previous chain. The `ø` forces this variadic chain to be niladic, with an argument of 0, so we map `,` niladically over each integer **1**, **2**, **3**. As the chain is niladic, `,` always yields `[0, 0]`. Finally, the trailing `²` squares all `0`s to `0`

</details>

<details>
  <summary><code>Hø,ð²</code></summary>
  
  Output: `[0, 0]`
  
  Here, `ø` works with `ð` to turn the chains into a *nilad-dyad pair* of chains. The nilad part of this pair is `,` (run niladically), which yields `[0, 0]`. The `ð` then forces the next chain (`²`) to be a dyad, with `[0, 0]` on the left and **3** on the right (the result of `H`). This just squares `[0, 0]`, yielding `[0, 0]`

</details>

<details>
  <summary><code>Hø,ɓ²</code></summary>
  
  Output: `9`
  
  As with the previous version, `ø` works with `ɓ` to turn the chains into a *nilad-dyad pair* of chains. The nilad part of this pair is `,` (run niladically), which yields `[0, 0]`. The `ɓ` then forces the next chain (`²`) to be a dyad. But, this time, the chain has `[0, 0]` on the *right* and **3** on the *left* (the result of `H`), so we square the **3** instead

</details>

<details>
  <summary><code>Hµ,ø²</code></summary>
  
  Output: `[3, 3]0`
  
  Once again, `ø` is an unparseable nilad, which makes the execution fairly straightforward.

</details>

<details>
  <summary><code>Hµ,µ²</code></summary>
  
  Output: `[9, 9]`
  
  The `µ` just separates each of the three monadic chains, passing the previous values onto the next. First, we halve `6` to get `3`. Then, pair `3` with itself. Finally, square both `3`s to `[9, 9]`

</details>

<details>
  <summary><code>Hµ,)²</code></summary>
  
  Output: `[[1, 1], [4, 4], [9, 9]]`
  
  Again, this is pretty simple as chains go. The `)` pops the previous chain (`,`) which is forced to be monadic by `µ`. We iterate over each integer **1**, **2**, **3** (from `H`), pairing each integer with itself. Finally, we square all the integers in the list, yielding a list of pairs of squares

</details>
