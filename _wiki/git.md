---
layout: wiki
title: git
categories: git
description: git 常用操作记录。
keywords: git, 版本控制
---

## 常用命令

| 功能                      | 命令                                  |
|:--------------------------|:--------------------------------------|
| 添加文件/更改到暂存区     | git add filename                      |
| 添加所有文件/更改到暂存区 | git add .                             |
| 提交                      | git commit -m msg                     |
| 从远程仓库拉取最新代码    | git pull origin master                |
| 推送到远程仓库            | git push origin master                |
| 查看配置信息              | git config --list                     |
| 查看文件列表              | git ls-files                          |
| 比较工作区和暂存区        | git diff                              |
| 比较暂存区和版本库        | git diff --cached                     |
| 比较工作区和版本库        | git diff HEAD                         |
| 从暂存区移除文件          | git reset HEAD filename               |
| 查看本地远程仓库配置      | git remote -v                         |
| 回滚                      | git reset --hard 提交SHA              |
| 强制推送到远程仓库        | git push -f origin master             |
| 修改上次 commit           | git commit --amend                    |
| 推送 tags 到远程仓库      | git push --tags                       |
| 推送单个 tag 到远程仓库   | git push origin [tagname]             |
| 删除远程分支              | git push origin --delete [branchName] |
| 远程空分支（等同于删除）  | git push origin :[branchName]         |
| 查看所有分支历史          | gitk --all                            |
| 按日期排序显示历史        | gitk --date-order                     |

## git commit提交规范

基本格式
```python
<type>(<scope>): <subject>
```

type

用于说明 commit 的类别。

- feat：新功能（feature）
- fix：修补bugfix：修补bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

scope为commit的作用范围，如数据层、视图层，也可以是目录名称

subject必填用于对commit进行简短的描述