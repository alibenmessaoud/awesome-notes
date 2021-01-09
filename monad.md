## Monad Definition

You need three things to define a monad—in `Optional`’s terms:

1. The type `Optional<T>` itself
2. The method `ofNullable(T)` that wraps a value `T` into an `Optional<T>`
3. The method `flatMap(Function<T, Optional<U>>`) that applies the given function to the value that is wrapped by the `Optional` on which it is called

There’s an alternative definition using `map` instead of `flatMap`, but it’s too long to fit here.

## Monad Laws

Now it gets interesting—a monad has to fulfill three laws to be one of the cool kids. In `Optional`’s terms:

1. For a `Function<T, Optional<U>> f` and a value `v`, `f.apply(v)` must equal `Optional.ofNullable(v).flatMap(f)`. This *left identity* guarantees it doesn’t matter whether you apply a function directly or let `Optional` do it.
2. Calling `flatMap(Optional::ofNullable)` returns an `Optional` that equals the one you called it on. This *right identity* guarantees applying no-ops doesn’t change anything.
3. For an `Optional<T> o` and two functions ...

`Optional`’s methods fulfill these definitions *but* break the laws. Not without consequences...