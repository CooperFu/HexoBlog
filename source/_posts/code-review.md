title: 给各种小公司的CodeReview机制建立的建议.
date: 2015-11-30 17:41:56
tags: [Code Review]
---

# 给各种小公司的CodeReview机制建立的建议.


## What is the Code Review


[维基百科官方解释](https://en.wikipedia.org/wiki/Code_review)

[百度百科官方解释](http://baike.baidu.com/link?url=_0mMBXDHHPuI2T9S4fDd8rQy8h4IdBzW190vA1j8BMroGd8uEfLNTfjfk2jmUccOXPjsOrW8iVOjB16Boemx5K)

**人话版:**

 `Code Review`主要是让你的代码可以更好的组织起来，更易读，有更高的维护性，同时可以达到知识共享，找到bug只是其中的副产品。

## Why we do Code Review

- 提高代码质量;
- 及早发现潜在缺弦及`bug`. 降低事故成本;
- 促进团队内部知识共享,提高团队整体水平;
- `Review`过程中对于`Reviewer`来说,也是一种思路重构的过程.


###一些文章
- [腾讯员工对于Code Review的感悟](http://www.kuqin.com/shuoit/20150319/345323.html)
- [从Code Review谈如何做技术](http://kb.cnblogs.com/page/205352/)


## How we do Code Review

下面的是我们公司的正常流程.我们是使用自己搭建的gitlab. 如果贵公司还在用svn可以略过了


- 从`Teambition`上接到任务  请根据实际情况 新建一条分支,新功能请以`feature`开头,修改bug请以`fix`开头,; 
- 请尽量保证多`commit` 多`push` 1次`merge request`;  
- 请不要乱写`commit log`;
- 功能整体完成的时候, 发送`merge request`;
- `Reviewer`请交替进行.比如我提了一个`merge request`. 这次让**daixun**当我的`Reviewer`.下次当我再提`merge request`的时候,就应该找**pengfei**当我的`Reviewer`.

### Android Code Review 具体流程

1. Base on web-server rules.
2. 打开命令行,根据Teambition的任务创建新分支: `git branch feature_add_userpage `
3. 切换到当前分支并开始开发: `git checkout feature_add_userpage`
4. 请尽量多的使用 `git commit -m "add user ok or balabala"` 
5. 请保证`git push`之前 使用了 CheckStyle插件 跑过了你修改或者创建的类. 确保 no problems.
6. 提交到gitlab之后,使用create merge request. 
   - ![](http://ww1.sinaimg.cn/large/c1b2f9f1gw1eyj0jrglmqj20mm07jjs8.jpg)
7. 写明title和desc. assign to处填写你指定谁是你这段代码的`Reviewer`然后`SUBMIT` 
   - ![](http://ww4.sinaimg.cn/large/c1b2f9f1gw1eyj0livpusj20op0tngnz.jpg)
8.    - 如果`Reviewer`看过了之后,审核通过就请勾选`Remove source branch`然后点`ACCEPT`
      - ![](http://ww2.sinaimg.cn/large/c1b2f9f1gw1eyj0og8swij20se0es0uk.jpg)
      - 如果`Reviewer`看过了之后,审核不通过, 请在不通过的代码上写批注  然后`CLOSE`掉.
      - ![](http://ww3.sinaimg.cn/large/c1b2f9f1jw1eyj0yyf5bzj20s50haq50.jpg)
     
     
     
## 一些说明

- 尊重他人，就事论事，对事不对人，毕竟每个人都写过烂代码；
- `merge request`中的每一个`commit log`都应该可以和代码对应，方便 `review`；
- 尽量不要发太大的`merge request`，以免引起 `Reviewer `的恐慌；
- 建议保证一个`merge request`的粒度和专注，最好不要出现一个`merge request`里又有重构又加新`feature`的情况，同样容易引起`Reviewer`的恐慌；
- 提`merge request`之前请确保在本地或测试环境一切正常；
- `merge request`合并者与提交者不能是同一个人；
- `merge request`需从一个特定分支（分支的名字尽量能表达代码的功能）发往上游的 `develop` 分支；
- 芝兰生于空谷，不以无人而不芳. 君子修身养道，不以穷困而改志.