# What's a memory allocator anyway?
*Notes on a presentation by Benjamin Feng*

## Why is a memory allocator?
* Stack allocation is usually automatic because it's very easy for the compiler to handle these automatically
* There is fixed total memory on the stack
* All stack sizes are fixed
* All stack lifetimes are known at comptime

## Simplest allocation strategies
### Page Allocator
* Request a page of memory from the Os (usually a fixed, large size)
* Slow
* wasteful
* not optimized

### Bump Allocator: e.g. FixedBufferAllocator
* Very simple, whenever something is allocated we bump the pointer to the next free index
* Very fast
* Cannot deallocate individual pieces of memory, the allocator only tracks it's length and the current index
* Entire buffer has to be freed at once, so it can be re-used, multiple times before releasing the buffer space, can be useful


### Expandable Bump Allocator: Arena Allocator (Requires a backing allocator)
* Fast allocation
* Expandable Total Memory
* Manual lifetime
* Must be freed all at once, just like a bump allocator
* If you are applying it correctly this isn't always an issue, especially since it all shares a lifetime

### Free List:  FreeListAllocator
* A linked list of nodes
* Allows you to free memory
* Allocations have a minimum size
* Very Slow
* Memory fragmentation, the gaps created by freed memory are never filled

### Free Lists with size buckets
* Faster
* Kind of adresses fragmentation, but not completely
* Puts pressure on the cache

### Cache friendly linear memory: Slap Allocator
* Simmilar to an arena
* Data is spacially local
* Allocations are fixed size
* All blocks take up the same slab, which makes fragmentation a non-issue
* Storing metadata is wasteful (will occupy at least one slab)


