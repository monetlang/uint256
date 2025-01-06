# UInt256

Zero-dependency implementation of 256-bit integer type with a sane and [semantic API](https://dev.to/pancy/what-is-semantic-api-4i1j).

## Aims

### Light-weight

It does not rely on dependencies.

### Transparent, Semantic API

The API is designed so that one can learn the workings of the library through coding in a self-descriptive and transparent way, potentially with very little need for separate documentation.

The library attempts to be as transparent as possible than to hide certain inner workings in the form of default values for short productivity gain. This can be taxing at first, but just like Rust, understanding how it works will result in sustainable productivity.

## Examples

Here is example usage of `UInt256Builder` to build a `UInt256` type.

```rust

let my_bytes_array = [
    0xff, 0x45, 0x67, 0x89, 0x0a, 0xbc, 0xde, 0xf1,
    0x23, 0x45, 0x67, 0x89, 0x0a, 0xc2, 0x03, 0xd5,
    0x12, 0x34, 0x56, 0x78, 0x90, 0xab, 0xcd, 0xef,
    0x12, 0x34, 0x56, 0x78, 0x90, 0xab, 0xcd, 0xef,
];

let num = UInt256Builder::new()
    .with_endian(Endian::Big)
    .from_bytes(my_bytes_array)
    .build();

```

**Note the following:**

- Without calling `with_endian(Endian)` this operation will panic. There is no assumption of a default [endianness](https://dev.to/pancy/what-are-big-and-little-endians-91h). You are responsible to learn about and configure it. There is no shortcut to not understand endianness and able to "run it".
- `from_ bytes([u8; 32])` takes a bytes array of exactly 32 bytes, not vectors. Fixed-size bytes array ensure correctness of data. But more importantly, it ensures the user actually understand bytes data. Vectors would have completely glazed over this for the sake of friendliness. But real friends expect the best from you and put you on the spot, not letting you off easy so you can have a beer early.
- If you want to pass a vector, you will be required to call `with_padding(Padding)` to pad the "missing space" of the bytes vector provided. This check once again check the user’s understanding of what he’s doing. 

Check out the following example.

```rust

let num = UInt256Builder::new()
    .with_padding(Padding::Left(0x00))
    .with_endian(Endian::Big)
    .from_partial_bytes(vec![0xcd, 0xef])
    .build();

```

In this example, `with_padding(Padding)` is called. You could pad a `u8` to fill the left or right side of the bytes vector you provide. Note that subsequently you will be required to call `from_partial_bytes(Vec<u8>)` instead of `from_bytes([u8; 32])` or again it will panic.

This code is equivalent to passing the following raw bytes array to build a UInt256 using `UInt256Builder` in the first example:

```rust

let bytes = [
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xcd, 0xef,
];

```

However, it is really hard to miss what one is doing when they are required to call with_padding(Padding).

