# How Chains are interpreted

The behaviour of a chain changes depending on its arity. However, all chains are parsed by repeatedly chopping off the first few links in the chain and acting on them.

The running value, `v`, through the chain is initially **0** before any parsing happens.

For the following, `J` and `¹` represent arbitrary monadic links, and `c` and `,`  represent arbitrary dyadic links. Numbers and `Ø` are niladic links. Additionally, remember that a *link* can be composed of atoms and quicks, or just atoms. The symbols chosen here represent *links*, not just atoms.

## Niladic chains

Niladic chains - those that aren't passed any arguments - are the easiest to understand. If the first link in the chain is a nilad **N**, it is chopped off, the left and right arguments are both set to be **N** and the running value becomes **N**. If the first link is *not* a nilad, then the left and right arguments become **0**. In either case, the rest of the chain is parsed as a monadic chain

## Monadic chains

Here, the chain is passed an argument `a`. We begin the chain by setting both the left and right arguments to `a`, then we set the running value to `v = a`.

Next, we repeat the following steps until the chain is empty:

1. The first patterns looked for are `cØ` or `1,`. That is, **dyad-nilad pairs**. These set the running value to `v = v c Ø`. For example, `+5` would add 5 to `v`
2. If no dyad-nilad pairs are found, we look for **dyad-monad pairs**, such as `,J`. These run the monad on `v` first, then run the dyad on `v` and the monad result. For example, `+H` first halves `v`, then adds that to `v`, yielding `v = v + v÷2`
3. Next, we look for a single dyad `c`. In this case, we replace `v` with `v c a`, our argument. So, `+` would calculate `v = v + a`
4. Finally, we look for a single monad `J`. In this case, we simply replace `v` with the result of running the monad on `v`
5. Now, we chop off the first pattern that was matched from the from of the chain, and, if the chain is non-empty, go to step 1

Let's examine how we'd execute `+²×3H;` with an argument of `a = 2`:

- To start with, we set both arguments to `2`, and then `v = 2`
- Now, we match the second rule with `+²`, as `²` is the *square* monad. We calculate `v² = 4`, which will be the right argument to our dyad. The left argument will be `v`. This then sets `v = v + v²`, or `v = 2 + 4 = 6`. Finally, we chop off `+²` and continue parsing with `×3H;`
- `×3` is a dyad-nilad pair, matching our first pattern. This just multiplies `v` by 3, yielding `v = v × 3 = 6 × 3 = 18`. We chop off `×3` and continue with `H;`
- `H;` does not match either of the first two patterns (note that the monad has to be on the *right* side of the dyad for the second one), but `H` does match the 4th pattern. Therefore, we just halve `v`, yielding `v = 18 ÷ 2 = 9`. Finally, we chop off `H`, yielding just `;`
- `;` matches our third pattern (as `;` is the *concatenate* dyad), so we run `v = v ; a`, yielding `v = [9, 2]`. When we chop this off, the chain is now empty, so we output the current value of `v` and end execution.

More generally, `+²×3H;` returns `[3(a+a²)÷2, a]` for any argument `a`: [Try this online!](https://tio.run/##y0rNyan8/1/70KbD0409rP///28EAA)

## Dyadic chains
