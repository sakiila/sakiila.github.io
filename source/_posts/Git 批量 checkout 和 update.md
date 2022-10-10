---
title: Git 批量 checkout 和 update
tags:
  - git
categories: 开发
description: <center>Git 批量 checkout 和 update 脚本。</center>
abbrlink: 1177415153
date: 2022-10-09 15:50:22
---
# 一、下载脚本

脚本代码：
```shell
#!/bin/bash
branch="production"

if [ "$1" ]
then
    branch="$1"
fi

for f in `ls ./`
do
    if [[ ${f} == * ]]
    then
        echo -e "\n${f}"
        cd ${f}

        echo "1. checkout ${branch} result: "
        git checkout ${branch}
        echo "2. pull rebase result: "
        git pull --rebase
        cd ..
    fi
done
echo "Finished"
```

将脚本放置在项目代码同一目录下。


# 二、赋权

进入当前目录，执行赋权命令：  
```shell
chmod +x ./update.sh
```

# 三、运行

注意：执行前，记得 commit 或者 shelve 代码，以防本地代码丢失。  

默认将所有仓库代码 checkout 到 production 分支，并执行 pull --rebase 命令：  
```shell
./update.sh
```
如果需要 checkout 到其他分支，可以加上分支名，例如：  
```shell
./update.sh master
```