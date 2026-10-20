# mini build

## Background

### 重要概念
理解现代Pyhton的build最重要的概念：build前端和后端的分离。

常见的build前端：

* pip
* build
* uv

常见的build后端：

* setuptools
* uv-build
* poetry core

### 参考资料

**Packaging Python Projects**
    
https://packaging.python.org/en/latest/tutorials/packaging-projects/

要点：

* This tutorial walks you through how to package a simple Python project. It will show you how to add the necessary files and structure to create the package, how to build the package, and how to upload it to the Python Package Index (PyPI).
* 项目结构、代码组织、如何打包发布

**Build System Interface**

https://pip.pypa.io/en/stable/reference/build-system/

要点：

* build backends也叫build systems
* Build process

**Build System Support (setuptools)**

https://setuptools.pypa.io/en/latest/build_meta.html

要点：

* setuptools.build_meta所提供的功能

**Writing your `pyproject.toml`**

https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

**How to modernize a `setup.py` based project?**

https://packaging.python.org/en/latest/guides/modernize-setup-py-project/

要点：

* A `pyproject.toml` file is strongly recommended.
* `setup.py` can exist in a modern Setuptools based project.

**PEP 660 – Editable installs for pyproject.toml based builds (wheel based)**
    
https://peps.python.org/pep-0660/

## Experiments

```
uv venv
source .venv/bin/activate

uv pip install build
```

## Summary


## Takeaways

