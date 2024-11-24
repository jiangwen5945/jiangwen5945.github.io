---
title: Hello Hexo
tags: [Hexo]
categories: Hexo
sticky: 999
thumbnail: 'https://jiangwen-blog.oss-cn-fuzhou.aliyuncs.com/images/dd3e880811ebb6e017c2d2eca2.webp'
excerpt: This is the excerpt of the post
date: 2022-09-28 11:45:00
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## 快速开始111

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)


## 进阶优化

### 新增文章时自动打开Markdown编辑器

> 需求：由于每次在使用 `hexo n "文章名称"` 时还要去打开编辑器，这太麻烦了！我们可以通过一个监听的` js`代码去监听`hexo`新建文章的命令，并自动打开相应的 Markdown编辑器来实现联动，这样岂不是即方便且优雅！

1. 首先在 `hexo/scripts` 下新建一个 `editArticle.js` 文件，如果没有 `scripts` 文件可以手动创建一个。

2. 在`editArticle.js` 文件并写入如下内容即可

  ```js
  const { spawn } = require("child_process");
  const os = require("os");
  
  // 新建文档时，启动typora打开文章
  hexo.on("new", (data) => {
    const filename = data.path;
    const typoraPath = getTyporaPath();
  
    if (typoraPath) {
      spawn(typoraPath, [filename]);
    }
  });
  
  // 根据当前的操作系统，设置启动应用的路径（编辑器可执行文件的位置,要根据自己实际情况更改）
  function getTyporaPath() {
    const platform = os.type();
    if (platform === "Darwin") {
      // MacOS
      return "/Applications/Typora.app/Contents/MacOS/Typora";
    } else if (platform === "Windows_NT") {
      // Windows
      return "D:\\Typora\\Typora.exe";
    } else {
      console.error("Unsupported platform:", platform);
      return null;
    }
  }
  
  ```