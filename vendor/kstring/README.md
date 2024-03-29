KString
===========

> Key String: Optimized for map keys.

[![Crates Status](https://img.shields.io/crates/v/kstring.svg)](https://crates.io/crates/kstring)

## Background

Considerations:
- Large maps
- Most keys live and drop without being used in any other way
- Most keys are relatively small (single to double digit bytes)
- Keys are immutable
- Allow zero-cost abstractions between structs and maps (e.g. no allocating
  when dealing with struct field names)

Ramifications:
- Inline small strings rather than going to the heap.
- Preserve `&'static str` across strings (`KString`),
  references (`KStringRef`), and lifetime abstractions (`KStringCow`) to avoid
  allocating for struct field names.
- Use `Box<str>` rather than `String` to use less memory.

Features
- `max_inline`: Instead of aligning the inline-string for performance (15 bytes + length on 64-bit), use the full width (22 bytes on 64-bit)
- `arc`: Instead of using `Box<str>`, use `Arc<str>`.  Note: allocations are fast enough that this can actually slow things down for small enough strings.

Alternatives
- [`smol_str`](https://crates.io/crates/smol_str)
  - Size of String
  - Always uses `Arc` instead of `Box`
  - Always inlines 22 bytes
  - Whitespace-only optimizations
- [`smartstring`](https://crates.io/crates/smartstring)
  - Size of String
  - Allows mutability at the cost of relying on implementation details of `String`
  - Always inlines 23 bytes
- [`compact_str`](https://crates.io/crates/compact_str)
  - Size of String
  - Always uses `Arc` instead of `Box`
  - Inlines 22-23 bytes, depending on implementation

## License

Licensed under either of

 * Apache License, Version 2.0, (http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license (http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally
submitted for inclusion in the work by you, as defined in the Apache-2.0
license, shall be dual licensed as above, without any additional terms or
conditions.
