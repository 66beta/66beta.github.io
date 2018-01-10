---
title: 如何在Github上删除commit
date: 2014-08-11 15:30:08
tags:
  - git
categories:
  - 其他
---
自己写项目，放在Github上，在PC和MAC间同步，可是在MAC上同步一不小心就会出问题，这个应该是Github的MAC客户端有问题。昨天就不小心给同步坏了，本来就感冒了，身心一下了疲惫了。试过Revert到之前一个commit，但是删掉的依然没有找回。<!-- more -->
想来也只能删掉这两个commit（原commit及其Revert）了。正好记录下，下次可能还用得上。

## 在git/Github中删除commit的命令

```bash
git reset --hard <commit_id>
git push origin HEAD --force
```

**git reset –hard** 彻底回退到某个版本
**<commit_id>** 长长的SHA1值
**HEAD** 最近一个提交
需要在命令行中执行，Github客户端没有这个功能。
