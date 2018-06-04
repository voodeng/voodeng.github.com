---
title: Hexo + Travis-CI 自动化部署到 Github
author: Voodeng
tags: []
categories: []
date: 2018-06-04 20:37:00
---


## Hexo

hexo 新建后，改变源文件分支至hexo-source

master分支仅放入hexo g生成的文件



## Travis CI

[travis yaml online lint](http://yaml-online-parser.appspot.com/)

### .travis.yml 部署


CI_TOKEN 是 Github Person access key，需要去 Github 生成，然后在 travis-ci 项目中定义环境变量


- 并不推荐，是强制push，git里只有一条记录

```yml
after_script:
  - cd ./public
  - git init
  - git add .
  - git commit -m "TravisCI Hexo Generate"
  - git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:master 
```


- 先 clone 再 commit，避免直接 force commit，brance 会延续 commit， 并 commit 信息

```yml
after_success:
  - git clone -b master "https://${GH_REF}" .deploy_git
  - cd .deploy_git
  - git checkout master
  - mv .git/ ../public/
  - cd ../public
  - git add .
  - git commit -m "Site updated ${STAMP}"
  - git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:master
  
env:
 global:
   # Github Pages
   - GH_REF: github.com/voodeng/voodeng.github.com.git
   # 时间是根据服务器来的
   - STAMP: $(date +%Y-%m-%d-%H:%M:%S)
```

- 分离的脚本

```bash
#!/bin/bash
set -ev
export TZ='Asia/Shanghai'

git clone -b master "https://${GH_REF}" .deploy_git

cd .deploy_git
git checkout master
mv .git/ ../public/
cd ../public

git add .
git commit -m "Site updated: `date +"%Y-%m-%d %H:%M:%S"`"
git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:master
```


### 使用 hexo-deploy

_config.yml 里 
```yml
deploy:
  type: git
  repo: git@github.com:voodeng/voodeng.github.com.git # 这里需要替换下
  branch: master
```

在.travis.yml中，构建过程中替换
```yml
after_success:
  - sed -i'' "/^ *repo/s~git@github\.com~https://${CI_TOKEN}@github.com~" _config.yml
  - hexo deploy
```

还是需要定义一下push过程，不然一样只有一个commit


## Prose.io

![](https://prose.io/img/prose@57.png)

[Prose](https://prose.io) 等在线的工具，只要能访问 Github 分支并新建保存 markdown 文件，保存后即可自动同步哈


### 参考
http://www.qingpingshan.com/m/view.php?aid=387037

https://iamstarkov.com/deploy-gh-pages-from-travis/

https://blessing.studio/deploy-hexo-blog-automatically-with-travis-ci/
