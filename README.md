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

# Update: archived.

The main point of that fork was to keep the length being a `usize`,
because returning a different type was a footgun (adding lengths, in
particular, could easily trigger an overflow).

However, that change was eventually
[reverted](https://github.com/ziglang/zig/commit/85747b266aac8a2ff7fea4a4b18f722133544ad7),
so applications should just go back to using the type from the standard library.
