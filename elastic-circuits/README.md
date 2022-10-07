# Dynamatic: Dynamically Scheduled HLS

## Build the Elastic Pass

放在前面：
安装过程可能会遇见一些麻烦，仅供参考
### 1.
编译安装llvm到home/llvm-6.0
但在后面make的时候,会去/usr下面找include头文件,如果用户之前已经apt安装了llvm-6.0但没装polly
会在编译elastic circuit时报错找不到polly相关头文件

我的做法
删除系统之前安装的llvm,然后在编译安装时默认安装位置,是/usr/local
解决了编译代码的问题

### 2.
跑example时,报错找不到xxxxxxxxxxx.txt,这是有个readcsv函数里,指定了一个绝对路径,而这个文件是github上没有的

我的做法,注释掉了这一部分代码,这一部分和后端的delay,latency有关,在IR层面没什么影响

### Build and install LLVM 

```bash
git clone http://github.com/llvm-mirror/llvm --branch release_60 --depth 1
cd llvm/tools
git clone http://github.com/llvm-mirror/clang --branch release_60 --depth 1
git clone http://github.com/llvm-mirror/polly --branch release_60 --depth 1
cd ..
mkdir _build && cd _build
cmake .. -DCMAKE_BUILD_TYPE=Debug -DLLVM_INSTALL_UTILS=ON -DLLVM_TARGETS_TO_BUILD="X86" -DCMAKE_INSTALL_PREFIX=$HOME/llvm-6.0
make
make install
```

### Compile the elastic pass code from the source repository, where this README lies:

```bash
mkdir _build && cd _build
cmake .. -DLLVM_ROOT=$HOME/llvm-6.0
make
```

--------------------------------------------------------------

## Run the Elastic Pass

```bash
cd examples/
vim Makefile
```

Change the lines below according to which installation of LLVM you are using:
```makefile
CLANG = ../../llvm-6.0/bin/clang # change depending on where your installation of clang is
OPT = ../../llvm-6.0/bin/opt # change depending on where your installation of opt is
```

Use this option together with function addBuffersSimple (in MyCFGPass.cpp) to obtain a naive buffer placement:
```bash
make name=loop_array graph
xdg-open _build/loop_array_graph.png
```

Use this option together without function addBuffersSimple (in MyCFGPass.cpp) to obtain a design without buffers (which can then be optimized with Buffers):
```bash
make name=loop_array graph_f
xdg-open _build/loop_array_graph.png
```
Note: this option requires a main() and a function call. Still not present in many examples.

Ta-da!

Check out the files generated in `_build`.
