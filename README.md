# UInt256

Zero-dependency implementation of 256-bit integer type with a [transparent API]().

## Aims

### Light-weight

It should not rely on dependencies and that the library can potentially be used to run on bare metal.

### Transparent API

The API is designed so that one can learn the workings of the library through coding in a self-descriptive and transparent way.
The library attempts to be as transparent as possible than to hide certain inner workings in the form of default values for short productivity gain. This can be taxing at first, but just like Rust, understanding how it works will result in sustainable productivity.

## Examples

Here is example usage of `UIntBuilder` to build a `UInt256` type.

```rust
let num = UIntBuilder::new()
    .with_endian(Endian::Big)
    .from_bytes([
        0xff, 0x45, 0x67, 0x89, 0x0a, 0xbc, 0xde, 0xf1,
        0x23, 0x45, 0x67, 0x89, 0x0a, 0xc2, 0x03, 0xd5,
        0x12, 0x34, 0x56, 0x78, 0x90, 0xab, 0xcd, 0xef,
        0x12, 0x34, 0x56, 0x78, 0x90, 0xab, 0xcd, 0xef,
    ]).build();
```

**Note the following:**

- Without calling `with_endian(Endian)` this operation will panic. There is no assumption of a default [endianness](https://dev.to/pancy/what-are-big-and-little-endians-91h). You are responsible to learn about  and configure it.
- `from_ bytes([u8; 32])` takes a bytes array of exactly 32 bytes, not vectors. Fixed-size bytes array ensure correctness of data.
- If you want to pass a vector, you will be required to call `with_padding(Padding)` to pad the "missing space" of the bytes vector provided. Check out the following example.

```rust
let num = UInt256Builder::new()
    .with_padding(Padding::Left(0x00))
    .with_endian(Endian::Big)
    .from_partial_bytes(vec![0xcd, 0xef])
    .build();
```

In this example, `with_padding(Padding)` is called. You could pad a `u8` to fill the left or right side of the bytes vector you provide. Note that subsequently you will be required to call `from_partial_bytes(Vec<u8>)` instead of `from_bytes([u8; 32])` or again it will panic.