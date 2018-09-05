# 个人博客系统

### 更新
- 提升hoek版本，修复漏洞危险

## 简介
1. 该博客是基于hexo搭建的一个个人博客
## 过程
- 安装
    npm install -g hexo
- 初始化hexo
    hexo init
- 生成静态页面
    hexo generate
- 安装git插件
    npm install --save hexo-deployer-git
- 配置hexo
    - hexo的配置在hexo根目录下的_config.yml文件中
    - 安装hexo admin后台
- 实现RRS功能
    npm install hexo-generator-feed --save
- hexo中实现本地搜索功能
    npm install hexo-generator-searchdb --save
- 在hexo中实现分享功能
    npm i -S hexo-helper-qrcode
- 在hexo中添加字数统计、阅读时长
    npm i --save hexo-wordcount