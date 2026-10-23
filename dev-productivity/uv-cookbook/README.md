# uv

## Background

## Experiments

### 虚拟环境

新建：

```
❯ uv venv --python 3.12 --seed --managed-python
Using CPython 3.12.13
Creating virtual environment with seed packages at: .venv
 + pip==26.2.1
Activate with: source .venv/bin/activate
```

激活：

```
❯ source .venv/bin/activate
```

清除并重建：

```
❯ uv venv --clear
Using CPython 3.12.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

删除：

```
❯ rm -rf .venv
```

### 项目管理

项目初始化：

```
workspace/projects/playground
❯ uv init --python 3.12
Initialized project `playground`
```

### 包管理

同步依赖：

```
❯ uv sync
Resolved 1 package in 16ms
Checked in 0.02ms
```

根据`requirements.txt`安装

```
uv pip install -r requirements.txt
```

### SCM

```
❯ uv run --no-project --with setuptools-scm python -m setuptools_scm
```

## Summary


## Takeaways