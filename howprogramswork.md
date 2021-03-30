# How does a Jelly program work?

A full Jelly program is made up by one of more *lines*. Each line is made up of one or more *chains*. Each chain is made up of one or more *links*. Each link is made up of one or more *atoms* and zero or more *quicks*.

Execution works by only considering the last line, the **main link**. If this line contains any *chain separators* (any of `øµ)ðɓ`), it contains more than one chain. Otherwise, it is made of a single chain.

## Single chain lines

These are the simplest of Jelly lines. They consist of a single chain, made up of one or more links. In this case, we simply parse the links based on their arity patterns and the arity of the chain.

### Niladic chain

Here, the chain is passed no arguments. If the first link in the chain is a nilad `α`, then we remove this link and evaluate the rest of the chain as a monadic link with argument `α`. If the first link isn't a nilad, we instead evaluate the chain as a monadic chain with argument `0`

### Monadic chain

Jelly parses a monadic or dyadic chain by repeatedly chopping off links from the start until the chain is empty. The chain is passed an argument `ω` and has a "flow-through" value `v`. If the chain begins with a nilad `α` followed by a monad, then `v = α`. Otherwise, `v = ω`.

Then, Jelly begins executing the chain, by matching the arities of the links in the chain against the following patterns. Each step, we work our way down the list of patterns, stopping at the first one it matches:

1. **dyad-monad pair**. The arities of the first 2 links are **2, 1**. For example, `+H` or `,²`. We first apply the monad to `v`, yielding `v'`. We then set `v` to the result of applying the dyad, with `v` on the left and `v'` on the right. For the `+H` example, we first halve `v`, then add it to `v`, yielding `v + v÷2`
2. **dyad-nilad pair**. The arities of the first 2 links are **2, 0**. We apply the dyad to `v` with the nilad as the right argument and `v` as the left.
3. **nilad-dyad pair**. The arities of the first 2 links are **0, 2**. We apply the dyad to `v` with the nilad as the *left* argument and `v` as the right.
4. **dyad**. The arity of the first link is **2**. We apply the dyad to `v` with `v` as the left argument and `ω` (the argument passed to the chain) as the right
5. **monad**. The arity of the first link is **1**. We apply the monad to `v`

After updating `v` to the value from the pattern, we remove the link(s) matched and begin again with the rest of the chain.

If none of the patterns are met, we have what's called an *unparseable nilad*, i.e. a nilad that isn't part of a dyad pair. In this case, Jelly outputs `v` (without a trailing newline) and resets `v` to this nilad. [For example](https://tio.run/##y0rNyan8///QJtNDm/7//28GAA)

### Dyadic chain

This time, the chain is passed two arguments, `λ` (left) and `ρ` (right). Again, the chain has a "flow-through" value `v`. However, unlike a monadic chain, the starting value isn't so easily determined:

- If the chain begins with a nilad `α` followed by a monad, then `v = α` and we chop off the nilad
- If the chain begins with 3 dyads, then we apply the first dyad with `λ` on the left and `ρ` on the right, assign the result to `v` and chop off the first dyad
- Otherwise, `v = λ`.

As with the monadic chain, we then work our way down the following patterns of arities, stopping at the first match:

1. **dyad, dyad, nilad**. The arities of the first three links are **2, 2, 0**. We take the first dyad and apply it to `v`, with `v` on the left and `ρ` on the right. We then take the **dyad-nilad pair** formed by the other 2 links and apply that to the result of the first dyad
2. **dyad, dyad**. The arities of the first two links are **2, 2** (and the third link is not a nilad). First, we apply the second dyad with `λ` on the left and `ρ` on the right, calling the result `v'`. Then, we apply the first dyad with `v` on the left and `v'` on the right. [An example](https://tio.run/##y0rNyan8/19H@////4b/jQA)
2. **dyad-nilad pair**. The arities of the first 2 links are **2, 0**. We apply the dyad to `v` with the nilad as the right argument and `v` as the left.
3. **nilad-dyad pair**. The arities of the first 2 links are **0, 2**. We apply the dyad to `v` with the nilad as the *left* argument and `v` as the right.
4. **dyad**. The arity of the first link is **2**. We apply the dyad to `v` with `v` as the left argument and `ρ` as the right
5. **monad**. The arity of the first link is **1**. We apply the monad to `v`

After updating `v` to the value from the pattern, we remove the link(s) matched and begin again with the rest of the chain.

As with a monadic chain, if none of the patterns are met, we have an *unparseable nilad*, i.e. a nilad that isn't part of a dyad pair. In this case, Jelly outputs `v` (without a trailing newline) and resets `v` to this nilad. [For example](https://tio.run/##y0rNyan8/1/70CYTj////5v9NwQA)

## Multi chain lines

More complex Jelly programs will involve lines with multiple chains. These are separated from each other with the *chain separators*: any of `øµ)ðɓ`. The separators set the arity of the next chain to either **0**, **1** or **2**, depending on the separator.

`ø`, `µ` and `ð` are all fairly basic. They set the arity of the next chain to 0, 1 and 2, respectively. `ɓ` is similar to `ð`, in that it sets the next chain to be dyadic, but it swaps the order of the arguments passed to it.

`)` doesn't modify the next chain. Instead, it takes the *previous* chain and the value that would be passed to it, `v`. Then, it maps the previous chain over the elements of `v`. [For example](https://tio.run/##y0rNyan8/z/o0NZDmzT///9vBgA). This generates the `R`ange of the argument, `[1, 2, 3, ..., ω]`. The `µ` then creates a new monadic chain made up of one link, `²` (square). `)` then takes this range and the chain, and maps the chain over each element in the range, yielding the first `ω` square numbers.

Thae parsing rules for a group of multiple chains are exactly the same as parsing a chain with multiple links. We match the chains based on their arities, then break down the group based on the same patterns as the patterns used in the single chain rules. However, this does involve figuring out the arities of each chain. Consider the following example, run monadically with argument `ω`:

    Cð+×µH
    
Breaking this up into a list of chains we get:

    [C, +×, H]
    
We can now use the separators to figure out the arities of each chain. As this is passed a single argument, we know that the first chain is monadic. The `ð` separator tells us that the second chain is dyadic and `µ` means the third is monadic. Therefore, the arities here are **1, 2, 1** and the entire thing is monadic.

By looking up the rules for monadic parsing, we can see that this matches pattern 5 (**monad**), then pattern 1 (**dyad-monad**). So the first thing we do is apply the first monad to `v = ω`, which yields `1-ω`. We then deal with with dyad-monad pattern. First, we apply the monad to `v = ω` to get `ω÷2`. Finally, we apply the dyadic chain `+×` with `1-ω` on the left and `ω÷2` on the right. Applying the "dyadic chain" parsing rules to this, we match pattern 2 (**dyad, dyad**). `λ = 1-ω`, `ρ = ω÷2` and `v = λ = 1-ω`. We first get `λ×ρ = (1-ω)(ω÷2)`, then calculate `v+(λ×ρ) = λ+(λ×ρ) = (1-ω)+(1-ω)(ω÷2)`, which is our final output.

The key thing to remember if you want to succinctly use multiple chains in your Jelly programs is that *the parsing rules are the sae* as the parsing rules for links. By analysing the arities of the chains, you can often save a lot of bytes.
