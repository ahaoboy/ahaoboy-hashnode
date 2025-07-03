---
title: "UPX: Rust Binary Size Optimization"
datePublished: Thu Jul 03 2025 12:36:50 GMT+0000 (Coordinated Universal Time)
cuid: cmcnde1ql000702lb9agu0huc
slug: upx-rust-binary-size-optimization
tags: rust, rust-programming, upx

---

By tweaking your build configuration and applying a post-build compressor, you can dramatically reduce the size of your Rust executables.

## 1\. Default Release Build

With the default `cargo build --release` settings, the produced binary is about **6.1 MB**.

```bash
$ cargo build --release
$ ls -lh target/release/your_binary
# -rwxr-xr-x  1 user  staff    6.1M Jul  3 20:00 your_binary
```

## 2\. Optimized Release Profile

Add the following to your `Cargo.toml` to enable Link Time Optimization (LTO), strip symbols, and use a single codegen unit:

```toml
[profile.release]
debug = false
lto = true
strip = true
opt-level = 3
codegen-units = 1
```

Rebuild and check the size:

```bash
$ cargo build --release
$ ls -lh target/release/your_binary
# -rwxr-xr-x  1 user  staff    5.4M Jul  3 20:05 your_binary
```

This reduces the binary from **6.1 MB** down to **5.4 MB**.

## 3\. Compression with UPX

For even smaller footprints, compress your Windows executable (or Linux ELF) with [UPX](https://upx.github.io/):

```bash
$ upx ./target/release/your_binary.exe
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.0.2       Markus Oberhumer, Laszlo Molnar & John Reiser   Jul 10th 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
    5,400,000 ->   1,800,000   33.33%    win32/pe    your_binary.exe

Packed 1 file.
```

After UPX, the binary shrinks from **5.4 MB** down to **1.8 MB**.

---

**Summary of sizes:**

* **Default** (`--release`): 6.1 MB
    
* **Tuned release profile**: 5.4 MB
    
* **\+ UPX compression**: 1.8 MB
    

These simple steps can help you minimize the distribution size of your Rust applications.