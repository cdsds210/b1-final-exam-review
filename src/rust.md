# Rust: Quick Review and What to Know

## Basics

- Ownership model: each value has a single owner; move semantics by default.
  - When an item is moved, its heap memory will be deallocated when the variable goes out of scope (Box Deallocation Principle)
- Borrowing: references `&T` (immutable) and `&mut T` (mutable) with lifetimes enforcing safety.
- Mutability: `let` vs `let mut`
- Types & inference: static typing with strong inference
- Pattern matching: `match` for control flow and destructuring.
- `Option<T>` to handle possibility of `None`, can be propagated with the `?` operator.
- Error handling: `Result<T, E>` for recoverable errors, `panic!` for unrecoverable; prefer `?` for propagation.
- `enum`s are used for object with different variants (ex: `Direction::North`, `Direction::South`, and `Direction::East`)
- `structs` are used to group related data, and provide OOP functionality with `impl`

## Midterm 2 Content

- Memory theory: stack vs heap, where values live, and how ownership controls deallocation.
- Ownership & borrowing rules: who can mutate, who can read, and how lifetimes prevent dangling refs.
- References: `&T` vs `&mut T`, borrowing rules (one `&mut` or many `&`), lifetime annotations conceptually.
- `Vec`: dynamic array; push/pop at end O(1) amortized; contiguous memory
  - Will have a fat pointer with `ptr`, `len`, and `cap`
- Slice `&[T]` points to a contiguous region in memory as a subset of another collection
  - Fat pointer contains `ptr` and `len`
  - Difference between `String`, `&String` and `&str`
- `HashMap` / `HashSet`: average O(1) lookup/insert/delete; require `Hash` + `Eq` needed for keys
- `BTreeMap` / `BTreeSet`: ordered maps/sets; `BTreeMap` is good for ordered unique value iteration and large datasets.

## Traits (very brief)

- What they are: interfaces describing shared behavior (`trait Foo { fn bar(&self); }`).
- Important derivable traits: `Debug`, `Clone`, `Copy` (small, trivially copyable types), `Eq`/`PartialEq`, `Ord`/`PartialOrd`, `Hash`.
- When to derive vs implement manually: derive when semantics are straightforward; implement when custom behavior exists.
  - Derive with macro `[#derive(T1, T2, T3)]`

## Post-Midterm

- Iterators & closures: iterator adaptors (`map`, `filter`, `for_each`), `for` loops vs iterator methods, `.collect()` to gather results.
- Reading algorithm code: you should be able to look at Rust code, and know what algorithm that is / determine it's time and space complexity
- Writing tests: `#[test]` and `cargo test` basics; use `assert!`/`assert_eq!` and small focused cases.
