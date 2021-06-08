---
title: Node相关工具
date: 2021/6/8
tags: [nvm, npm]
categories: Node
---

# 01 Node 相关工具

## 一：NVM：Node Version Manager

- NVM 为 node 版本管理工具，可切换 Node 的版本

### 安装 Node

> 安装 Node 相关可访问 [我另一篇文章](https://blog.csdn.net/ww19990520/article/details/109710089?spm=1001.2014.3001.5501)

- 官网安装
- nvm 安装 node（推荐）

### NVM 常用命令

- 查看当前 node 版本
  - `node -v`
- 查看 nvm 相关帮助
  - `nvm --help`
- 查看安装过的 node 历史版本信息
  - `nvm list`
- 切换 node 版本
  - `nvm use 14.15.0`
- 设置 NVM 默认版本
  - `nvm alias default 14.15.0`

## 二：NPM：Node Package Manager

- NPM 为 node 包管理工具

### package.json 文件

- `npm init -y` 来自动创建
- 记录当前项目的相关信息，包括依赖包的信息
- 本地安装包，在命令行里执行包的 bash 需要到 node_modules 下的对应模块才能执行，如果在 npm 脚本 scripts 可以即可运行命令行里的命令，可以简化缩写的输入命令行
  - `npm run dev` -> `./node_modules/.bin/gulp -v`
  - 脚本里面会先去全局环境里找命令，找不到的话再去当前 `node_modules` 里的找
    - 故上者可简写为`npm run dev` -> `gulp -v`

```json
{
  "script": {
    "dev": "./node_modules/.bin/gulp -v"
  }
}
```

### 安装包的版本号信息规则

- node package versions

  - 1.14.6
  - major：主版本号->大版本，更新是颠覆性的
  - minor：次版本号->添加新功能、做功能修改
  - patch：补丁号->修补 bug（偶数稳定，奇数不稳定）

- 执行 `npm outdated` 查看当前环境依赖包有哪些是过期的
- jQuery 模拟

```
Current -> 表示当前包版本
Wanted -> 表示想要package.json里配置的包
Current -> 表示最新包版本

Package Current  Wanted  Latest
jquery   1.12.4   1.12.4  3.5.1
```

- 执行 `npm update` 可以将所有包当前版本更新至 Wanted 指定的版本

- 在`package.json`可配置管理需要下载依赖包版本信息（Wanted）

```json
{
  "devDependencies": {
    "jquery": "^1",
    "jquery": "~1.12",
    "jquery": "1.12.4",
    "jquery": "*"
  }
}
```

- 配置版本号：
  - `^1` ：`^`表示只锁定主版本号，后续版本号都默认为最新（1.x.x）
  - `~1.1`：`~`表示锁定主版本号以及次版本号，补丁版本默认为最新（1.1.x）
  - 无符号：表示全版本
  - `*`：直接`*`表示最新版本

### npm 常见命令

1. 安装包

- 安装 package.json 文件里所有记录的包
  - `npm i`
- 安装 package.json 文件里记录的所有生产时依赖包
  - `npm i --production`
- 全局安装
  - `npm install jquery --global`
  - 默认安装最高版本的包
  - 在任意目录都可使用安装包暴露出来的命令
  - 全局安装包目录：window -> `C:\Users\用户名\AppData\Roaming\npm\node_modules`
- 本地安装开发环境包
  - 开发时依赖包
    - `npm i gulp -dev--save`
    - `-dev` 表示安装在开发环境，`--save`表示安装后记录在`package.json`文件（devDependencies）里
  - 生产时依赖包
    - `npm i lodash --save`
    - `--save`表示安装后记录在`package.json`文件（dependencies）里
- 安装指定版本的包
  - 安装 jQuery2.2.4 版本：`npm i jquery@2.2.4 --save`
  - 安装 jQuery1 的最高版本：`npm i jquery@1 --save`
  - 安装 jQuery1.1 的最高版本：`npm i jquery@1.1 --save`

2. 卸载包

- 全局卸载
  - `npm uninstall jquery -g`

3. 查看安装包依赖信息

- 生成所有的包依赖分支树信息
  - `npm list`
  - 筛选出当前包的依赖分支版本信息
    - `npm list | grep packageName`

4. 查看包所有版本信息

- `npm view jquery versions`

5. 清理缓存

- 当第一次安装总出错，可清除缓存之后再安装试试
- `npm cache clean --force`
