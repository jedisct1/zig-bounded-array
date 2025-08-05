# BoundedArray for Zig

A `BoundedArray` is a structure containing a fixed-size array, as well as the length currently being used.

It can be used as a variable-length array that can be freely resized up to the size of the backing array.

If you're looking for a Zig equivalent to Rust's super useful `ArrayVec`, this is it.

It is useful to pass around small arrays whose exact size is only known at runtime, but whose maximum size is known at comptime, without requiring an `Allocator`.

Bounded arrays are easier and safer to use than maintaining buffers and active lengths separately, or involving structures that include pointers.

They can also be safely copied like any value, as they don't use any internal pointers.

```zig
var actual_size = 32;
var a = try BoundedArray(u8, 64).init(actual_size);
var slice = a.slice(); // a slice of the 64-byte array
var a_clone = a; // creates a copy - the structure doesn't use any internal pointers
```

`BoundedArray` is an extremely convenient structure, which (IMHO) greatly contributes to making code using small arrays safe and simple.

Unfortunately, it has been removed from the standard library in Zig 0.15.

If you want the convenience of `BoundedArray` back, add this repository as a dependency:

```sh
zig fetch --save https://github.com/jedisct1/zig-bounded-array/archive/refs/tags/0.1.0.tar.gz
```

In `build.zig`:

```zig
const bounded_array = b.dependency("bounded_array", .{});
...
x.addImport("bounded_array", bounded_array.module("bounded_array"));
```

And finally, in your application:

```zig
const BoundedArray = @import("bounded_array").BoundedArray;
```
