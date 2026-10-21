# Mini Build

## Background

### 重要概念
理解现代Pyhton的build最重要的概念：build前端和后端的分离。

常见的build前端：

* build
* pip
* uv

常见的build后端：

* setuptools
* uv-build
* poetry core

### 参考资料

**Packaging Python Projects**
    
https://packaging.python.org/en/latest/tutorials/packaging-projects/

* This tutorial walks you through how to package a simple Python project. It will show you how to add the necessary files and structure to create the package, how to build the package, and how to upload it to the Python Package Index (PyPI).
* 项目结构、代码组织、如何打包发布

**Build System Interface**

https://pip.pypa.io/en/stable/reference/build-system/

* build backends也叫build systems
* Build process

**Build System Support (setuptools)**

https://setuptools.pypa.io/en/latest/build_meta.html

* setuptools.build_meta所提供的功能

**Writing your `pyproject.toml`**

https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

**How to modernize a `setup.py` based project?**

https://packaging.python.org/en/latest/guides/modernize-setup-py-project/

* A `pyproject.toml` file is strongly recommended.
* `setup.py` can exist in a modern Setuptools based project.

**PEP 660 – Editable installs for pyproject.toml based builds (wheel based)**
    
https://peps.python.org/pep-0660/

## Experiments

### Experiment 1: build

```
uv venv
source .venv/bin/activate

uv pip install build
python -m build --help
usage: ...
    A simple, correct Python build frontend.
```

build 是独立的 Python 构建前端工具，build 会根据 pyproject.toml 准备隔离环境，再调用 setuptools 后端。

```
❯ python -m build -w -v
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:
  - setuptools>=77
> /private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/build-env-ngbmropn/bin/python -Im pip install --ignore-installed --use-pep517 --no-warn-script-location --no-compile --no-input -r
  /var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/build-requirements-hb65mkz4.txt
< Collecting setuptools>=77 (from -r /var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/build-requirements-hb65mkz4.txt (line 1))
<   Using cached setuptools-84.2.0-py3-none-any.whl.metadata (6.6 kB)
< Using cached setuptools-84.2.0-py3-none-any.whl (875 kB)
< Installing collected packages: setuptools
< Successfully installed setuptools-84.2.0

< [notice] A new release of pip is available: 25.0.1 -> 26.2.1
< [notice] To update, run: python -m pip install --upgrade pip
* Getting build dependencies for wheel...
>>> setup.py is executed
running egg_info
creating src/mini_build.egg-info
writing src/mini_build.egg-info/PKG-INFO
writing dependency_links to src/mini_build.egg-info/dependency_links.txt
writing top-level names to src/mini_build.egg-info/top_level.txt
writing manifest file 'src/mini_build.egg-info/SOURCES.txt'
reading manifest file 'src/mini_build.egg-info/SOURCES.txt'
writing manifest file 'src/mini_build.egg-info/SOURCES.txt'
* Installed build dependency versions:
  - setuptools==84.2.0
* Building wheel...
>>> setup.py is executed
running bdist_wheel
running build
running build_py
creating build/lib/mini_build
copying src/mini_build/__init__.py -> build/lib/mini_build
running egg_info
writing src/mini_build.egg-info/PKG-INFO
writing dependency_links to src/mini_build.egg-info/dependency_links.txt
writing top-level names to src/mini_build.egg-info/top_level.txt
reading manifest file 'src/mini_build.egg-info/SOURCES.txt'
writing manifest file 'src/mini_build.egg-info/SOURCES.txt'
installing to build/bdist.macosx-11.0-arm64/wheel
running install
running install_lib
creating build/bdist.macosx-11.0-arm64/wheel
creating build/bdist.macosx-11.0-arm64/wheel/mini_build
copying build/lib/mini_build/__init__.py -> build/bdist.macosx-11.0-arm64/wheel/./mini_build
running install_egg_info
Copying src/mini_build.egg-info to build/bdist.macosx-11.0-arm64/wheel/./mini_build-0.1.0-py3.12.egg-info
running install_scripts
creating build/bdist.macosx-11.0-arm64/wheel/mini_build-0.1.0.dist-info/WHEEL
creating '/Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build/dist/.tmp-c3w_cdls/mini_build-0.1.0-py3-none-any.whl' and adding 'build/bdist.macosx-11.0-arm64/wheel' to it
adding 'mini_build/__init__.py'
adding 'mini_build-0.1.0.dist-info/METADATA'
adding 'mini_build-0.1.0.dist-info/WHEEL'
adding 'mini_build-0.1.0.dist-info/top_level.txt'
adding 'mini_build-0.1.0.dist-info/RECORD'
removing build/bdist.macosx-11.0-arm64/wheel
Successfully built mini_build-0.1.0-py3-none-any.whl
```

参数含义：

* `-w`：只构建 wheel。
* `-v`：显示更详细的日志。

检查产物：

```
❯ find dist -maxdepth 1 -type f
dist/mini_build-0.1.0-py3-none-any.whl
```

### Experiment 2: build no-isolation

```
python -m build -w --no-isolation
```

要求当前环境中已经安装构建后端及所需依赖。若缺少 setuptools，先在当前环境中安装：

```
uv pip install setuptools
```

### Experiment 3: pip

```
❯ python -m pip install -v .
Using pip 26.2.1 from /Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build/.venv/lib/python3.12/site-packages/pip (python 3.12)
Processing ./.
  Running command installing build dependencies
  Using pip 26.2.1 from /Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build/.venv/lib/python3.12/site-packages/pip (python 3.12)
  Collecting setuptools>=77
    Obtaining dependency information for setuptools>=77 from https://files.pythonhosted.org/packages/dd/a2/2e1af99d56c8145aa69496026c7e5fd7b6ec759b500c3ad697e2062832dc/setuptools-84.2.0-py3-none-any.whl.metadata
    Using cached setuptools-84.2.0-py3-none-any.whl.metadata (6.6 kB)
  Using cached setuptools-84.2.0-py3-none-any.whl (875 kB)
  Installing collected packages: setuptools
  Successfully installed setuptools-84.2.0
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  >>> setup.py is executed
  running egg_info
  writing src/mini_build.egg-info/PKG-INFO
  writing dependency_links to src/mini_build.egg-info/dependency_links.txt
  writing top-level names to src/mini_build.egg-info/top_level.txt
  reading manifest file 'src/mini_build.egg-info/SOURCES.txt'
  writing manifest file 'src/mini_build.egg-info/SOURCES.txt'
  Getting requirements to build wheel ... done
  Running command Preparing metadata (pyproject.toml)
  >>> setup.py is executed
  running dist_info
  creating /private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info
  writing /private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info/PKG-INFO
  writing dependency_links to /private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info/dependency_links.txt
  writing top-level names to /private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info/top_level.txt
  writing manifest file '/private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info/SOURCES.txt'
  reading manifest file '/private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info/SOURCES.txt'
  writing manifest file '/private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build.egg-info/SOURCES.txt'
  creating '/private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-modern-metadata-a0piv_mi/mini_build-0.1.0.dist-info'
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: mini-build
  Running command Building wheel for mini-build (pyproject.toml)
  >>> setup.py is executed
  running bdist_wheel
  running build
  running build_py
  running egg_info
  writing src/mini_build.egg-info/PKG-INFO
  writing dependency_links to src/mini_build.egg-info/dependency_links.txt
  writing top-level names to src/mini_build.egg-info/top_level.txt
  reading manifest file 'src/mini_build.egg-info/SOURCES.txt'
  writing manifest file 'src/mini_build.egg-info/SOURCES.txt'
  installing to build/bdist.macosx-11.0-arm64/wheel
  running install
  running install_lib
  creating build/bdist.macosx-11.0-arm64/wheel
  creating build/bdist.macosx-11.0-arm64/wheel/mini_build
  copying build/lib/mini_build/__init__.py -> build/bdist.macosx-11.0-arm64/wheel/./mini_build
  running install_egg_info
  Copying src/mini_build.egg-info to build/bdist.macosx-11.0-arm64/wheel/./mini_build-0.1.0-py3.12.egg-info
  running install_scripts
  creating build/bdist.macosx-11.0-arm64/wheel/mini_build-0.1.0.dist-info/WHEEL
  creating '/private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-ephem-wheel-cache-rdrmvcu9/wheels/f1/b3/9e/94a1337839948caee67dc7341f4666e994dcff324b7fef85a3/tmp6fwzf_kv/.tmp-op3sqbll/mini_build-0.1.0-py3-none-any.whl' and adding 'build/bdist.macosx-11.0-arm64/wheel' to it
  adding 'mini_build/__init__.py'
  adding 'mini_build-0.1.0.dist-info/METADATA'
  adding 'mini_build-0.1.0.dist-info/WHEEL'
  adding 'mini_build-0.1.0.dist-info/top_level.txt'
  adding 'mini_build-0.1.0.dist-info/RECORD'
  removing build/bdist.macosx-11.0-arm64/wheel
  Building wheel for mini-build (pyproject.toml) ... done
  Created wheel for mini-build: filename=mini_build-0.1.0-py3-none-any.whl size=1243 sha256=156a8c3ae5afb87115f2a74ec6070fbd4931b57660f5a0f065a8e62ae522e082
  Stored in directory: /private/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/pip-ephem-wheel-cache-rdrmvcu9/wheels/f1/b3/9e/94a1337839948caee67dc7341f4666e994dcff324b7fef85a3
Successfully built mini-build
Installing collected packages: mini-build
Successfully installed mini-build-0.1.0
```

```
❯ python -c "import mini_build; print(mini_build.hello())"
hello from the build experiment!
```

### Experiment 4: uv pip editable install

```
uv pip install -e . -v
```

```
❯ uv pip install -e . -v
DEBUG uv 0.10.12 (Homebrew 2026-03-19 aarch64-apple-darwin)
DEBUG Marking explicit source tree for reinstall: `/Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build`
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.13-macos-aarch64-none` at `/Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.13 environment at: .venv
DEBUG Using request connect timeout of 10s and read timeout of 30s
DEBUG Found PEP 621 metadata for /Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build in `pyproject.toml` (mini-build)
DEBUG Found static `pyproject.toml` for: mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.13
DEBUG Solving with target Python version: >=3.12.13
DEBUG Adding direct dependency: mini-build*
DEBUG Searching for a compatible version of mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build (*)
DEBUG Tried 1 versions: mini-build 1
DEBUG marker environment resolution took 0.000s
Resolved 1 package in 0.72ms
DEBUG Must revalidate requirement: mini-build
DEBUG Unnecessary package: build==1.6.1
DEBUG Unnecessary package: packaging==26.3
DEBUG Unnecessary package: pip==26.2.1
DEBUG Unnecessary package: pyproject-hooks==1.3.3
DEBUG Computed cache info: Timestamp(SystemTime { tv_sec: 1792484227, tv_nsec: 175475641 }), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1792484142, tv_nsec: 878611230 })))}. Most recently modified: pyproject.toml
   Building mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build
DEBUG Building: mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build
DEBUG Assessing Python executable as base candidate: /Users/maochenxiao/.local/share/uv/python/cpython-3.12-macos-aarch64-none/bin/python3.12
DEBUG Reusing existing build environment for: mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build
DEBUG No workspace root found, using project root
DEBUG Assessing Python executable as base candidate: /Users/maochenxiao/.local/share/uv/python/cpython-3.12-macos-aarch64-none/bin/python3.12
DEBUG Using base executable for virtual environment: /Users/maochenxiao/.local/share/uv/python/cpython-3.12-macos-aarch64-none/bin/python3.12
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.13
DEBUG Solving with target Python version: >=3.12.13
DEBUG Adding direct dependency: setuptools>=77
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=77)
DEBUG Selecting: setuptools==84.2.0 [compatible] (setuptools-84.2.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/dd/a2/2e1af99d56c8145aa69496026c7e5fd7b6ec759b500c3ad697e2062832dc/setuptools-84.2.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.001s
DEBUG Installing in setuptools==84.2.0 in /Users/maochenxiao/.cache/uv/builds-v0/.tmpSxpPUm
DEBUG Registry requirement already cached: setuptools==84.2.0
DEBUG Installing build requirement: setuptools==84.2.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta.get_requires_for_build_editable()`
DEBUG >>> setup.py is executed
DEBUG running egg_info
DEBUG writing src/mini_build.egg-info/PKG-INFO
DEBUG writing dependency_links to src/mini_build.egg-info/dependency_links.txt
DEBUG writing top-level names to src/mini_build.egg-info/top_level.txt
DEBUG reading manifest file 'src/mini_build.egg-info/SOURCES.txt'
DEBUG writing manifest file 'src/mini_build.egg-info/SOURCES.txt'
DEBUG No workspace root found, using project root
DEBUG Locking the source tree for setuptools
DEBUG Calling `setuptools.build_meta.build_editable("/Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l", {}, None)`
DEBUG >>> setup.py is executed
DEBUG running editable_wheel
DEBUG creating /Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info
DEBUG writing /Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info/PKG-INFO
DEBUG writing dependency_links to /Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info/dependency_links.txt
DEBUG writing top-level names to /Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info/top_level.txt
DEBUG writing manifest file '/Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info/SOURCES.txt'
DEBUG reading manifest file '/Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info/SOURCES.txt'
DEBUG writing manifest file '/Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build.egg-info/SOURCES.txt'
DEBUG creating '/Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build-0.1.0.dist-info'
DEBUG creating /Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build-0.1.0.dist-info/WHEEL
DEBUG running build_py
DEBUG running egg_info
DEBUG creating /var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info
DEBUG writing /var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info/PKG-INFO
DEBUG writing dependency_links to /var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info/dependency_links.txt
DEBUG writing top-level names to /var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info/top_level.txt
DEBUG writing manifest file '/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info/SOURCES.txt'
DEBUG reading manifest file '/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info/SOURCES.txt'
DEBUG writing manifest file '/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpqmcmrnhu.build-temp/mini_build.egg-info/SOURCES.txt'
DEBUG
DEBUG         Editable install will be performed using .pth file to extend `sys.path` with:
DEBUG         ['src']
DEBUG
DEBUG Options like `package-data`, `include/exclude-package-data` or
DEBUG `packages.find.exclude/include` may have no effect.
DEBUG
DEBUG adding '__editable__.mini_build-0.1.0.pth'
DEBUG creating '/Users/maochenxiao/.cache/uv/builds-v0/.tmpqqvo6l/.tmp-lyged7we/mini_build-0.1.0-0.editable-py3-none-any.whl' and adding '/var/folders/7g/nmg6_hj17hn31qmn93dt0r400000gn/T/tmpmxzive23mini_build-0.1.0-0.editable-py3-none-any.whl' to it
DEBUG adding 'mini_build-0.1.0.dist-info/METADATA'
DEBUG adding 'mini_build-0.1.0.dist-info/WHEEL'
DEBUG adding 'mini_build-0.1.0.dist-info/top_level.txt'
DEBUG adding 'mini_build-0.1.0.dist-info/RECORD'
DEBUG Built `mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build` into `mini_build-0.1.0-0.editable-py3-none-any.whl`
      Built mini-build @ file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build
Prepared 1 package in 319ms
DEBUG Uninstalled mini-build (10 files, 1 directory)
Uninstalled 1 package in 0.50ms
Installed 1 package in 1ms
 ~ mini-build==0.1.0 (from file:///Users/maochenxiao/workspace/projects/personal/tech-labs/python-labs/mini-build)
```

## Summary

Build frontend + build backend

## Takeaways

uv + setuptools
