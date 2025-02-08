# Zig Allocators

## FixedBufferAllocator/ThreadSafeFixedBufferAllocator
only allocator in the standard library that handles stack data, works with a buffer of objects created on the stack

## General Purpose Allocator

## Page Allocator

## Arena Alocator

## Choosing an Allocator
What allocator to use depends on a number of factors. Here is a flow chart to help you decide:

1. Are you making a library? In this case, best to accept an Allocator as a parameter and allow your library's users to decide what allocator to use.
2. Are you linking libc? In this case, std.heap.c_allocator is likely the right choice, at least for your main allocator.
3. Need to use the same allocator in multiple threads? Use one of your choice wrapped around std.heap.ThreadSafeAllocator
4. Is the maximum number of bytes that you will need bounded by a number known at comptime? In this case, use std.heap.FixedBufferAllocator.
5. Is your program a command line application which runs from start to end without any fundamental cyclical pattern (such as a video game main loop, or a web server request handler), such that it would make sense to free everything at once at the end? In this case, it is recommended to follow this pattern: 
