---
title: lerna 用法
date: 2022-10-09 16:57:46
tags:
    - 技术
    - 总结
categories: 技术
---

# lerna 用法

> Lerna是一个工具，它优化了使用git和npm管理多包存储库的工作流。

## 概览

### 工作的两种模式

#### Fixed/Locked mode (default)

vue,babel都是用这种，在publish的时候,会在lerna.json文件里面"version": "0.1.5",,依据这个号，进行增加，只选择一次，其他有改动的包自动更新版本号。

#### Independent mode
lerna init --independent初始化项目。
lerna.json文件里面"version": "independent",
每次publish时，您都将得到一个提示符，提示每个已更改的包，以指定是补丁、次要更改、主要更改还是自定义更改。

### 初始化

```bash
$ npm install lerna -g
$ mkdir lerna-gp && cd $_
$ lerna init # 用的默认的固定模式，vue-cli babel等都是这个
```

### 项目结构


```
➜  lerna-gp git:(master) ✗ tree
.
├── lerna.json
├── package.json
└── packages
    ├── daybyday
    │   └── package.json
    ├── gpnode
    │   └── package.json
    └── gpwebpack
        └── package.json

4 directories, 5 files

```
### 配置文件
- package.json
- lerna.json

```
# package.json 文件加入
  "private": true,   // 私有模式不发布
  "workspaces": [
    "packages/*"   // 自定义包路径
  ],

# lerna.json 文件加入
"useWorkspaces": true,   // 使用自定义包路径
"npmClient": "yarn",   // 修改包管理工具

```

## 命令

##### lerna create < name > [loc]

创建一个包，name包名，loc 位置可选

```
# 根目录的package.json 
 "workspaces": [
    "packages/*",
    "packages/@gp0320/*"
  ],
  
# 创建一个包gpnote默认放在 workspaces[0]所指位置
lerna create gpnote 

# 创建一个包gpnote指定放在 packages/@gp0320文件夹下，注意必须在workspaces先写入packages/@gp0320，看上面
lerna create gpnote packages/@gp0320

```

##### lerna add [@version] [--dev] [--exact]

增加本地或者远程package做为当前项目packages里面的依赖


```bash
# Adds the module-1 package to the packages in the 'prefix-' prefixed folders
lerna add module-1 packages/prefix-*

# Install module-1 to module-2
lerna add module-1 --scope=module-2

# Install module-1 to module-2 in devDependencies
lerna add module-1 --scope=module-2 --dev

# Install module-1 in all modules except module-1
lerna add module-1

# Install babel-core in all modules
lerna add babel-core

```

##### lerna bootstrap

默认是npm i,因为我们指定过yarn，so,run yarn install,会把所有包的依赖安装到根node_modules.

##### lerna list

列出所有的包，如果与你文夹里面的不符，进入那个包运行yarn init -y解决。

##### lerna import < path >

![image](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/5/29/16b022e704dafb50~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

##### lerna run < script > -- [..args]

运行依赖包里面的有这个script的命令

```bash
lerna run --scope my-component test
```


##### lerna link

项目包建立软链，类似npm link

##### lerna clean

删除所有包的node_modules目录

##### lerna changed

列出下次发版lerna publish 要更新的包。

原理： 需要先git add,git commit 提交。 然后内部会运行git diff --name-only v版本号，搜集改动的包，就是下次要发布的。并不是网上人说的所有包都是同一个版全发布。


```
➜  lerna-repo git:(master) ✗ lerna changed                                     
info cli using local version of lerna
lerna notice cli v3.14.1
lerna info Looking for changed packages since v0.1.4
daybyday #只改过这一个 那下次publish将只上传这一个
lerna success found 1 package ready to publish

```

##### lerna publish

会打tag，上传git,上传npm。 如果你的包名是带scope的例如："name": "@gp0320/gpwebpack", 那需要在packages.json添加

```
 "publishConfig": {
    "access": "public"
  },
```

##### lerna exec -- < command > [..args]

运行任意命令在每个包


```bash
lerna exec -- rm -rf ./node_modules

lerna exec --scope my-component -- ls -la
```
