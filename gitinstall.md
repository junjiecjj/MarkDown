默认已正确安装git

# 挑选一个目录，建立本地仓库
例如 ~/gitmanager/~
在此目录下运行指令：git init
生成.git文件表示建立成功，目录下即为工作区，.git即表示仓库
# 配置用户
运行指令 git config --global user.name "tojia"
         git config --global user.email "hg19940820@163.com"
用于表示由谁提交文件，指示作用，用户名和邮箱名可以随意设置
# 将本地仓库与远程仓库链接
1)免密登录github
输入指令：ssh-keygen -t rsa -C "hg19940820@163.com"
一路enter，生成.ssh/id_rsa.pub文件，复制文件内容
打开github主页，setting->SSH and GPG Keys,新建一个ssh key，将复制内容粘贴进去保存
2)j链接仓库
git remote add origin git@git.com:用户名/仓库名.git
git pull origin master #拷贝master分支中的readme.md文件
git push -u origin master #链接到远程仓库master分支上，也可选择其他分支
git push #上传本地仓库
*过程中要输入gitghub的用户名和token，token需要在github主页上设置
*常有远程拒绝连接的情况，多尝试几次

# 从工作区提交文件到缓存区

在工作区目录下运行指令：git add rebootARM.sh
可以提交多个文件到缓存区
# 提交文件到本地仓库
在工作区目录下运行指令：git commit -m "commit changes"
此指令会将缓存区所有文件提交到仓库，""内是描述说明内容
# 同步本地仓库文件到远程仓库
git push



# 常见问题 

git push 时报错："error:failed to push some refsto '远程仓库地址'"
根本原因
我们在创建仓库的时候，都会勾选“使用Reamdme文件初始化这个仓库”这个操作初识了一个README文件并配置添加了忽略文件。当点击创建仓库时，

它会帮我们做一次初始提交。于是我们的仓库就有了README.m和.gitignore文件，然后我们把本地项目关联到这个仓库，并把项目推送到仓库时，我们在关联本地与远程时，两端都是有内容的，但是这两份内容并没有联系，当我们推送到远程或者从远程拉取内容时，都会有没有被跟踪的内容，于是你看git报的详细错误中总是会让你先拉取再推送，但是拉取总是失败。

解决办法
方法一
对于error: failed to push some refsto‘远程仓库地址’
1 使用如下命令
git pull --rebase origin master

2 然后再进行上传:

git push -u origin master

