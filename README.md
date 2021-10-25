# dev-env-config
dev environment config

## 一、ctags的使用
1.生成索引文件
在你想要建立索引文件的目录下执行：ctags -R *
-R: 递归目录生成

2.设置vim查找索引文件路径
在~/.vimrc中，添加：set tags=./tags;,tags
"./" 的意思是，vim解析时，碰到 "./" 会被替换成当前编辑文件的文件夹。
注意第一个 tags后面有一个分号，代表 “向上搜索”，你需要搜索tag的时候，
它会首先在你当前文件所在的文件夹（不是当前文件夹）里面搜索名为 tags的文件，
没有的话，往上一级目录，再没有的话，再往上一级目录，直到搜索到根目录为止。

3.跳转操作
别用 CTRL-] 来跳转定义，多用用下面两个：

```
<C-W>]
<C-W>}
```
能新split出一个窗口来再跳转到定义，比会把当前窗口切换走了的 <C-]> 好用。

## 二、mac管理多个ssh key

1.生成SSH-Key

（1）打开终端，进入到.ssh文件夹内

```bash
  cd .ssh
```

（2）生成ssh-key

```bash
  ssh-keygen -t rsa -C "youremailname" -f id_rsa_hostname
```

在生成ssh-key时，会让输入一个key名，默认是 id_rsa。需要管理多个key的情况下，建议这个key名是自定义，后面跟着域名的key以方便管理和查看。因此key的名字可以以这种方式命名：比如id_rsa_github

（3）设置密码

这个密码可以设置也可以不设置，在这里我是不设置的，当然你也可以进行设置

（4）生成ssh-key

生成ssh-key的时候在 .ssh文件目录下可以看到刚才以 id_rsa_hostname命名的两个文件 —— id_rsa_hostname和 id_rsa_hostname.pub。这两个文件一个是私钥一个是公钥

（5）配置ssh-key

打开或者查看公钥文件 —— id_rsa_hostname.pub。复制里面的内容粘贴到需要设置的域名中，如：GitHub，在GitHub设置中添加ssh。将内容粘贴到SSH keys里面。

（6）配置多个ssh-key

在 .ssh文件目录下创建一个config文件,编辑文件：

```bash
  cd .ssh
  vim config
```

将配置的内容添加进去，以下是需要添加的内容：

```ruby
  # github
  Host github.com
  HostName github.com
  # github对应的email或者用户名
  User Rosalindjuan
  PreferredAuthentications publickey
  # github对应的私钥
  IdentityFile ~/.ssh/id_rsa_github

  # coding
  Host git.coding.net
  # coding对应的email
  User youremail
  PreferredAuthentications publickey
  # coding对应的私钥
  IdentityFile ~/.ssh/id_rsa_coding
```

（7）测试ssh-key是否成功添加

```css
  ssh -T git@github.com
```

如果提示： Hi Rosalindjuan! You've successfully authenticated, but GitHub does not provide shell access.
 那么ssh-key将添加成功

以此类推，如果有新的ssh-key需要管理，那么生成ssh之后，配置一下config文件即可

参考：https://www.jianshu.com/p/194f787998c1

## 三、github git push需要输入用户名和密码

解决方法：

```
执行git remote -v，查看git clone用的是https而不是ssh
git remote set-url origin git@github.com:username/repo.git
git remote -v
```



参考：https://stackoverflow.com/questions/6565357/git-push-requires-username-and-password



