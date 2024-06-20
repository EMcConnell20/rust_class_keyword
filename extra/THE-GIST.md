## What's the point of this? ##
**To establish a way of creating generic constants.**

___

Rust's trait system covers a wide variety of uses, but it has its limitations.
Specifically, traits just don't mix with constants. Sure, there's a feature in the
nightly channel for constant traits, but that's a can of snake-sized worms
I'm not to keen on opening.

When you look at how traits work, they can do two major things:
1. They describe how to perform an action on a given type.
2. They indicate that a rudimentary action can be performed on a type.

With just those two uses, traits create an elaborate system of logical operations
that allow much of the Rust language to function as it does. It's a solid system,
and messing with it is a pretty scary task.

So, instead of trying to mess with an already highly delicate system that
controls most of the Rust ecosystem, I thought it would be a good idea to
just wrap it up and say, "yeah, it does its job like we want, so let's not overload
it with functionality." Traits are good enough as is, and they do their job pretty well.
I'm not about to mess with that.

Instead, I'm going to design a system specifically in place of a constant trait.
I'm going to be implementing a `class` system that allows me to create generic
constants to statically characterize types through monomorphism.

___

## What is a class? ##
**Good question.**

___

A `class` is like a trait, but for constants. If a trait describe how to perform
a something, then a class describes what to perform. They have a similar syntax
to traits, although the way they work is quite different.

The `class` keyword is used to define a class.

The `impl` keyword is used to implement a class on a type.

A class can contain these three elements as fields:
- Constants
- Constant Functions
- Statically Defined Types

___

## Examples ##

___

Imagine you have to manage several types of shapes.

```
struct Triangle {
    a: [f32; 2],
    b: [f32; 2],
    c: [f32; 2],
}

struct Cube {
    top_right_front: [usize; 3],
    bottom_left_back: [usize; 3],
}
```

Both `Triangle` and `Cube` store the position of a shape, but they do so in
completely separate ways for completely separate items. A `Triangle` stores
the 2D coordinates of each of its three points, but a `Cube` stores the 3D
coordinates for only two of its six points.

It can often be tedious, confusing, and even problematic to try and manage these
types under a single structure. However, classes can simplify this process without
any significant cost. By using a class, you can describe the key characteristics
of a type in a clear and concise manner both //TODO - Continue here!

```
class Shape {
    type Position;
    
    const AXES: usize; 
    const POINTS: usize;
    
    const fn from_abstract(array: [[Position; AXES]; POINTS]) -> Self;
}
```

The class `Shape` defines several key aspects of how each representation
of a shape is intended to work. It defines they type of value that the shape
uses as a position, the number of axes that the shape has, the number of
points that a shape uses, and the
