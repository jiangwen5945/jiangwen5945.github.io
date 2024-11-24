---
title: Hello git
tags:
  - Git
categories:
  - Git
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/cover/4.webp'
excerpt: git从入门到删库
date: 2022-09-28 11:45:00
---

## git起步

##### …在命令行上创建新的存储库

``` bash
echo "# article-img" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:jiangwen5945/article-img.git
git push -u origin main
```

##### …从命令行推送现有存储库

```bash
git remote add origin git@github.com:jiangwen5945/article-img.git
git branch -M main
git push -u origin main
```