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

## HimitsuObfuscator
A lightweight LLVM-17 obfuscator for any Linux.

<img src="https://raw.githubusercontent.com/HimitsuShell/Himitsu/refs/heads/main/assets/features_obfuscation.png" width="200"><br>
<sub><b>Block Flow Graph (Ghidra)</b></sub>

## Usage
```shell
tar -xvf himitsu_obfuscator_v1.2.0_0.tar

vim main.c
-----------------------------
#include <stdio.h>
int main() {
  printf("Hello World!\n");
  return 0;
}
-----------------------------

# builds a binary that runs on any linux (static musl)
sudo apt-get install -y build-essential
./compiler/bin/x86_64-unknown-linux-musl-clang -flto -fuse-ld=lld -mllvm -sobf -mllvm -sub -static main.c -o main
./main
```

### Obfuscation Options
```shell
- bcf         # Bogus Control Flow (Warning: Significantly increases build time and binary size.)
  - bcf_prob  # Probability (1–100, default: 70)
  - bcf_loop  # Number of Iterations (default: 2)
- sub         # Instruction Substitution (add/and/sub/or/xor)
  - sub_loop  # Number of Iterations (default: 1)
- sobf        # String Encryption
- split       # Basic Block Splitting
  - split_num # Number of Splits (default: 3)
- ibr         # Indirect Branches
- icall       # Indirect Calls
- igv         # Indirect Global Variable
```

### System Requirements
- **OS:** Ubuntu 24.04
- **CPU:** x86_64 (Intel/AMD), 2.5 GHz or higher *(6 cores / 12 threads recommended)*
- **Memory:** 16 GB RAM
- **Storage:** 10 GB available space (SSD/NVMe)

### Supported Platforms
- **Linux x86_64 (static musl)**
- Linux ARM64 (Coming Soon)
- Linux ARMv7 (Planned)
- Linux RISC-V 64 (Planned)

## Maintenance (Requires Ubuntu)
```shell
curl -LO https://github.com/HimitsuShell/Himitsu/releases/download/v1.2.0/himitsu_core_v1.2.0.tar.gz

docker load -i himitsu_core_v1.2.0.tar.gz                  # Load docker image
docker run --name himitsu_core -d -it himitsu_core:v1.2.0  # Run container
sudo docker cp himitsu_core:/var/work/compiler/. .         # Copy comiler
sudo chown -R $USER:$USER .                                # Remove root permission
rm -rf himitsu_core_v1.2.0.tar.gz

rm -rf checksums.txt
find . -not -path './.git/*' -type f -exec file {} + | grep -E 'ELF|ar archive' | cut -d: -f1 | sed 's|^\./||' > .gitignore
git ls-files -c -o -i --exclude-standard | while read -r f; do
  sha256sum "$f" >> checksums.txt
  sudo rm -rf "$f"
done

git add .
git commit -m "commit message"
git push origin dev

# github release
sudo docker cp himitsu_core:/var/work/compiler .
sudo chown -R $USER:$USER .
tar -cvf himitsu_obfuscator_v1.2.0_0.tar ./compiler
```

## Discussions
Questions, bug reports, feature requests, and general discussions are welcome.  
You can also contact us at hjyun@mushsw.com.

## License
[MIT License](https://github.com/HimitsuShell/HimitsuObfuscator/blob/main/LICENSE)
