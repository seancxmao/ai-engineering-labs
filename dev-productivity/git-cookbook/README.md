# Git Cookbook

## Background


## Experiments

### clone upstream

```
❯ git clone git@github.com:seancxmao/vllm.git

❯ git remote add upstream https://github.com/vllm-project/vllm.git

❯ git fetch upstream

❯ git rebase upstream/main

❯ git push -u origin main
branch 'main' set up to track 'origin/main'.
Everything up-to-date
```

### 状态查看

```
git status
```

```
❯ git branch --show-current
```

```
❯ git log -1 --oneline --decorate
❯ git describe --tags --long --always
❯ git tag --sort=-version:refname | head -20
```

## Summary



## Takeaways

