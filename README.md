# hashtable_old

This is a hashtable lib I used for projects like `ctetris`. It's a naive implementation with no easy way to reset/clean the table and has no way to resize the table. This is one of the best tables to use for insertion speed, as all it might do is one malloc and one hash. Deletion speed is abysmally slow as a trade-off. Naturally, indexing/data access speed is very fast too.

### Hash Function Credit

I have also posted this link in `hashfunc.c`, but here it is again: http://www.azillionmonkeys.com/qed/hash.html

This function seems to perform amazingly, and I personally recommend it for other hashing algorithms as it is well studied and used in production.

## Examples

### Inserting and getting keys

```c
hashtable* h = ht(100); // Takes the size of the table as the first argument.

hti(h, "hi", new_s("hello")); // `new_x` function allocates passed in item on the heap for ease of use.
hti(h, "hi.name", "anthony"); // You do not have to use `new_x` functions, but it is recommended for security.
hti(h, "hi.id", new_ui(10));  // You can have anything as the value, and mix based on name schemes.
hti(h, new_ui(0x2340), new_s(":o")); // You can add number keys as long as you add a 0 to the end, because that's sufficient enough of a disguise over a string. Please use this info with caution :(

printf("%s", htg(h, "hi"));     // Prints `hello`
printf("%d", htgi(h, "hi.id")); // Prints `10`

free(htd(h, "hi")); // Deletes the key off of the keystore and heap, use with caution. Returns value. Call free on return when you heap allocate items.

printf("%s", htg(h, "hi")); // SEGFAULT: Core Dumped (tried to read nullptr, as htg returns null for non-existent keys)
```

### Deleting all keys and freeing the hash table

```c

hashtable* h = ht(2000);

// ... do stuff with h

void* item;
while((item = htdfast(h))) free(item);
// Note: HTDFast breaks the hashtable, so use with caution. It's separated out because it's significantly faster than `htd`ing the whole table.
// If your `item`s weren't heap allocated, it's fine to just call `while(htdfast(h));`
// You can either do `htreset()` to essentially use the old hashtable as it was before, or `htfree()` for obvious reasons.
htfree(h);
```
