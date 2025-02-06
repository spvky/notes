# Pointers, Arrays, Slices
*These are notes from the presentation on arrays, slices and pointers from "Solving common Pointer Conundrums" by Loris Cro*

* [n]T = An array of length n containing type T

* [_]T = same as above the compiler is expected to infer the length of the array, length must be known at compile time

* *T = A pointer of type T
* [*]T = a many pointer, you will only really need to interact with these when interfacing with C code
* *[n]T = pointer to an array of length n and of type T, this converts to a slice automatically

Declaration of an array:
```
const a = [_]u8{ 1, 2, 3, 4 };
reating a slice from an array, accepts a range argument, start of range cannot be ommitted
const b = a[0..];

?*T = an optional pointer, can either be null or a valid pointer, in Rust: Option<&T>
*?T = a pointer to an optional, the pointer is always valid but the type could be null, in Rust: &Option<T>
```
