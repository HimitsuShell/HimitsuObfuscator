<p align="center">
  <a href="https://himitsushell.com/" target="blank"><img src="https://avatars.githubusercontent.com/u/264618628?s=200&v=4" width="100" alt="HimitsuShell Logo" /></a>
</p>
<p align="center">
  <a href="https://github.com/HimitsuShell/HimitsuObfuscator/releases">
    <img src="https://img.shields.io/github/v/release/HimitsuShell/HimitsuObfuscator?color=2da44e" alt="Latest Release" />
  </a>
  <a href="https://github.com/HimitsuShell/HimitsuObfuscator/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue" alt="MIT License" />
  </a>
</p>

# HimitsuObfuscator
a lightweight, stable, gpl-free llvm-17 obfuscator for linux.

<img src="https://raw.githubusercontent.com/HimitsuShell/Himitsu/refs/heads/main/assets/features_obfuscation.png" width="200"><br>
<sub><b>Block Flow Graph (Ghidra)</b></sub>

## Usage
```shell
# download obfuscator
curl -LO https://github.com/HimitsuShell/HimitsuObfuscator/releases/download/v2.1.0/himitsu_obfuscator.tar
tar -xvf himitsu_obfuscator.tar

# create sample source
vim main.c
-----------------------------
#include <stdio.h>
int main() {
  printf("Hello World!\n");
  return 0;
}
-----------------------------

# build and run (x86_64-linux-gnu)
apt install -y build-essential
./bin/clang --target=x86_64-linux-gnu -mllvm -sobf main.c -o main 
./main

# supported targets: x86_64-linux-musl, aarch64-linux-gnu, aarch64-linux-musl, etc.
```

#### Obfuscation Options
```shell
- fla         # control flow flattening
- bcf         # bogus control flow (slow build, larger binary)
  - bcf_prob  # probability (1–100, default: 30)
  - bcf_loop  # number of iterations (default: 1)
- sub         # instruction substitution (add/and/sub/or/xor)
  - sub_loop  # number of iterations (default: 1)
- sobf        # string encryption
- split       # basic block splitting
  - split_num # number of splits (default: 2)

# selective obfuscation ('no' prefix disables)
void test1() __attribute__((annotate("sobf"), annotate("nobcf")));
void test1() { printf("Hello World!"); }

# planned options: indirect branches, indirect calls, indirect global variable
```

#### Specifications
- **llvm:** 17.0.6
- **languages:** c, c++ (or llvm ir)
- **platforms:** linux (x86_64, aarch64) | **planned:** armv7, riscv64, win11
- **requirements:** ubuntu 24.04, x86_64 cpu (6c/12t rec.), 16gb ram, 10gb ssd
- **bcf option:** do not compile with `-g` when using `-bcf` (debug info skips obfuscation).

## Info
- **discussions:** open a github issue/discussion or email hjyun@mushsw.com
- **sponsors:** Supported by the [Pyeongtaek Industrial Promotion Agency](https://pipabiz.or.kr/web/main/index.do) (South Korea), a government-affiliated public institution.
- **license:**
  - [MIT License](https://github.com/HimitsuShell/HimitsuObfuscator/blob/main/LICENSE)
  - all copyleft (gpl/agpl) removed. fully commercial-safe.
  - `[HimitsuShell CE]` notice applies to HimitsuShell only, not HimitsuObfuscator.