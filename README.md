# 《前端志》

本书计划涉及以下几个方面：

- 基础知识；
- 简单算法题；
- 几个短小JS库的源码解析；
- 几个JS框架的介绍；
- 前端面试/笔试题。

有点地方可能自己写得不好，不完善，甚至是不对，希望看发现的朋友能提个issue，或者提个pr。^_^

本书内容采用markdown编写，使用gitbook生成网站静态内容，拥有较好的阅读体验，在线阅读地址：

- [前端志](http://www.lookjavascript.com/)：http://www.lookjavascript.com/


## 源码地址

本项目代码托管于Github上，项目地址：[https://github.com/Yakima-Teng/memo](https://github.com/Yakima-Teng/memo)。


## 意见反馈

由于个人时间、精力和知识深度有限，编写中如有勘误或不妥之处，还请不吝指出。[点此进行反馈](https://github.com/Yakima-Teng/memo/issues)。


## 版权说明

本书为开源项目，可在线免费阅读。未经允许不得用于商业用途。但允许出于学习目的的少量转载，转载请以链接形式注明出处：

- 来源：[前端志](http://www.lookjavascript.com/)：http://www.lookjavascript.com/


## 脚本说明

如果想参与本文档的撰写，下面这些命令说明可能会有些帮助：

- `npm run docs`: 会进入`docs`目录下执行`gitbook serve`，供本地开发时使用

- `npm run installGitPlugins`: 会进入`docs`目录下执行`gitbook install`命令安装书籍需要的gitbook插件

- `npm run deploy`: 等同于先后执行`npm run docs:deploy`命令和`npm run deployToServer`命令

- `npm run docs:deploy`: 将编译后的电子书静态内容发布到远端仓库的`gh-pages`分支上

- `npm run docs:generateSummary`: 自动生成电子书目录（注意，电子书目录请尽量手动生成）

- `npm run deployToServer`: 将电子书静态内容发布到远程服务器上。


## TODO List


- js精度丢失
- vue计算属性和直接在标记语言里使用函数的区别
- vue原理解析
- vue-router的两种模式
- vue-router动态路由
- vue-router切换组件原理
- 正则基础、懒惰模式与贪婪模式
- webStorage
- 函数节流和防抖的区别
