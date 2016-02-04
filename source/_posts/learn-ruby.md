title: Learn ruby
date: 2016-01-25 11:40:53
tags: [ruby]
---
 
# 开始学习ruby ! Fighting!

##教程参考
[RailsGuides](http://guides.ruby-china.org/getting_started.html)

## 安装ruby
 mac下已经默认集成了ruby. 如果需要自己手动安装 去[ruby-lang.org](ruby-lang.org)下载安装.
 
检查是否安装成功. 命令行输入

`ruby --version`
 
## 安装rails
`sudo gem install rails` 

如安装成功 输入

`rails --version`

# 搭建第一个ruby on rails blog

刚输入的时候就发现了一个坑. 教程里写直接输入`rails new blog`就行了.
结果运行的时候报错. 需要安装bundle.

`sudo bundle install --path vendor/bundle`

安装之后才好用.

- 启动rails 服务器: `rails server`

rails 程序的视图都存放在 app/views/ 这个目录. 想修改的话直接修改这里面的内容.

修改了里面的welcome/index.html.erb 的文件之后. 保存退出. 

设置修改好的主页 在 `http://localhost:3000` 这里显示. 需要修改config/文件夹中的配置文件.

回到根目录. `cd config/` `vim routes.rb` 把 root 哪一行的注释打开. `:wq`
就保存退出. 这个时候在启动 `rails server`  magic!


## vim 黑魔法


 `:w | !open http://localhost:3000/articles`

- w 写
- | 执行下一个命令
- ! 执行shell命令 open url  直接打开浏览器并刷新