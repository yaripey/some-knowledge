# Transitions
Transitions are used to give different actions animations in css.

We can set what property change we want to animate by using the `transition-propery` rule.

We can set the animation time with `transition-duration` rule. We can specify time in _seconds_ as _s_ and _miliseconds_ as _ms_.

We can also set a delay before an animations occurs by using `transition-delay`.

We can also set how exactly the animation is being handles using _timing functions_.

The default is `ease`, which starts the transition slowly, speeds up in the middle, and slows down again at the end. There are some another values:
- `ease-in` - starts slow, accelerates quickly, stops abruptly.
- `ease-out` - begins abruptly, slows down, and ends slowly.
- `ease-in-out` - starts slow, gets fast in the middle, and ends slowly.
- `linear` - constant speed throughout.

We can set those time functions by `transition-timing-function`.

Also, we can combine all those properties in one, for convenience. Like this: `transition: color 1.5s linear 0.5s`. The properties must be specified in this order:
1. `transition-property`
2. `transition-duration`
3. `transition-timing-function`
4. `transition-delay`

We can also combine different transitions into one property, by separating sets of descriptions by commas.

And if you want to specify all transitions, you can just type `all` instead of a particular property.