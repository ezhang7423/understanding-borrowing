# understanding-borrowing

Possibly the coolest part about rust is how it guarantees memory safety without a garbage collector. This is done through its ownership system, which follows these rules:

- Each value in Rust has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

(https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#ownership-rules)

Anything that is stored on the heap is shallow copied and the old reference is invalidated. For example:

```rust
let s1 = String::from("boi");
let s2 = s1; // s1 is no longer valid after this!
```

This is so that the value will not be double freed when the program reaches end of scope.

Anything that is stored just on the stack is "deep copied" and old references are not invalidated. Instead, you get two different objects afterwards. Integers, floating points, booleans, char, tuples. If you make a custom object that only stores data on the stack, you can implement the `copy` trait which will enable "deep copying".

Passing values to a function act similarly to assigning to a variable. However, taking ownership and returning it with every function is tedious. Rust solves this with references, which allow us to use a value without taking ownership. This concept is referred to as borrowing. The function uses the value, and they gives it back without taking ownership. References follow these two rules:

- At any given time, you can have either one mutable reference or any number of immutable references.
- References must always be valid.

Valid means that a reference will never point to an empty object. The first rule is to prevent race conditions.

## Interior mutability

http://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/second-edition/ch15-05-interior-mutability.html
