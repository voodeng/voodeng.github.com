title: Hexo 自动化部署
author: Voodeng
tags: []
categories: []
date: 2018-06-04 06:37:00
---

[参考1](http://www.qingpingshan.com/m/view.php?aid=387037)

https://iamstarkov.com/deploy-gh-pages-from-travis/

https://blessing.studio/deploy-hexo-blog-automatically-with-travis-ci/

需要实现 仅编辑 markdown 文件，上传至github指定分支后， travis-CI自动执行，生成文件并部署到github指定项目分支.

[travis yaml online lint](http://yaml-online-parser.appspot.com/)

1. 单纯脚本
```
after_script:
  - cd ./public
  - git init
  - git add .
  - git commit -m "TravisCI Hexo Generate"
  - git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:master 
```

会强制push，并且git里只有一条记录

```
after_success:
- .travis/deploy.sh
```

```.travis/deploy.sh
#!/bin/bash
set -ev
export TZ='Asia/Shanghai'

# 先 clone 再 commit，避免直接 force commit
# 不然整个 branch 就总是只有一个 commit，不好看
git clone -b master git@github.com:voodeng/voodeng.github.com.git .deploy_git

cd .deploy_git
git checkout master
mv .git/ ../public/
cd ../public

git add .
git commit -m "Site updated: `date +"%Y-%m-%d %H:%M:%S"`"
git push origin master:master --force --quiet
```

2. 使用 hexo-deploy

有了这个 token 后，原先用

https://github.com/username/repo.git
进行访问，现在换成：

https://<token>@github.com/owner/repo.git

```
after_success:
- sed -i'' "/^ *repo/s~github\.com~${CI_TOKEN}@github.com~" _config.yml
- hexo deploy
```