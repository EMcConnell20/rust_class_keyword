## What's the point of this? ##
**To establish a way of characterizing types through constants.**

___

Rust's trait system covers a wide variety of uses, but it has its limitations.
Specifically, traits just don't mix with constants. Sure, there's a feature in the
nightly channel for constant traits, but that's a can of snake-sized worms
I'm not too keen on opening.

When you look at how traits work, they do two major things:
1. They describe how to perform an action with a type.
2. They indicate that a rudimentary action can be performed on a type.

With just those two uses, the trait system creates an elaborate mesh of
logical operations that form much of Rust's functionality. It's an impressive
system with loads of intricacy and collaboration, both of which being things
I currently lack.

So, instead of trying to mess with an already-delicate system that wasn't
designed for what I want, I'm just going to make my own thing. By implementing
a custom `class` system, I can avoid the immediate complexities of the type
system and define something with the exact functionality that I want.

___

## What is a class? ##
**Good question.**

___

A `class` is like a trait, but for constants. The most similar aspect between
the two is the syntax, which is essentially the same between the two:

- The `class` keyword is used to define a class.
- The `impl` keyword is used to implement a class.

Classes and traits look almost exactly the same in definition, but their content
does differ slightly. Classes can contain any of these elements as a field:

- A Constant
- A Constant Function
- A Statically Defined Type

Unlike traits, classes require all of their elements to be evaluated at compile time.
Because of this, classes can act as arbitrary constants that characterize a type
in an abstract state using generics.

___

## The Logic Behind It ##
**The hypothetical use of classes.** 

___

Imagine you're building a library that works with simple shapes. You want it to be
easy for developers to use features from your crate, but you also don't want to turn
your Rust library into a Python library with how excessively adaptable it can be.

The most immediate solution would be to create a `Shape` trait to add some common
functionality to everything that should be a shape.

```Rust
pub trait Shape {
    type Position;
    
    const AXES: usize;
    const POINTS: usize;
}
```

With this trait, developers can now tell your library:

- What type they use to store positions.
- How many axes their shape has. (2D, 3D, 4D, ...)
- How many points their shape has.

Next, we should create a simple function to let that person move their shape.
We don't know what kind of fields the shape will have, so we'll just add a way
of converting the type to a standard shape format that we can use for several
operations.

```Rust
pub trait Shape {
    type Position;
    
    const AXES: usize;
    const POINTS: usize;
    
    fn standardize(&self) -> Vec<Self::Position>;
    fn personalize(standard: &Vec<Self::Position>) -> Self;
}
```

It would be nice if we could use `AXES` and `POINTS` to define the size of an array,
but they're members of a trait, so we can't use them in a constant operation.
What we can do, however, is use a generic parameter instead.

```Rust
pub trait Shape<const A: usize, const P: usize> {
    type Position;
    
    fn standardize(&self) -> [[Self::Position; A]; P];
    fn personalize(standard: &[[Self::Position; A]; P]) -> Self;
}
```

By turning the two constants into parameters, we can use them as actual constants.
It's a simple but effective hack that lets you actually have constants in a trait.
Unfortunately, this method does come with some minor downsides.

Whenever you want to use those parameters outside your trait, you have to declare
both the trait and the parameters independently. This can be tedious to write out,
but that's not the actual issue. The real problem starts when you give your have to
choose between using `R` and `RP` or `RADIUS` and `RENDER_PRIORITY`.

```Rust
// Option #1
impl <T: Shape<A, P, CC, N>, const A: usize, const P: usize, const CC: usize, const N: bool> Drawer<T> {}

// Option #2
impl <T, const AXES: usize, const POINTS: usize, const COLOR_CHANNELS: usize, const NORMALIZED: bool> Drawer<T>
    where T: Shape<AXES, POINTS, COLOR_CHANNELS, NORMALIZED> {}
```

You either turn your constants into alphabet soup and regret it less than a week later,
or you fill your code with impl lines longer than the actual code itself. It's like you're
in a less exciting, more annoying version of The Matrix where the pill just gets lodged
in your throat and Morpheus has to give you the heimlich maneuver.

So, as an alternative to that very uncomfortable scenario, I would like to introduce
classes to Rust. Instead of parameters, you can write actual constants defined on
a case-by-case basis for each type.

```
class Shape {
    const AXES: usize;
    const POINTS: usize;
    const COLOR_CHANNELS: usize;
    const NORMALIZED: bool;
}

impl <T: Shape> Drawer<T> {}
```

By evaluating their content fields only at compile time, classes can be used to
statically characterize types with actual constants. On top of that, because classes
are only evaluated at compile time, you can declare types based on the values of
their fields.

```
class Grid {
    type Item;

    const HEIGHT: usize;
    const WIDTH: usize;
    
    const AREA: usize = HEIGHT * WIDTH;
    
    type Generic = [[Item; WIDTH]; HEIGHT];
}
```

Unlike a trait, which prohibits constant functions, classes explicitly require
functions to be constant.

```
class StaticDefault {
    const DEFAULT: Self;
    
    const fn is_default(&self) -> bool;
}
```

Elements of a class must be statically evaluated and statically associated. If one
of those conditions is broken, then the class won't work.

```
use std::cell::Cell;

struct TestType {}
class MarkerClass<T> {}

struct Allowed(usize);
struct NotAllowed(Cell<Allowed>);
struct StillNotAllowed(Option<NotAllowed>);

// Works because usize can be statically evaluated. 
impl MarkerClass<Allowed> for TestType {}

// Works because 'MarkerClass' never uses 'Self'.
impl MarkerClass<Allowed> for NotAllowed {}

// Doesn't work because a 'Cell' cannot be a constant.
impl MarkerClass<NotAllowed> for TestType {}

// Doesn't work because 'StillNotAllowed' might contain an unallowed value.
impl MarkerClass<StillNotAllowed> for TestType {}
```

Pretty much anything mutable doesn't fly inside a class. They're used at compile time,
so anything that can't be checked at compile time won't work.
