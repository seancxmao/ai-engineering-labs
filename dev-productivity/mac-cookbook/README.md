# Mac Cookbook

## Background


## Recipes

### C++ Dev Toolchain

建议采用以下组合：

* VS Code: IDE 和代码阅读
* Xcode Command Line Tools: Apple 平台编译器、SDK 等
* CMake: 构建系统
* CMake Tools: VS Code 中配置、构建和调试 CMake 工程
* clangd: C++ 代码补全、跳转、诊断
* LLDB: C++ 调试器

### VS Code

主力 IDE：VS Code

用于 C++ 学习项目、CMake 工程、Python/C++ 混合开发、阅读 vLLM 源码，以及通过 Remote SSH 开发远程 Linux 上的 CUDA 项目。VS Code 官方支持 macOS 上的 Clang 调试和远程开发。

底层工具链：Xcode / Command Line Tools

提供 Apple 平台的 SDK、Clang 等编译工具。即使主要使用 VS Code，也需要这些工具来编译和调试 macOS 上的 C++ 程序。

### Xcode Command Line Tools

通过安装 Xcode Command Line Tools，安装 C++ 编译器

```
# 查看当前选中的开发者目录
xcode-select -p

# 切换当前选中的开发者目录
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer

# 触发 Command Line Tools 安装
xcode-select --install
```

如果已安装，会提示更新

```
❯ xcode-select --install
xcode-select: note: Command line tools are already installed. Use "Software Update" in System Settings or the softwareupdate command line interface to install updates
```

用softwareupdate更新

```
❯ softwareupdate --list
Software Update Tool

Finding available software
Software Update found the following new or updated software:
...
* Label: Command Line Tools for Xcode 27.0-27.0
	Title: Command Line Tools for Xcode 27.0, Version: 27.0, Size: 519460KiB, Recommended: YES,
...
```
