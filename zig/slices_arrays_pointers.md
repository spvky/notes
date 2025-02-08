# Pointers, Arrays, Slices
*These are notes from the presentation on arrays, slices and pointers from "Solving common Pointer Conundrums" by Loris Cro*

## Type signatures
* [n]T = An array of length n containing type T

* [_]T = same as above the compiler is expected to infer the length of the array, length must be known at compile time

* *T = A pointer of type T

* [*]T = a many pointer, you will only really need to interact with these when interfacing with C code
* *[n]T = pointer to an array of length n and of type T, this converts to a slice automatically

* ?*T = an optional pointer, can either be null or a valid pointer, roughly equivalent the following in Rust:
```rust
Option<&T>
```
* *?T = a pointer to an optional, the pointer is always valid but the type could be null, equivalent in Rust:
```rust
&Option<T>
```

Declaration of an array:
```zig
const a = [_]u8{ 1, 2, 3, 4 };
```
Crreating a slice from an array, accepts a range argument, start of range cannot be ommitted
```zig
const b = a[0..];
```

## So what's const?

* `const` variable is not the same as a `const` pointer

* if you don't need to modify a string, always ask for a slice / pointer

* string literals exist as their own unique type

## Coercion & Conversion
* array to an array pointer, *Note: an array pointer will automatically coerce to a slice*
```zig
var a: [5]u8 = {1,2,3,4,5};
var b: *[5]u8 = &a;
```
* array to slice
```zig
var a: [5]u8;
var b: []u8 = a[0..];
```

## Stack Memory

* the following is a classic gotcha that behaves differently in debug vs release. The code run fine in debug, but you will get random memory in release, because we are returning a pointer that becomes invalid when the function scope ends (each function call creates a temporary stack frame, that will only be valid for the scope of that function)



```zig
const std = @import("std");
const print = std.debug.print;

pub fn main() void {
	const msg = getMsg();
	print("{}\n", .{msg});
}

fn getMsg() []u8 (
	var buf = [5]u8 {'h', 'e', 'l', 'l', 'o' };
	return &buf;
)
```

```zig
const std = @import("std");
const print = std.debug.print;

pub fn main() void {
	const msg = getMsg();
	print("{}\n", .{msg});
}

fn getMsg() []const u8 (
	return "hello";
)
```
