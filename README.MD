### 主题同步问题


##### 关联子模块
> git submodule add [-b keng-customer] git@github.com:username/hexo-theme-next.git themes/NexT

##### 修改提交
```
git commit -am "refine UI"
git push origin master
```


##### 在另外的终端执行
```
git submodule init // 这句很重要
git submodule update
```