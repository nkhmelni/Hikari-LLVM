# Hikari-LLVM

> **Mirror of the Hikari LLVM Obfuscator**
>
> This is an archived mirror of [Hikari-LLVM15](https://github.com/61bcdefg/Hikari-LLVM15) by **61bcdefg** (also known as **NeHyci**), which was a fork of the original [HikariObfuscator/Hikari](https://github.com/HikariObfuscator/Hikari) project.
>
> The original repositories are no longer available. This mirror preserves the source code for archival and educational purposes.

## Branches

| Branch | LLVM Version |
|--------|--------------|
| `main` | 18.1.8 |
| `llvm-16.0.0` | 16.0.0 |

## Obfuscation Features

Hikari adds the following obfuscation passes to LLVM/Clang. All flags are passed via `-mllvm`.

### Enable Flags

| Flag | Description |
|------|-------------|
| `-enable-allobf` | Enable all obfuscation passes |
| `-enable-strcry` | String encryption |
| `-enable-constenc` | Constant encryption |
| `-enable-cffobf` | Control flow flattening |
| `-enable-splitobf` | Split basic blocks |
| `-enable-subobf` | Instruction substitution |
| `-enable-bcfobf` | Bogus control flow |
| `-enable-indibran` | Indirect branching |
| `-enable-funcwra` | Function wrapper |
| `-enable-fco` | Function call obfuscation |
| `-enable-acdobf` | Anti-class dump (iOS/macOS) |
| `-enable-antihook` | Anti-hooking (iOS/macOS) |
| `-enable-adb` | Anti-debugging (iOS/macOS) |

### Configuration Options

#### Bogus Control Flow
| Flag | Default | Description |
|------|---------|-------------|
| `-bcf_prob` | 40 | Probability of applying BCF to a function |
| `-bcf_loop` | 1 | Number of BCF iterations |
| `-bcf_cond_compl` | 3 | Condition complexity |
| `-bcf_junkasm` | false | Insert junk assembly in bogus blocks |
| `-bcf_onlyjunkasm` | false | Only insert junk assembly (no opaque predicates) |
| `-bcf_junkasm_minnum` | 2 | Minimum junk assembly instructions |
| `-bcf_junkasm_maxnum` | 4 | Maximum junk assembly instructions |
| `-bcf_createfunc` | false | Use function wrapper for opaque predicates |

#### Function Wrapper
| Flag | Default | Description |
|------|---------|-------------|
| `-fw_prob` | 30 | Probability of wrapping a function call |
| `-fw_times` | 2 | Number of wrapper iterations |

#### String Encryption
| Flag | Default | Description |
|------|---------|-------------|
| `-strcry_prob` | 100 | Probability of encrypting each byte |

#### Constant Encryption
| Flag | Default | Description |
|------|---------|-------------|
| `-constenc_times` | 1 | Number of encryption iterations |
| `-constenc_togv` | false | Replace constants with global variables |
| `-constenc_togv_prob` | 50 | Probability for togv |
| `-constenc_subxor` | false | Use complex XOR substitution |
| `-constenc_subxor_prob` | 40 | Probability for subxor |

#### Indirect Branching
| Flag | Default | Description |
|------|---------|-------------|
| `-indibran-use-stack` | false | Load jump table address from stack |
| `-indibran-enc-jump-target` | false | Encrypt jump table and indices |

#### Instruction Substitution
| Flag | Default | Description |
|------|---------|-------------|
| `-sub_prob` | 50 | Probability of substituting an instruction |

#### Split Basic Blocks
| Flag | Default | Description |
|------|---------|-------------|
| `-split_num` | 2 | Number of splits per basic block |

#### Anti-Hooking (iOS/macOS)
| Flag | Default | Description |
|------|---------|-------------|
| `-ah_inline` | true | Check for inline hooks |
| `-ah_antirebind` | false | Prevent fishhook symbol rebinding |

#### Anti-Debugging (iOS/macOS)
| Flag | Default | Description |
|------|---------|-------------|
| `-adb_prob` | 40 | Probability of adding anti-debug checks |

#### Miscellaneous
| Flag | Default | Description |
|------|---------|-------------|
| `-aesSeed` | 0x1337 | Seed for AES encryption |

## Building

```bash
git clone https://github.com/nkhmelni/Hikari-LLVM.git
cd Hikari-LLVM
mkdir build && cd build
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DLLVM_ENABLE_PROJECTS="clang" \
  -DLLVM_TARGETS_TO_BUILD="X86;AArch64;ARM" \
  ../llvm
ninja
```

## Usage Example

```bash
# Compile with string encryption and bogus control flow
./build/bin/clang -mllvm -enable-strcry -mllvm -enable-bcfobf \
  -mllvm -bcf_prob=80 -mllvm -bcf_loop=2 \
  -O2 -o output source.c
```

## Credits

- **61bcdefg / NeHyci** - Hikari-LLVM15 maintainer
- **Naville / Zhang** - Original Hikari author
- **LLVM Project** - Base compiler infrastructure

## License

See [LICENSE.TXT](LICENSE.TXT) for LLVM licensing terms.

---

# The LLVM Compiler Infrastructure

[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/llvm/llvm-project/badge)](https://securityscorecards.dev/viewer/?uri=github.com/llvm/llvm-project)
[![OpenSSF Best Practices](https://www.bestpractices.dev/projects/8273/badge)](https://www.bestpractices.dev/projects/8273)

Welcome to the LLVM project!

This repository contains the source code for LLVM, a toolkit for the
construction of highly optimized compilers, optimizers, and run-time
environments.

The LLVM project has multiple components. The core of the project is
itself called "LLVM". This contains all of the tools, libraries, and header
files needed to process intermediate representations and convert them into
object files. Tools include an assembler, disassembler, bitcode analyzer, and
bitcode optimizer.

C-like languages use the [Clang](http://clang.llvm.org/) frontend. This
component compiles C, C++, Objective-C, and Objective-C++ code into LLVM bitcode
-- and from there into object files, using LLVM.

Other components include:
the [libc++ C++ standard library](https://libcxx.llvm.org),
the [LLD linker](https://lld.llvm.org), and more.

## Getting the Source Code and Building LLVM

Consult the
[Getting Started with LLVM](https://llvm.org/docs/GettingStarted.html#getting-the-source-code-and-building-llvm)
page for information on building and running LLVM.

For information on how to contribute to the LLVM project, please take a look at
the [Contributing to LLVM](https://llvm.org/docs/Contributing.html) guide.

## Getting in touch

Join the [LLVM Discourse forums](https://discourse.llvm.org/), [Discord
chat](https://discord.gg/xS7Z362),
[LLVM Office Hours](https://llvm.org/docs/GettingInvolved.html#office-hours) or
[Regular sync-ups](https://llvm.org/docs/GettingInvolved.html#online-sync-ups).

The LLVM project has adopted a [code of conduct](https://llvm.org/docs/CodeOfConduct.html) for
participants to all modes of communication within the project.
