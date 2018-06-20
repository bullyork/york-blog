---
title: mac下执行定时任务之动态生成git分支
date: 2018-05-08 23:36:25
tags: mac git
---

### 问题

我在 5 月 7 号创建了一个 qa/20180509 分支，然而 5 月 9 号并不是一个发版日（我们的发版日是二，四，日）。最后重新从 master 拉了一个 qa/20180508 解决问题，这样会导致其他同学要把自己代码重新合到新的 qa/20180508 分支，这，很有问题。如果项目开发人员很多，这样就要浪费掉很多的沟通成本。解决问题有两种，一是细心一点，发版日的前一个工作日早上上班立马创建一个正确版本的分支。二是用程序解决这个问题，让一切自动化的执行。

### 分析问题

我选择第二个方案，因为我粗心，而且懒。。。

**一、创建 qa 分支的逻辑**

创建 qa 分支的目的是为了测试用于发布的代码。如果提前二个版本日创建，例如周二创建周四的 qa 分支，这会导致周二的测试代码同步到周四，但周二大部分测试代码周二就会被发掉，也就是说合并到 master。而且 master 也会有一些 hotfix，这些都不会同步到周四的测试分支上，这很僵硬。所以最好就是提前一个版本创建。也就是说最好周三早上创建一个从 master 出来的干净的测试分支。

**二、自动化方式的逻辑**

那么怎么通过自动化解决呢？如果开机启动，执行一次，那么 mac 如果不关机，下次就会失效，所以最好是连上公司 wifi 的那一刻，执行自动化的脚本。还有一种简单粗暴的办法是，每隔一段时间执行一次，我这里暂时使用了这种粗暴的方法，每隔一个小时执行一次脚本。因为检测公司 wifi 这个还要细探。

**三、自动化脚本的逻辑**

脚本使用的是 shell 脚本，用其他脚本最后还要使用 shell 调用，感觉更麻烦些，所以直接采用 shell 脚本

1.  判断当前是不是发版日
2.  如果是，判断下个版本的 qa 分支有没有远端分支；如果否，退出
3.  如果是，退出；如果否，判断下个版本的 qa 分支有没有本地分支
4.  如果是，直接推到远端(逻辑不严谨，但情况不多，有的话是人为的)。如果否，切换到 master，拉取最新代码，创建本地分支，推送远端。

**四、自动化工具**

[利用 Launchd 定制 Mac 启动任务](https://sspai.com/post/37258)

### 实现

cd ~/Library/LaunchAgents

touch com.baidi.plist

launchctl load com.baidi.plist

launchctl start com.baidi.plist

plist 内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.baidi.wificheck</string>
        <key>Program</key>
        <string>/Users/xxc/pdd/study/auto/run.sh</string>
        <key>StartInterval</key>
        <integer>3600</integer>
        <key>RunAtLoad</key>
        <true/>
    </dict>
</plist>
```

run.sh 内容如下

```sh
#!/bin/bash
editionDay(){
  for DAY in 2 4 0
  do
    WEEK_DAY=$(date +%w)
    if test $WEEK_DAY -eq $DAY
    then
      return 1
    fi
  done
  return 0
}
echo "-----函数开始执行-----"
WEEK_DAY=$(date +%w)
editionDay
if test $? = 0
then
  EDITION=''
  if test $WEEK_DAY -eq 5
  then
    EDITION=$(date -v +2d +%Y%m%d)
  else
    EDITION=$(date -v +1d +%Y%m%d)
  fi
  QABRANCH='qa/'$EDITION
  cd /Users/xxc/pdd/mobile-merchant
  RESULT=$(git branch -r | grep $QABRANCH)
  RESULT2=$(git branch | grep $QABRANCH)
  if [ ! $RESULT ]
    then
    if [ ! $RESULT2 ]
      then
      git checkout master
      git pull
      git checkout -b $QABRANCH
      git push --set-upstream origin $QABRANCH
    else
      git checkout master
      git pull
      git checkout $QABRANCH
      git rebase master
      git push --set-upstream origin $QABRANCH
    fi
  else
    echo '远端分支已存在'
  fi
fi
echo "-----函数执行完毕-----"
```

### 其他

改进一下可以自动生成更多版本控制的分支，比如 dev or release。自动化可以做很多事情， 比如：定时提醒自己去美餐网点餐。想要做的可以搞起来哦。
