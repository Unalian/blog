---
date: 2020/2/2
tags:
- blog
- 服务器
- hexo
- 自学笔记
categories:
- 计算机网络
- blog搭建（hexo
password: 6235283
abstract: This is pricate passage, please enter the passpord.
message: This is pricate passage, please enter the passpord.
---
# 基于hexo的配置
## 基本结构

hexo的下载和配置参见[官网](https://hexo.io)，有详细的下载，配置，模板信息。并且介绍了文件的结构，对于学习前端知识有很大的帮助。

如下是下载后的文件夹内部结构。其中，source是根目录，_posts存放单个post，格式是.md。

_config.yml存放一些结构文件，其中常修改的是title, author, language

<!-- more -->

```
`.├── _config.yml├── package.json├── scaffolds├── source|   ├── _drafts|   └── _posts└── themes`
```

##  _post

(1) _posts存放单个post，格式是.md。

(2) quote：(修改_posts)

```yaml
`{% blockquote %}Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.{% endblockquote %}`
```

(3) Categories & Tags（修改_posts）

```yaml
categories:
- Sports
- Baseball
tags:
- Injury
- Fight
- Shocking
```

## page

建立新的导航菜单项：

* 前往你的Hexo博客的根目录

* 输入`hexo new page tags`

* 找到`source/tags/index.md`这个文件

* 修改这个文件：

  ```yaml
  ---
  title: 标签
  date: 2018-01-05 00:00:00
  type: "tags"
  ---
  ```

type必须是`tags`，增加`categories`页面的话也是一样的。

* 然后配置melody.yml（即./themes/melody/_config.yml)

  ```yaml
  menu:
    Home: /
    Archives: /archives
    Tags: /tags
    Categories: /categories
  ```

## 节选

<!-- more -->之前的内容将在主页显示

# themes

## 修改主题

主题的配置选用了[melogy](https://molunerfinn.com/hexo-theme-melody-doc/zh-Hans) 

* 在github中下载后，自动配置入themes文件夹中，并为git文件夹（这可能是造成整个blog文件无法push的原因，但是还不确定在一个本地仓库中还有一个仓库是否会影响git push，现状是最后本地可运行的blog文件无法被git push）。

* 配置blog文件中的_config.yml，将themes改为melogy

## 配置_config.yml(./themes/melody/ _config.yml)

theme_color: (修改主题色)

menu：(1.3 修改导航菜单)

favicon: /dog2.ico （修改网站图标 .ico格式）

highlight_theme: light （代码高亮模式 其他模式还有pale night, dark, ocean）

fireworks: true （点击特效，在./themes/melody/source/js/firework.js中可修改特效）

social:github fa: [https://github.com/Unalian]()  (社交网站图标，点击可关联社交媒体。设置详细方法参见主题配置说明网站)

avatar: /rushroom.jpg （头像）

top_img: /lightInDark.jpg # false or url of img

top_img_height: 80 # best range: 60 - 100（顶图设置）

footer_custom_text: to be or not to be （底字）

## layout

.pug格式：所谓的pug就是jade,也就是一种通过缩进的方式来编写代码的过程，在编译的过程中，我们不需要考虑标签是否闭合的问题。此外，用这种编译方式，加快了我们写代码的速度，也为代码复用提供了便捷。

可用以修改网页内容，对这一部分的修改并不多。

# 服务器的建立与使用

## 租用服务器

在k的引导下，租用了阿里云的服务器（轻量级应用服务器）。（云翼计划，学生每月9.8）

设置为系统镜像，系统为CentOS。在terminal访问时的指令是：

```
ssh root@47.***.***.155
```

k解析到了ip.***.monster 上，并且设置了数字签名，让登录更加方便了呢。()

## 修改blog内容:**Rsync**

```
rsync  -av  /test/  /backup --本机上的同步，把/test目录下的内容同步到/backup目录下(包括隐藏文件)

rsync  -av  192.168.1.20:/backup/  /backup/   --把远端192.168.1.20的/backup目录下的内容同步到本地的/backup目录

（注意：路径写法的区别！原目录后面加不加/也影响你的同步目录；没加/，就是将目录本身同步过去；目录加/，就是将目录里的内容同步过去！）

rsync  -av  /home  --exclude=abc  /backup    --将/home目录下除了abc其他内容都同步到/backup目录下

rsync  -a  --delete  /backup/  /test/                 --如果同步后，源主机中有文件删除了，这时要想目标主机与源主机的内容保持一致，可以使用--delete参数进行同步
```

在本机blog文件外 

```
#  rsync  -av  /test/  192.168.1.20:/backup          --把本地的/test目录内容，同步到远端191.168.1.10的/backup目录下
```

# exp

* 最大的教训就是要做好版本控制，在过程中出现了好几次以外的错误，却不知道是哪里出了错，因此以后应该利用git做好版本控制，减少工作清零的风险。
* 这次反复出现的一个问题是系统强制改了一个关键文件，我一直没发现这个问题，最后一次经过对报错的分析终于找到了这个问题。



# v2ray 

![见文档](https://www.v2ray.com/chapter_00/install.html)



最简单的 单也算是自己成功了



![](https://img.cetacis.dev/uploads/big/622e953653dd2222979f2e7ceb8020ec.jpg)