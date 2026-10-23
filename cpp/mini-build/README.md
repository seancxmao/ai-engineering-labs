# Mini Build

## Background

如果没有 CMake 或 Ninja，可以通过 Homebrew 安装：

```
brew install cmake ninja
```

## Experiments

### Experiment 1

`CMakeLists.txt`三个关键指令：

* add_library()：定义一个名为 math_utils_lib 的库目标。
* add_executable()：定义一个名为 hello_native 的可执行文件目标。
* target_link_libraries()：声明可执行文件依赖于 math_utils_lib，CMake 据此组织构建顺序和链接关系。

Configure + Generate：

```
cmake -S . -B build -G Ninja
```

参数含义：

* -S .：源码目录是当前目录。
* -B build：构建目录是 build/。
* -G Ninja：指定 Ninja 作为生成器。

命令行输出：

```
❯ cmake -S . -B build -G Ninja
-- The CXX compiler identification is AppleClang 21.0.0.21000101
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.5s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/maochenxiao/workspace/projects/personal/tech-labs/cpp-labs/mini-build/build
```

Build 阶段。CMake 根据生成的构建规则调用 Ninja，Ninja 再调用 C++ 编译器和链接器。

```
cmake --build build --verbose
```

命令行输出：

```
❯ cmake --build build --verbose
Change Dir: '/Users/maochenxiao/workspace/projects/personal/tech-labs/cpp-labs/mini-build/build'

Run Build Command(s): /opt/homebrew/bin/ninja -v
[1/4] /usr/bin/c++  -I/Users/maochenxiao/workspace/projects/personal/tech-labs/cpp-labs/mini-build/include -std=gnu++17 -arch arm64 -MD -MT CMakeFiles/math_utils_lib.dir/src/math_utils.cpp.o -MF CMakeFiles/math_utils_lib.dir/src/math_utils.cpp.o.d -o CMakeFiles/math_utils_lib.dir/src/math_utils.cpp.o -c /Users/maochenxiao/workspace/projects/personal/tech-labs/cpp-labs/mini-build/src/math_utils.cpp
[2/4] : && /opt/homebrew/bin/cmake -E rm -f libmath_utils_lib.a && /usr/bin/ar qc libmath_utils_lib.a  CMakeFiles/math_utils_lib.dir/src/math_utils.cpp.o && /usr/bin/ranlib libmath_utils_lib.a && /opt/homebrew/bin/cmake -E touch libmath_utils_lib.a && :
[3/4] /usr/bin/c++  -I/Users/maochenxiao/workspace/projects/personal/tech-labs/cpp-labs/mini-build/include -std=gnu++17 -arch arm64 -MD -MT CMakeFiles/hello_native.dir/src/main.cpp.o -MF CMakeFiles/hello_native.dir/src/main.cpp.o.d -o CMakeFiles/hello_native.dir/src/main.cpp.o -c /Users/maochenxiao/workspace/projects/personal/tech-labs/cpp-labs/mini-build/src/main.cpp
[4/4] : && /usr/bin/c++ -arch arm64 -Wl,-search_paths_first -Wl,-headerpad_max_install_names CMakeFiles/hello_native.dir/src/main.cpp.o -o hello_native  libmath_utils_lib.a && :
```

运行程序：

```
./build/hello_native
```

命令行输出：

```
❯ ./build/hello_native
Hello from native build!
1 + 2 = 3
```

## Summary


## Takeaway

