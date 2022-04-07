---
title: "MacOS下iterm，Dracula主题配置"
author: curya
date: 2020-11-17
categories: [Blogs, 环境配置]
tags: [环境配置]
math: true
mermaid: true
---

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2653228/1605578918571-1832ee45-980c-4a34-af83-814beecb6732.png#align=left&display=inline&height=988&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1976&originWidth=1740&size=330779&status=done&style=none&width=870)前提：已安装Git和Anaconda环境

- Git：应该是安装Command_Line_Tools_for_Xcode之后即可
- Anaconda：[https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)，从清华镜像站下载安装即可
- 安装brew：参考 [mac安装homebrew失败怎么办？ - 金牛肖马的回答 - 知乎](https://www.zhihu.com/question/35928898/answer/133380744)
```shell
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

- 安装wget：
```shell
brew install wget
```
# 1 安装iterm2
前往[iterm2官网](https://iterm2.com/)下载安装
# 2 安装oh-my-zsh
GitHub仓库地址：[https://github.com/ohmyzsh/ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)
安装方法如下（貌似需要科学~~_skr_~~上网）：
```shell
# curl下载
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# wget下载
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# fetch下载
sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
# 3 安装PowerLine
```shell
pip install powerline-status
```
Note：此处我已经安装过Anaconda
# 4 安装PowerFonts
GitHub仓库：[https://github.com/powerline/fonts](https://github.com/powerline/fonts)
好像很多主题必须得改用Meslo字体，否则会导致显示乱码。
```shell
# 新建文件夹，用来存储相关资源
mkdir -p ~/The/Path/U/Like
cd ~/The/Path/U/Like
# 下载源码
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
# 安装字体
./install.sh
```
安装好字体之后，进入iterm2的Preferences中进行设置
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2653228/1605579858088-21c36cdd-8278-48a9-8e47-b32c8d91cef3.png#align=left&display=inline&height=515&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1030&originWidth=1932&size=203529&status=done&style=none&width=966)
# 5 安装Dracula主题
## 5.1 Dracula for ZSH
GitHub仓库地址：[https://github.com/dracula/zsh](https://github.com/dracula/zsh)
```shell
# 新建文件夹，用来存储Dracula相关资源
mkdir -p ~/The/Path/U/Like/Dracula
cd ~/The/Path/U/Like/Dracula
# 下载主题资源
git clone https://github.com/dracula/zsh.git
# 复制文件
cp ./zsh/dracula.zsh-theme ~/.oh-my-zsh/themes/
# 参考GitHub仓库issue#11，https://github.com/dracula/zsh/issues/11
cp -r ./lib/ ~/.oh-my-zsh/themes/
```
安装完成之后，需要修改~/.zshrc文件，参见[https://draculatheme.com/zsh](https://draculatheme.com/zsh)，如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2653228/1605580362001-6cf15399-3937-45cb-8b02-df228002a345.png#align=left&display=inline&height=146&margin=%5Bobject%20Object%5D&name=image.png&originHeight=292&originWidth=1222&size=54970&status=done&style=none&width=611)
修改之后，激活~/.zshrc文件
```shell
source ~/.zshrc
```
## 5.2 Dracula for iterm2
GitHub仓库地址：[https://github.com/dracula/iterm](https://github.com/dracula/iterm.git)
```shell
cd ~/The/Path/U/Like/Dracula
# 下载主题资源
git clone https://github.com/dracula/iterm.git
```
设置主题，参见[https://draculatheme.com/iterm](https://draculatheme.com/iterm)：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2653228/1605580739709-f5f88603-2614-4ac1-9c20-39f414f0ac1f.png#align=left&display=inline&height=223&margin=%5Bobject%20Object%5D&name=image.png&originHeight=446&originWidth=1240&size=73786&status=done&style=none&width=620)
设置完成之后，如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2653228/1605580892994-59dfa971-5fff-4643-b587-d0d98648e3e4.png#align=left&display=inline&height=716&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1432&originWidth=1924&size=636369&status=done&style=none&width=962)
## 5.3 Dracula for vim
GitHub仓库：[https://github.com/dracula/vim](https://github.com/dracula/vim)
安装参见[https://draculatheme.com/vim](https://draculatheme.com/vim)
```shell
mkdir -p ~/.vim/pack/themes/start
cd ~/.vim/pack/themes/start
git clone https://github.com/dracula/vim dracula
vi ~/.vimrc
```
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2653228/1605581367095-52044f07-3fe6-4d6c-9a03-3a17bb3398a1.png#align=left&display=inline&height=219&margin=%5Bobject%20Object%5D&name=image.png&originHeight=438&originWidth=470&size=49405&status=done&style=none&width=235)
# 6 安装高亮和命令补全插件
高亮插件，GitHub仓库地址：[https://github.com/zsh-users/zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
命令补全插件，GitHub仓库地址：[https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
```shell
cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions
# 修改~/.zshrc文件
vi ~/.zshrc
# 修改之后激活环境
source ~/.zshrc
```
修改内容如下：
```shell
plugins=(
    git
    zsh-autosuggestions
    zsh-syntax-highlighting
)

# 并在文件末尾添加
source ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

参考博客：

- [https://zhuanlan.zhihu.com/p/37195261](https://zhuanlan.zhihu.com/p/37195261)
- [https://blog.csdn.net/daiyuhe/article/details/88667875](https://blog.csdn.net/daiyuhe/article/details/88667875)
