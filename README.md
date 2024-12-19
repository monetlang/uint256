# UInt256

Zero-dependency implementation of 256-bit integer type.

## Aims

### Light-weight

It should rely on no dependencies that the library can be used to run on bare metal.

### Learning through API

The API is designed so that one can learn the workings of the library through coding because it is self-descriptive and transparent.

### No obscure defaults

The library attempts to be as transparent as possible than to hide certain inner workings in the form of default values for short productivity gain. This can be taxing at first, but just like Rust, understanding how it works will result in sustainable productivity.

## features

- `UIntBuilder` for simple, declarative and self-descriptiveness.

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

Without calling `with_endian` this operation will panic. You are hence encouraged to learn about [endianness]() to build a `UInt256` type.

```rust
let num = UInt256Builder::new()
    .with_padding(Padding::Left(0x00))
    .with_endian(Endian::Big)
    .from_partial_bytes(vec![0xcd, 0xef])
    .build();
```

In this example, `with_padding(Padding)` is called. You could pad a `u8` to fill the left or right side of the bytes you provide. Note that subsequently `from_partial_bytes(Vec<u8>)` should be called instead of `from_bytes([u8; 32])` or again it will fail.

This is the concept of a "Learning API". There is no magic. Each call forces the user to know what they are doing.


