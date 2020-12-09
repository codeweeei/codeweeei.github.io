---
title: npm和cnpm的安装方法和配置
categories: 前端工具
tags: [node.js,npm,cnpm]
---

## _一：npm 的安装_

### 1：在官网里安装

- 1：在[Node.js 官网](https://nodejs.org/en/)里下载，选择操作系统对应的包，下载完成后双击打开安装，安装时默认安装在 C 盘里，如果 C 盘空间不大可以修改安装位置，我的安装路径为 D:\nodejs ;
  > 注：node 不建议安装在 D:\program files 里，因为中间有空格，在 CMD 里配置环境变量就需要格外留意，需使用%programfiles%来代替 program files ；
- 2：安装完之后，打开 CMD，依次输入 node -v 和 npm -v 回车有显示版本即说明安装成功;
- 3:配置 npm 在安装全局模块时的路径和缓存 cache 的路径:因为在执行例如 npm install webpack -g 等命令全局安装的时候，默认会将模块安装在 C:\Users\用户名\AppData\Roaming 路径下的 npm 和 npm_cache 中，不方便管理且占用 C 盘空间，所以这里配置自定义的全局模块安装目录，在 node.js 安装目录下新建两个文件夹 node_global 和 node_cache，然后在 cmd 命令下执行如下两个命令来配置 node 全局下载包的安装路径：

      npm config set prefix "D:\nodejs\node_global"
      npm config set cache "D:\nodejs\node_cache"

  > 注：执行完输入 npm config ls 来查看 npm 配置路径，如无误，继续配置系统环境变量；

- 4：配置系统环境变量：在系统环境变量（右键此电脑-属性-高级系统设置-环境变量）新建一个变量名为 NODE_PATH，变量值为：D:\nodejs\node_modules ，然后点击 PATH-编辑-新增-D:\nodejs\node_global 即可；因为我们在执行指令时，它会默认在 node 安装根目录下查找指令文件，然后还会在 node 安装根目录下的 node_modules 下查找依赖包文件夹，因为我们之前修改了全局包的存放路径，所以我们需要把我们指定的全局包存放路径添加到系统环境变量；

### 2：使用 nvm 来安装

> 注：使用 nvm 来安装 node 时需要把之前的 node 卸载干净（使用电脑自带的安装卸载程序，然后清空 node 相关的包），卸载干净后按以下步骤来安装与配置

- 1：nvm 为 node 版本控制工具，好处是可以更改 node 的版本，因为 node 会涉及很多版本冲突问题，在 git 里下载 nvm：[下载链接](https://github.com/coreybutler/nvm-windows/releases)，选择 nvm-setup.zip 格式下载；
- 2：解压安装：注意路径不要有中文和空格，首先设置 nvm 的安装路径，然后设置 node.js 的存放路径，install，nvm -v 来验证安装；
- 3：
  - 使用 nvm：下载 node：nvm install 10.8.0 在 nvm install 后面加上你要安装的版本号就可以直接下载；
  - 查看可下载版本：nvm list available
  - 查看已经下载过的版本：nvm list
  - 切换 node 版本：nvm use 11.0.0 直接 nvm use 加上你要切换的版本号（当前使用的 node 版本）
- 4：使用 nvm 切换 node 版本之后可能会出现全局包无法共用的问题，此时可以将 npm 缓存和安装的包的路径挂载到原有缓存路径上：npm config set prefix "原有缓存路径" ； npm config set cache "原有缓存路径" 如有版本问题可使用[npm-check-updates](https://libraries.io/npm/npm-check-updates)工具来进行管理，[使用手册](https://www.cnblogs.com/vickylinj/p/12230374.html)，npm install -g npm-check-updates 下载，然后 ncu -g -u 更行所有全局包,_该方法引用[原处](https://segmentfault.com/q/1010000009977998/a-1020000009978391)_;

## _二：cnpm 的安装_

> 安装 cnpm 前提是 npm 已经安装成功且成功配置好了环境变量，可以看前面教程；

- 1：通过 npm 安装 cnpm，同时将镜像源设置为国内镜像，可增强下载包的速度；

        下载：
        npm install -g cnpm --registry=https://registry.npm.taobao.org
        验证下载：
        cnpm -v

注意事项：下载之后需要查看 cnpm 下载存放的全局地址，是否跟之前配置的全局地址一致 npm config set 的路径，如果是则成功安装，不是的话就需要直接改.npmrc 文件（需打开显示隐藏文件），.npmrc 一般都在 C:\用户\"你的用户名字或 Ad.."\.npmrc
打开之后修改如下即可：

      prefix = D:\nodejs\node_global\node_modules
      cache = D:\nodejs\node_cache

之后确认环境变量配置是否已经配置好 NODE_PATH，前面教程有讲配置过程，已经配置好，就可以全局使用 cnpm 指令了！
