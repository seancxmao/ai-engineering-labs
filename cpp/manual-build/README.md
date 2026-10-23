# Manual Build

## Background

不使用构建框架，用底层工具build。如果没有C++ 编译器，需要安装 Xcode Command Line Tools。

## Experiments

### Experiment 1

编译：

```
clang++ -std=c++17 -I include -c src/math_utils.cpp -o math_utils.o
clang++ -std=c++17 -I include -c src/main.cpp -o main.o
```

参数含义：

* clang++：C++ 编译器驱动程序
* -std=c++17：使用 C++17 标准
* -I include：将 include/ 加入头文件搜索路径
* -c：只编译，不执行链接
* -o：指定输出文件

链接：

```
clang++ main.o math_utils.o -o hello
```

运行：

```
./hello
```

### Experiment 2

编译并链接：

```
clang++ -std=c++17 -I include \
    src/main.cpp src/math_utils.cpp \
    -o hello
```

运行：

```
./hello
```

## Summary



## Takeaways
