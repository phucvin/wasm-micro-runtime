cd ..

uname -a

> Linux codespaces-30e938 6.5.0-1025-azure #26~22.04.1-Ubuntu SMP Thu Jul 11 22:33:04 UTC 2024 x86_64 GNU/Linux

sudo apt install build-essential cmake g++-multilib libgcc-11-dev lib32gcc-11-dev ccache

# Build fast interpreter

cd product-mini/platforms/linux/

mkdir build && cd build

cmake ..

make

cd /workspaces/wasm-micro-runtime/

cd test

wget https://github.com/phucvin/wasm3/raw/refs/heads/main/test/lang/fib32.wasm

```
$ time ../product-mini/platforms/linux/build/iwasm -f fib fib32.wasm 40
0x6197ecb:i32
real    0m6.340s
```

# Build JIT

cd /workspaces/wasm-micro-runtime/

cd product-mini/platforms/linux/

sudo apt install python3-requests

./build_llvm.sh

rm -rf build

mkdir build && cd build

cmake .. -DWAMR_BUILD_JIT=1

make

cd /workspaces/wasm-micro-runtime/test

```
$ time ../product-mini/platforms/linux/build/iwasm -f fib fib32.wasm 40
0x6197ecb:i32
real    0m0.671s
```


# Build Fast JIT

cd /workspaces/wasm-micro-runtime/product-mini/platforms/linux/

rm -rf build

mkdir build && cd build

cmake .. -DWAMR_BUILD_FAST_JIT=1

make

cd /workspaces/wasm-micro-runtime/test

```
$ time ../product-mini/platforms/linux/build/iwasm -f fib fib32.wasm 40
0x6197ecb:i32
real    0m1.558s
```
