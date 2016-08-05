#nohup
应用Unix/Linux时，我们一般想让某个程序在后台运行，不要关闭shell窗口程序就停止运行了。

于是我们将常会用 & 在程序结尾来让程序自动运行。比如我们要运行mysql在后台： /usr/local/mysql/bin/mysqld_safe –user=mysql &。

可是有很多程序并不想mysqld一样，这样我们就需要nohup命令，怎样使用nohup命令呢？下面是nohup命令语法:
```
nohup command [ > outfilename ] [ & ]
```
比如我们要在后台运行一个/home/serverstart.sh程序，命令如下：
```
nohup /home/serverstart.sh &
```
在shell中回车后会有如下的提示信息:
```
[~]$ appending output to nohup.out
```

当shell中提示了nohup成功后还需要按终端上键盘任意键退回到shell输入命令窗口，然后通过在shell中输入`exit`来退出终端。

而不要在nohup执行成功后直接点关闭程序按钮关闭终端，直接关闭终端会断掉该命令所对应的session，导致nohup对应的进程被通知需要一起shutdown。

原程序的的标准输出被自动改向到当前目录下的nohup.out文件，起到了log的作用，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件：
```
nohup command > myout.out 2>&1 &
```
在上面的例子中，输出被重定向到myout.out文件中。

**主要是中间的 2>&1的意思：**

这个意思是把标准错误（2）重定向到标准输出中（1），而标准输出又导入文件output里面，所以结果是标准错误和标准输出都导入文件output里面了。

至于为什么需要将标准错误重定向到标准输出的原因，那就归结为标准错误没有缓冲区，而stdout有。这就会导致 >output 2>output 文件output被两次打开，而stdout和stderr将会竞争覆盖，这肯定不是我门想要的。

这就是为什么有人会写成如下命令而出错的原因了
```
nohup ./command.sh >output 2>output
```

最后谈一下/dev/null文件的作用，这是一个无底洞，任何东西都可以定向到这里，但是却无法打开。所以一般很大的stdou和stderr当你不关心的时候可以利用stdout和stderr定向到这里。
```
nohup ./command.sh >/dev/null 2>&1
```
