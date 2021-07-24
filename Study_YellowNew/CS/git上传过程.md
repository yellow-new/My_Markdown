# git创建和上传GitHub过程

[上传链接]: https://www.jianshu.com/p/5d00521f187a/
```c#
1.cd 本地工程根目录
git init  //这个目录就变成了git可以管理的仓库

2.git add . //小数点 “.” ，意为添加文件夹下的所有文件；也可以将 “.” 换成具体的文件名，如果想添加项目中的指定文件，那就把 “.” 改为指定文件名即可

3.git commit -m "注释说明"  //将暂存区的文件提交到本地仓库

4.：在 github 或者 gitlab 上创建新的repository

5.git remote add origin https://github.com/xxxxx/xxxxx.git //将本地代码仓库关联到 github 上

6.//在这一步时如果出现错误：
fatal:remote origin already exists
   //那就先输入
 git remote rm origin

7. git push -u origin master // 把当前分支 master 推送到远程，执行此命令后有可能会让输入用户名、密码：
    
    8. 版本显示 git log
    

```

