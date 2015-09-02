# Iterators.jl

[![Iterators](http://pkg.julialang.org/badges/Iterators_0.3.svg)](http://pkg.julialang.org/?pkg=Iterators&ver=0.3)
[![Iterators](http://pkg.julialang.org/badges/Iterators_0.4.svg)](http://pkg.julialang.org/?pkg=Iterators&ver=0.4)

[![Build Status](https://travis-ci.org/JuliaLang/Iterators.jl.svg?branch=master)](https://travis-ci.org/JuliaLang/Iterators.jl)
[![Coverage Status](https://coveralls.io/repos/JuliaLang/Iterators.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/JuliaLang/Iterators.jl?branch=master)


Common functional iterator patterns.

## Installation

Install this package with `Pkg.add("Iterators")`

## Usage

-----------

- **takestrict**(xs, n)

  Equivalent to `take`, but will throw an exception if fewer than `n` items
  are encountered in `xs`.

- **repeatedly**(f, [n])

    Call a function `n` times, or infinitely if `n` is omitted.

    Example:
    ```julia
    for t in repeatedly(time_ns, 3)
        @show t
    end
    ```

    ```
    t = 0x0000592ff83caf87
    t = 0x0000592ff83d8cf4
    t = 0x0000592ff83dd11e
    ```

- **chain**(xs...)

    Iterate through any number of iterators in sequence.

    Example:
    ```julia
    for i in chain(1:3, ['a', 'b', 'c'])
        @show i
    end
    ```

    ```
    i = 1
    i = 2
    i = 3
    i = 'a'
    i = 'b'
    i = 'c'
    ```

- **product**(xs...)

    Iterate over all combinations in the cartesian product of the inputs.

    Example:
    ```julia
    for p in product(1:3,1:2)
        @show p
    end
    ```
    yields
    ```
    p = (1,1)
    p = (2,1)
    p = (3,1)
    p = (1,2)
    p = (2,2)
    p = (3,2)
    ```


- **distinct**(xs)

    Iterate through values skipping over those already encountered.

    Example:
    ```julia
    for i in distinct([1,1,2,1,2,4,1,2,3,4])
        @show i
    end
    ```

    ```
    i = 1
    i = 2
    i = 4
    i = 3
    ```

- **takenth**(xs, n)
    
    Iterate through every n'th element of `xs`

    Example:
    ```julia
    collect(takenth(5:15,3))
    ```

    ```
    3-element Array{Int32,1}:
      7
     10
     13
    ```

- **partition**(xs, n, [step])

    Group values into `n`-tuples.

    Example:
    ```julia
    for i in partition(1:9, 3)
        @show i
    end
    ```

    ```
    i = (1,2,3)
    i = (4,5,6)
    i = (7,8,9)
    ```

    If the `step` parameter is set, each tuple is separated by `step` values.

    Example:
    ```julia
    for i in partition(1:9, 3, 2)
        @show i
    end
    ```

    ```
    i = (1,2,3)
    i = (3,4,5)
    i = (5,6,7)
    i = (7,8,9)
    ```

- **groupby**(f, xs)

    Group consecutive values that share the same result of applying `f`.

    Example:
    ```julia
    for i in groupby(x -> x[1], ["face", "foo", "bar", "book", "baz", "zzz"])
        @show i
    end
    ```

    ```
    i = ASCIIString["face","foo"]
    i = ASCIIString["bar","book","baz"]
    i = ASCIIString["zzz"]
    ```

- **imap**(f, xs1, [xs2, ...])

    Iterate over values of a function applied to successive values from one or
    more iterators.

    Example:
    ```julia
    for i in imap(+, [1,2,3], [4,5,6])
         @show i
    end
    ```

    ```
    i = 5
    i = 7
    i = 9
    ```

- **subsets**(xs)

    Iterate over every subset of a collection `xs`.

    Example:
    ```julia
    for i in subsets([1,2,3])
     @show i
    end
    ```

    ```
    i = []
    i = [1]
    i = [2]
    i = [1,2]
    i = [3]
    i = [1,3]
    i = [2,3]
    i = [1,2,3]
    ```

- **subsets**(xs, k)

    Iterate over every subset of size `k` from a collection `xs`.

    Example:
    ```julia
    for i in subsets([1,2,3],2)
     @show i
    end
    ```

    ```
    i = [1,2]
    i = [1,3]
    i = [2,3]
    ```

- **iterate**(f, x)

    Iterate over successive applications of `f`, as in `f(x), f(f(x)), f(f(f(x))), ...`.

    Example:
    ```julia
    for i in take(iterate(x -> 2x, 1), 5)
        @show i
    end
    ```

    ```
    i = 1
    i = 2
    i = 4
    i = 8
    i = 16
    ```

## Moved Base Julia in 0.4, but still provided here for 0.3

- **countfrom**([start, [step]])

    Count, starting at ``start`` and incrementing by ``step``.  By default, ``start=0`` and ``step=1``.

    Example:
    ```julia
    for i in take(countfrom(5,5),5)
        @show i
    end
    ```
    yields
    ```
    i = 5
    i = 10
    i = 15
    i = 20
    i = 25
    ```

- **take**(xs, n)

    Iterate through at most n elements from `xs`.

    Example:
    ```julia
    for i in take(1:100, 5)
        @show i
    end
    ```

    ```
    i = 1
    i = 2
    i = 3
    i = 4
    i = 5
    ```


- **drop**(xs, n)

    Iterate through all but the first n elements of `xs`

    Example:
    ```julia
    for i in drop(1:10, 5)
        @show i
    end
    ```

    ```
    i = 6
    i = 7
    i = 8
    i = 9
    i = 10
    ```

- **cycle**(xs)

    Repeat an iterator in a cycle forever.

    Example:
    ```julia
    for i in take(cycle(1:3), 5)
        @show i
    end
    ```

    ```
    i = 1
    i = 2
    i = 3
    i = 1
    i = 2
    ```

- **repeated**(x, [n])

    Repeat one value `n` times, on infinitely if `n` is omitted.

    Example:
    ```julia
    for i in repeated("Hello", 3)
        @show i
    end
    ```

    ```
    i = "Hello"
    i = "Hello"
    i = "Hello"
    ```

## The `@itr` macro for automatic inlining in `for` loops

Using functional iterators is powerful and concise, but may incur in some
overhead, and manually inlining the operations can typically improve
performance in critical parts of the code. The `@itr` macro is provided to do
that automatically in some cases. Its usage is trivial: for example, given this code:
```
for (x,y) in zip(a,b)
    @show x,y
end
```
the automatically inlined version can be obtained by simply doing:
```
@itr for (x,y) in zip(a,b)
    @show x,y
end
```
This typically results in faster code, but its applicability has limitations:

* it only works with `for` loops;
* if multiple nested iterators are used, only the outermost is affected by the
  transformation;
* explicit expressions are required (i.e. when a `Tuple` is expected, an
  explicit tuple must be provided, a tuple variable won't be accepted);
* splicing is not supported;
* multidimensional loops (i.e. expressions such as `for x in a, y in b`) are
  not supported

The `@itr` macro can be used with the following supported iterators:

* zip
* enumerate
* take
* takestrict
* drop
* chain
