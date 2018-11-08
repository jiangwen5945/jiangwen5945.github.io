#### 安装教程
1. 首先要在别人的电脑上面安装好git、node、hexo，然后使用克隆clone的命令下载源代码：
> git clone -b dev-blog git@github.com:xxx/xxx.github.io.git // 克隆分支源码

> npm install hexo    // 安装hexo  安装完成hexo 不需要初始化hexo，否者hexo 配置参数会重置

> npm install    // 安装依赖库

> npm  install hexo-deployer-git  --save    // 安装部署相关配置


2. 接着同步文件源代码到github
> git add .

> git commit -m 'add new blog again'

> git pull origin dev-blog //先拉取仓库的源代码到本地进行合并，解决不同版本的冲突

> git push origin dev-blog  //再将现有的源代码文件push到dev-blog分支



3.最后发布博文,部署静态文件

> hexo g -d

#### 参与贡献

1. Fork 本项目
2. 新建 Feat_xxx 分支
3. 提交代码
4. 新建 Pull Request

