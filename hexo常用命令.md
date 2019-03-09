## 初始化hexo

```shell
$ hexo init <folder>
$ cd <folder>
$ npm install
```

## 新建文件(文章)

```shell
$ hexo new [layout]--->三种模式 <title> --->这是你的标题(文章)
```

### 布局（Layout）

Hexo 有三种默认布局：`post`、`page` 和 `draft`，它们分别对应不同的路径，而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。

### 模版（Scaffold）

在新建文章时，Hexo 会根据 `scaffolds` 文件夹内相对应的文件来建立文件，例如：

```shell
$ hexo new photo "My Gallery"
```

在执行这行指令时，Hexo 会尝试在 `scaffolds` 文件夹中寻找 `photo.md`，并根据其内容建立文章，以下是您可以在模版中使用的变量：

|   变量   |     描述     |
| :------: | :----------: |
| `layout` |     布局     |
| `title`  |     标题     |
|  `date`  | 文件建立日期 |

### 分类和标签

只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 `Foo, Bar` 不等于 `Bar, Foo`；而标签没有顺序和层次。

```shell
`categories:- Diarytags:- PS3- Games`
```

## JSON Front-matter

除了 YAML 外，你也可以使用 JSON 来编写 Front-matter，只要将 `---` 代换成 `;;;` 即可。

```shell
"title": "Hello World",
"date": "2013/7/13 20:46:25"
;;;
```

Front-matter 是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，举例来说：

```shell
title: Hello World
date: 2013/7/13 20:46:25
---
```

以下是预先定义的参数，您可在模板中使用这些参数值并加以利用。

| 参数         | 描述                 | 默认值       |
| ------------ | -------------------- | ------------ |
| `layout`     | 布局                 |              |
| `title`      | 标题                 |              |
| `date`       | 建立日期             | 文件建立日期 |
| `updated`    | 更新日期             | 文件更新日期 |
| `comments`   | 开启文章的评论功能   | true         |
| `tags`       | 标签（不适用于分页） |              |
| `categories` | 分类（不适用于分页） |              |
| `permalink`  | 覆盖文章网址         |              |

并列分类，了解一下：
categories:
\- [Linux]
\- [Tools]

并列 + 子分类，再了解一下：
categories:
\- [Linux, Hexo]
\- [Tools, PHP]

## 生成静态文件

```shell
$ hexo generate
简写：
$ hexo g
-d	文件生成后立即部署网站
-w  监视文件变动	
```

## 发表草稿

```shell
$ hexo publish [layout] <filename>
```

## 启动服务器

```
$ hexo server
```

启动服务器。默认情况下，访问网址为： `http://localhost:4000/`。

| 选项 |              描述              |
| :--: | :----------------------------: |
|  -p  |            重设端口            |
|  -s  |         只使用静态文件         |
|  -l  | 启动日记记录，使用覆盖记录格式 |

# 服务器



## [hexo-server](https://github.com/hexojs/hexo-server)

Hexo 3.0 把服务器独立成了个别模块，您必须先安装 [hexo-server](https://github.com/hexojs/hexo-server) 才能使用。

```shell
$ npm install hexo-server --save
```

安装完成后，输入以下命令以启动服务器，您的网站会在 `http://localhost:4000` 下启动。在服务器启动期间，Hexo 会监视文件变动并自动更新，您无须重启服务器。

```shell
$ hexo server
```

如果您想要更改端口，或是在执行时遇到了 `EADDRINUSE` 错误，可以在执行时使用 `-p` 选项指定其他端口，如下：

```shell
$ hexo server -p 5000
```

### 静态模式

在静态模式下，服务器只处理 `public` 文件夹内的文件，而不会处理文件变动，在执行时，您应该先自行执行 `hexo generate`，此模式通常用于生产环境（production mode）下。

```shell
$ hexo server -s
```

### 自定义 IP

服务器默认运行在 `0.0.0.0`，您可以覆盖默认的 IP 设置，如下：

```shell
$ hexo server -i 192.168.1.1
```

指定这个参数后，您就只能通过该IP才能访问站点。例如，对于一台使用无线网络的笔记本电脑，除了指向本机的`127.0.0.1`外，通常还有一个`192.168.*.*`的局域网IP，如果像上面那样使用`-i`参数，就不能用`127.0.0.1`来访问站点了。对于有公网IP的主机，如果您指定一个局域网IP作为`-i`参数的值，那么就无法通过公网来访问站点。

## Pow

[Pow](http://pow.cx/) 是一个 Mac 系统上的零配置 Rack 服务器，它也可以作为一个简单易用的静态文件服务器来使用。

### 安装

```shell
$ curl get.pow.cx | sh
```

### 设置

在 `~/.pow` 文件夹建立链接（symlink）。

```shell
$ cd ~/.pow
$ ln -s /path/to/myapp
```

您的网站将会在 `http://myapp.dev` 下运行，网址根据链接名称而定。



## 部署网站

```shell
$ hexo deploy
该命令可以简写为：
$ hexo d
```

| 参数 |           描述           |
| :--: | :----------------------: |
|  -g  | 部署之前预先生成静态文件 |

## 渲染文件

```shell
$ hexo render <file1> [file2] ...
```

| 参数 |     描述     |
| :--: | :----------: |
|  -o  | 设置输出路径 |

## 清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)

```shell
$ hexo clean
```

## 列出网站资料

```shell
$ hexo list <type>
```

## 显示 Hexo 版本

```shell
$ hexo version
```

## 显示草稿

```shell
$ hexo --draft
```

