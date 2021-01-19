# 个人搭建博客的形式

目前比较流行的搭建博客的形式分为三种。各有不同程度的技术门槛、功能支持、主题选择等。

* 使用现有平台
  * 个人微信公众号
  * 知乎专栏
  * CSDN
  * 简书
* 静态网站托管 （托管平台有Github，Coding Pages等）
  * Hexo
  * Jekyll
  * hugo
* 租借虚拟主机搭建博客
  * WordPress
  * ghost					

# Hexo + Github 搭建静态博客

Hexo是一个快速、简洁且高效的博客框架。



## 1. 准备环境

安装Hexo前，先安装下列应用程序：

* [Node.js](https://nodejs.org/en/)	(Node.js版本需不低于10.13)
* [Git](http://git-scm.com/)

安装完成后，在桌面右键打开`Git Bash here`，在命令行中输入相应命令验证是否成功，如果成功会有相应的版本号。

```text
git version
node -v
npm -v
```



## 2. 安装Hexo

如果以上环境准备就绪，就可以按照以下步骤安装Hexo。

在`Git Bash`中输入以下命令：

```text
npm install -g hexo-cli
```

安装完成后，通过输入以下命令来检测是否安装成功。

```text
hexo -v
```

若安装完成，再执行下列命令，Hexo会在指定文件夹中新建所需要的文件。

```text
hexo init "文件夹名"
cd "文件夹名"
npm install
```

新建完成后，指定"文件夹名"的目录如下:

```text
node_modules		#
scaffolds			#模板文件夹。当新建文章时，Hexo会根据scaffold来新建。Hexo的模板是指在新建的markdown文件中默认填充的内						容。(例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改)
source				#资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件					 将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
themes				#主题文件夹
.gitignore			#
_config.yml			#网络配置信息，用Notepad++打开可以配置大部分的参数
package.json		#应用程序的信息。可自由删除。
```

完成上述新建内容后，运行命令：

```text
hexo s
```

s是server的缩写，在浏览器上输入http://localhost:4000回车就可以预览效果了。



## 3. 注册Github

* 登录Github官网: [Github](https://github.com/)，并创建账号
* 创建新仓库create a new repository，这里有一个**硬性规定**，仓库名一定是<u>用户名.github.io</u>, 比如GitHub用户名为didabrother, 那么仓库名就说<u>didabrother.github.io</u>
* 打开仓库的setting，找到GitHub Pages，选择一个主题，这样就创建了一个个人的静态博客，可以通过didabrother.github.io访问



## 4. 配置SSH key

Git是分布式的代码管理工具，远程的代码管理是基于SSH的，所以要使用远程的Git则需要SSH的配置。

**<u>首次Git配置SSH</u>**

一、设置Git的user name和email:

```text
git config --global urse.name"didabrother"
git config --global user.email"邮箱"
```



二、生成SSH:

```text
ssh-keygen -t rsa -C '上面的邮箱'
```

按照提示完成三次回车，即可生成ssh key。

查看SSH:

```text
cd ~/.ssh/id_rsa.pub
```

首次使用还需要确认并添加主机到本地SSH可信列表:

```text
ssh -T git@github.com
```

若返回Hi xxx! You've successfully authenticated, But GitHub does not provide shell access. 则证明添加成功。



三、添加SSH key

* 打开GitHub，点击头像并选择settings
* 选择SSH and GPG keys并新建SSH key



## 5. 部署到Github

连接本地和GitHub。

* 打开项目根目录下的_config.yml配置文件，拉到末尾，按如下配置:

```text
deploy:
  type: git
  repo: http://github.com/didabrother/didabrother.github.io.git
  branch: main
```

* 安装一个部署插件hexo-deployer-git.

```text
npm install hexo-deployer-git --save
```

* 最后执行以下命令就可以部署上传了。g是generate的缩写，d是deploy缩写:

```text
hexo g -d
```

稍等一会，就可以使用[didabrother.github.io](https://didabrother.github.io)访问博客了!



## 6. 开始写作

找到在**2. 安装Hexo**中创建的文件夹，在其目录下输入命令:

```text
hexo new '文章标题'
```

即可创建新的文章。也可手动在根目录/source/_posts下新建**.md**文档。

在Markdown文章(例如Typora)里面编辑内容后执行以下命令:

```text
hexo g
hexo s
```

就可以看到文章在博客上显示。

最后，通过以下命令，讲文章内容部署到GitHub上:

```text
hexo clean		#该步骤的目的是清楚缓存文件(db.json)和已生成的静态文件(public)
hexo g -d
```



## 7. PicGo+GitHub搭建图床

PicGo是一款图片上传工具支持许多图床。采用PicGo+GitHub这种方式作为图床的思路是，将本地的图片或者剪切板的图片通过PicGo上传到GitHub，同时将储存于GitHub中的图片链接输入Markdown，实现异地图片共享。

搭建步骤:

* 在GitHub上创建新的仓库并进行设置
* 配置PicGo与GitHub，实现同步上传



一、GitHub仓库设置

> 流程：新建public仓库->创建token->复制token用于配置PicGo

> 其中创建token步骤：在创建完新仓库后，点击`右上角头像` ->`Developer settings` ->`Personal access tokens`->`Generate new token`->勾选`repo`->复制或储存一串字符token(只会出现一次)



二、配置PicGo

* [PicGo下载](https://github.com/Molunerfinn/PicGo/releases)
* 解压运行安装

运行PicGo，选择`图床设置`->`GitHub图床`，配置如下图:

![](https://raw.githubusercontent.com/didabrother/images/master/img/PicGo配置图.PNG)



至此，PicGo与GitHub配置完成，只需将所需图片在上传区上传，同时在相册中找到对应图片并复制其链接，就可以在Markdown里插入图片。



## 8. 未完待续。。。/to do