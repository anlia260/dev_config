# Linux - SSH免密登陆原理解析

------

## 密码认证

 * 优点：是设置简单，易于理解，通常为默认设置。
 * 缺点：任何人或者机器都可以输入密码。现有的密码字典已经很丰富了。所以普遍的密码认证常常会加上各种手段防止多次试探密码。

## SSH认证 - 公钥访问
* 优点： 可以使用相同的私钥（使用密码）来访问多个系统：不需要记住许多密码，私钥保存在本地文件。（暂且认为是安全可靠的）
* 缺点：需要在目标系统上一次性设置公钥 ， 需要在每个连接上使用秘密密码（如果设置密码的话）来解锁私钥 


> 常见的使用场景是 [Github SSH Key 认证](https://help.github.com/articles/connecting-to-github-with-ssh/)

1. 生成本机```SSH KEY```。一般我们会使用```RSA```加密算法。
2. 把 ```id_rsa.pub``` 放在```Github仓库```中

> 假设本地机器为A，目标服务器为B，如何免密登陆目标服务器呢？

 * 把A机器中 ```id_rsa.pub``` 放在目标服务器B的 ```$ HOME / .ssh / authorized_keys```中，就可以免密登陆了
 * 如果设置```SSH KEY```的时候加了密码，在每次远程登陆的时候还是需要用密码解密私钥。

> 为什么要给本地私钥加密码？

* 加了密码之后就算别人从你的机器拿到私钥也没有用，加强安全性
* 这是重要的但非显而易见的安全优势：由于您现在只负责一个秘密短语而不是许多密码，因此您更频繁地输入密码。这样你就可以更好地记住它，选择更强的密码来保护私钥，可能会阻碍私钥。
 
## SSH认证 - 代理支持（ssh-agent）

> 上一步说了，用户的私钥在每个请求中需要被解锁，有人说了这个密码认证有和区别？

* 用户体验大致相同：但是您将被提示输入密码并非登陆密码。并且，与设置对多台计算机系统（每个可能具有不同密码）的访问不同，使用公钥访问意味着您键入相同的密码，无论您连接到哪个系统。
* 尝试为不同的远程系统记住许多单独的密码是困难的，并不适合挑选强大的密码。公钥访问完全解决了这个问题。

> 当我频繁使用SSH连接的时候，可不可以简化输入解锁私钥密码的步骤了？（可以，```SSH-AGENT```提供这么一个功能）

* ```eval(`ssh-agent`)``` 开启```ssh-agent```的功能。
* ```ssh-add```，你本地的

再一次感谢您花费时间阅读这份欢迎稿，点击 <i class="icon-file"></i> (Ctrl+Alt+N) 开始撰写新的文稿吧！祝您在这里记录、阅读、分享愉快！

作者 [@fed][7]     



  [1]: http://www.unixwiz.net/images/sshpass1.gif
  [2]: http://www.unixwiz.net/images/sshpass2.gif
  [3]: http://www.unixwiz.net/images/sshpass3.gif
  [4]: http://www.unixwiz.net/images/sshpass4.gif
  [5]: https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown
  [6]: https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#cmd-markdown-高阶语法手册
  [7]: mailto:fed.akax@gmail.com
  [8]: http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference