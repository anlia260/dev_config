
#   linux 远程复制（scp）(非root)无密码登录

>我与两台Server （ServerA，ServerB）,下面有两个非root用户（user）,我想从ServerA CP 文件test.txt到ServerB,无密码。

如果是root权限（So Easy）：

1. ServerA 生成ssh key，然后加入到 ServerB authorized_keys中

```
[root@ServerA ~]: ssh-keygen -t rsa -P ''

[root@ServerA ~]: ssh-copy-id –i ~/.ssh/id_rsa.pub root@ServerB

[root@ServerA ~]: Enter Pass

```

如果是非root权限（就会有文件权限问题）：

1. ServerA 生成ssh key，然后加入到 ServerB authorized_keys中

```
[user@ServerA ~]: ssh-keygen -t rsa -P ''

[user@ServerA ~]: ssh-copy-id –i ~/.ssh/id_rsa.pub user@ServerB

[user@ServerA ~]: Enter Pass

这个时候我们需要登录ServerB，修改~/.ssh,~/.ssh/authorized_keys的权限

[user@ServerB ~]: chmod 700 ~/.ssh

[user@ServerB ~]: chmod 600 ~/.ssh/authorized_keys

```

# 参考链接

<a href="https://my.oschina.net/u/1258442/blog/180770" target="_blank" title="点击了解更多...">>> 配置ssh免密码登录</a>