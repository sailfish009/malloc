# malloc
Doug Lea's malloc

Doug Lea has developed dlmalloc ("Doug Lea's Malloc") as a general-purpose allocator, starting in 1987. The GNU C library (glibc) uses ptmalloc,[12] an allocator based on dlmalloc.[13]

Memory on the heap is allocated as "chunks", an 8-byte aligned data structure which contains a header, and usable memory. Allocated memory contains an 8 or 16 byte overhead for the size of the chunk and usage flags. Unallocated chunks also store pointers to other free chunks in the usable space area, making the minimum chunk size 24 bytes.[13]

Unallocated memory is grouped into "bins" of similar sizes, implemented by using a double-linked list of chunks (with pointers stored in the unallocated space inside the chunk).[13]

For requests below 256 bytes (a "smallbin" request), a simple two power best fit allocator is used. If there are no free blocks in that bin, a block from the next highest bin is split in two.

For requests of 256 bytes or above but below the mmap threshold, recent versions of dlmalloc use an in-place bitwise trie algorithm. If there is no free space left to satisfy the request, dlmalloc tries to increase the size of the heap, usually via the brk system call.

For requests above the mmap threshold (a "largebin" request), the memory is always allocated using the mmap system call. The threshold is usually 256 KB.[14] The mmap method averts problems with huge buffers trapping a small allocation at the end after their expiration, but always allocates an entire page of memory, which on many architectures is 4096 bytes in size.
