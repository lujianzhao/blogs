### 修改提交信息
```bash
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='serical'; GIT_AUTHOR_EMAIL='raoserical@gmail.com'; GIT_COMMITTER_NAME='serical'; GIT_COMMITTER_EMAIL='614235037@qq.com';" HEAD
    GIT_AUTHOR_NAME='serical'; #新用户名
    GIT_AUTHOR_EMAIL='raoserical@gmail.com'; #新邮箱
    GIT_COMMITTER_NAME='serical'; #老用户名
    GIT_COMMITTER_EMAIL='614235037@qq.com';" #老邮箱
```

### push到远程仓库
```bash
git push --force --tags origin 'refs/heads/*'
```

### 当上面的`push` 不上去的时候，先 `git pull` 确保最新代码，`Gitlab`的分支保护，在项目设置菜单下面的`Protected branches`
```bash
git pull  --allow-unrelated-histories
# 或者指定分枝
git pull origin master ----allow-unrelated-histories
```