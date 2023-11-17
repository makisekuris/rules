# 你奶奶都能看明白的 从0.5开始的 node、nvm 体系扫盲

- [你奶奶都能看明白的 从0.5开始的 node、nvm 体系扫盲](#你奶奶都能看明白的-从05开始的-nodenvm-体系扫盲)
  - [node](#node)
    - [什么是 node](#什么是-node)
    - [node 不同版本有什么主要区别](#node-不同版本有什么主要区别)
    - [版本选用原则](#版本选用原则)
    - [开发 web 页面和 node 有什么关系](#开发-web-页面和-node-有什么关系)
  - [nvm](#nvm)
    - [什么是 nvm](#什么是-nvm)
    - [安装与使用](#安装与使用)
      - [Unix、Linux 衍生环境](#unixlinux-衍生环境)
        - [备注](#备注)
      - [windows](#windows)
      - [验证和初步使用](#验证和初步使用)
  - [npm 和其他包管理器（yarn\\pnpm）](#npm-和其他包管理器yarnpnpm)
    - [什么是 npm](#什么是-npm)
    - [使用](#使用)
    - [代理](#代理)
    - [先进的提高生产力的包管理工具](#先进的提高生产力的包管理工具)
      - [yarn](#yarn)
      - [pnpm](#pnpm)
    - [package.json](#packagejson)
      - [查看脚本命令并运行项目（以 vue 为例）](#查看脚本命令并运行项目以-vue-为例)
  - [省流总结](#省流总结)

## node

node 官网（需要翻墙） <https://nodejs.org>

### 什么是 node

javascript 运行时(`runtime`)，现代前端项目开发和运行的基石。C++编写，基于 Google V8 引擎而来。绝大部分情况可以理解为 node 为 javascript（Typescript 在运行时也会转译成 js）在脱离浏览器进行运行（node 服务端、web 项目调试等）时的承载平台，他使用和 chromenium 系 以及其他现代浏览器大体几乎相同的代码解析引擎，但在某些细节上会因代码运行的平台而产生差异。

### node 不同版本有什么主要区别

- EMCA 标准跟进，更多的特性支持，更多的 api 支持。【原则上向前兼容
- 更好的性能
- 安全性与稳定性

### 版本选用原则

- LTS 版本或所在的 LTS 大版本号的最新版本号
- 同时满足相开发生态（vite、webpack、各种插件）硬性要求
- 满足以上前提下越新越好

### 开发 web 页面和 node 有什么关系

web 页面运行 http、js、css 而工作，node 在其中的作用为支持开发人员在开发过程中进行代码调试和验证、将开发代码处理为更适合浏览器加载的形式后方便浏览器展现内容、统一整合处理开发代码用于发布等。

## nvm

nvm github <https://github.com/nvm-sh/nvm>

nvm-windows github <https://github.com/coreybutler/nvm-windows>

### 什么是 nvm

本机 node 运行时的版本管理工具，可以实现在电脑上通过几行命令切换当前 node 环境的版本，用于解决开发不同项目时因为项目要求需要使用对应的 node 环境版本的问题

### 安装与使用

#### Unix、Linux 衍生环境

通常情况下使用 shell 命令进行安装，下载地址和运行命令如下（以 0.39.5 为例），具体变动和细节需要重新查看 github 网站。**安装脚本需要代理访问**

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

运行上述任一命令都会下载脚本并运行。该脚本将 nvm 存储库克隆到 `~/.nvm`，并尝试将下面代码片段中的源代码行添加到正确的配置文件(`~/.bash_profile`, `~/.zshrc`, `~/.profile`, `~/.bashrc`). 。

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

##### 备注

当脚本运行完成后，如果发现运行 `nvm` 没有反应，需要将上述 export 段手动复制加入电脑的环境变量中

_bash_: `source ~/.bashrc`

_zsh_: `source ~/.zshrc`

_ksh_: `. ~/.profile`

#### windows

nvm-windows github <https://github.com/coreybutler/nvm-windows> 下载对应版本的 windows 安装包

#### 验证和初步使用

```sh
$ nvm use 16
Now using node v16.9.1 (npm v7.21.1)
$ node -v
v16.9.1
$ nvm use 14
Now using node v14.18.0 (npm v6.14.15)
$ node -v
v14.18.0
$ nvm install 12
Now using node v12.22.6 (npm v6.14.5)
$ node -v
v12.22.6
```

下载、编译和安装最新版本的 node

```sh
nvm install node
```

安装特定版本

```sh
nvm install 14.7.0 # or 16.3.0, 12.22.1, etc
```

安装的第一个版本将成为默认版本。打开的新的 shell 将以默认版本的 node 启动

ls-remote 列出可用版本：

```sh
nvm ls-remote
```

选择当前使用的 node 版本

```sh
nvm use 16.18.1
```

## npm 和其他包管理器（yarn\pnpm）

### 什么是 npm

`node` 开发的包管理工具。可以理解为 python 的 pip、centos 的 yum、ubuntu 的 wget

### 使用

常用场景为安装其他更好用的包管理器

安装 yarn 包管理器

```sh
npm install yarn -g
```

安装 pnpm 包管理器

```sh
npm install pnpm -g
```

安装所在项目的依赖库（按照所在目录的`package.json`）

```sh
npm install
```

安装该项目开发所需的库并写入当前项目的`package.json`中

```sh
npm install <packageName>
```

### 代理

由于不可抗力的影响在大陆网络环境的代码库下载速度收到了极大的限制，通常情况下有两个选择

`代理`或者`镜像库`

配置 npm 代码仓库

官网 <https://npmmirror.com/>

依照官网使用 cnpm 进行替代或者

```sh
npm config set registry https://registry.npmmirror.com
```

### 先进的提高生产力的包管理工具

npm 的包下载为每个依赖进行重复性下载并且为单线程下载，为解决特性造成的**速度慢**和**占用空间过大**的弊端，通常会使用其他的包管理工具如 `yarn` `pnpm`

#### yarn

yarn 官网 <https://classic.yarnpkg.cn/>

带有验证、以单一项目为单位的重复包缓存、多线程下载的包管理工具

#### pnpm

pnpm 官网 <https://www.pnpm.cn/>

带有验证、以整台计算机为单位的重复包缓存、多线程下载的包管理工具

**_结合办公电脑的硬盘配置和使用体验，建议使用 pnpm 为包管理工具_**

可以理解在常规开发使用过程中 yarn/pnpm 是 npm 的高级替代品，且两者常用命令基本相同

安装该项目开发所需的库并写入当前项目的`package.json`中

```sh
yarn add xxx
# 或
pnpm add xxx
```

其余命令与 npm 基本无异

### package.json

js 项目管理配置文件，记录了该项目的名称、作者、版本号、相关脚本命令、依赖库等信息。一般情况下通常关注相关的脚本命令和依赖库

#### 查看脚本命令并运行项目（以 vue 为例）

通常情况下一个完整的 vue 项目中，package.json 文件一定含有类似的以下结构：

{

```json
  "name": "name",
  "version": "0.0.1",
  "scripts": {
    "dev": "vite",
    "build": "cross-env NODE_ENV=production NODE_OPTIONS=--max-old-space-size=8192 pnpm vite build"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

其中

- `name` 该项目项目名
- `version` 该项目的当前版本号
- `scripts` 和该项目相关的脚本命令
- `dependencies` 项目运行依赖
- `dependedevDependenciesncies` 项目开发依赖

约定俗成的情况是，需要进行开发预览调试的脚本为`dev`,对该项目进行上线文件打 包的命令为`build`。基本实例如下：

运行开发模式
ma

```sh
pnpm run dev
```

执行上线文件打包

```sh
pnpm run build
```

## 省流总结

node 时 js 在非浏览器上的`runtime`，用于 js 生态项目的开发调试运行

nvm 是 node 的开发环境版本管理工具

npm 是 node 的包管理工具，yarn 和 pnpm 是加强版，一般推荐能用 pnpm 用 pnpm

通常一般 vue 项目到手之后默认的安装依赖、运行开发调试操作：

```sh
pnpm install
pnpm run build
```
