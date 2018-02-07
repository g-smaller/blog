### 主题同步问题


##### 关联子模块
> git submodule add [-b keng-customer] git@github.com:username/hexo-theme-next.git themes/NexT

##### 切换分支

> git submodule add -b keng-customer -f git@github.com:g-smaller/hexo-theme-next.git themes/NexT

接下的分支切换时因为上面的命令或产生一个分离

> cd themes/NexT
> git checkout branch

##### 更新
> git submodule update --remote themes/NexT

##### 忽略文件

> git reset HEAD themes/NexT

##### 在另外的终端执行
```
git submodule init // 这句很重要
git submodule update
```

