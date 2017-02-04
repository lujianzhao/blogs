```bash
serical@DESKTOP-SCE4JAT MINGW64 /d/workspace/NodeJs/blogs (master)
#要修改哪次提交将pick改成edit
$ git rebase -i HEAD~3
Stopped at 0f65600... Oracle PLSQL配置.md
You can amend the commit now, with

        git commit --amend

Once you are satisfied with your changes, run

        git rebase --continue

serical@DESKTOP-SCE4JAT MINGW64 /d/workspace/NodeJs/blogs (master|REBASE-i 2/3)
#要修改的那个已经到第一个了
$ git log
commit 0f656006ed81ac824911471302b7fb0ff0fa6601
Author: serical <raoserical@gmail.com>
Date:   Sat Feb 4 16:11:58 2017 +0800

    Oracle PLSQL配置.md
    
serical@DESKTOP-SCE4JAT MINGW64 /d/workspace/NodeJs/blogs (master|REBASE-i 2/3)
#修改注释
$ git commit --amend
[detached HEAD af6c3e6] Oracle PLSQL配置
 Date: Sat Feb 4 16:11:58 2017 +0800
 1 file changed, 60 insertions(+)
 create mode 100644 "oracle/Oracle PLSQL\351\205\215\347\275\256.md"
 
 serical@DESKTOP-SCE4JAT MINGW64 /d/workspace/NodeJs/blogs (master|REBASE-i 2/3)
 #修改完成后返回
 $ git rebase --continue
 Successfully rebased and updated refs/heads/master.
 
 serical@DESKTOP-SCE4JAT MINGW64 /d/workspace/NodeJs/blogs (master)
 #pull
 $ git pull
 Merge made by the 'recursive' strategy.
 
 serical@DESKTOP-SCE4JAT MINGW64 /d/workspace/NodeJs/blogs (master)
 #push
 $ git push
 Counting objects: 4, done.
 Delta compression using up to 4 threads.
 Compressing objects: 100% (4/4), done.
 Writing objects: 100% (4/4), 564 bytes | 0 bytes/s, done.
 Total 4 (delta 2), reused 0 (delta 0)
 remote: Resolving deltas: 100% (2/2), completed with 1 local objects.
 To https://github.com/serical/blogs.git
    7670cdc..90ee15c  master -> master
```