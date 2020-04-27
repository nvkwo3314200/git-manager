# git-manager

## 这个一个模拟git分枝开发的的项目

[参考文档](https://www.cnblogs.com/lujiangping/p/10910558.html)


## 分支结构


```
|
|-------------------------------------- master (master 为主分支，也是用于部署生产环境的分支，确保master分支稳定性, master 分支一般由develop以及hotfix分支合并，任何时间都不能直接修改代码)
|     |  
|     ------ develop (develop 为开发分支，始终保持最新完成以及bug修复后的代码, 一般开发的新功能时，feature分支都是基于develop分支下创建的)
|           |
|           ----- feature (开发新功能时，以develop为基础创建feature分支, 分支命名: feature/ 开头的为特性分支， 命名规则: feature/user_module、 feature/cart_module)
|           |
|           ----- release (release 为预上线分支，发布提测阶段，会release分支代码为基准提测, 当有一组feature开发完成，首先会合并到develop分支，进入提测时，会创建release分支，如果测试过程中若存在bug需要修复，则直接由开发者在release分支修复并提交。当测试完成之后，合并release分支到master和develop分支，此时master为最新代码，用作上线。)
|
|----- hotfix (分支命名: hotfix/ 开头的为修复分支，它的命名规则与 feature 分支类似, 线上出现紧急问题时，需要及时修复，以master分支为基线，创建hotfix分支，修复完成后，需要合并到master分支和develop分支)
|
```


## 常见任务

1.增加新功能
```
(dev)$: git checkout -b feature/xxx            # 从dev建立特性分支
(feature/xxx)$: blabla                         # 开发
(feature/xxx)$: git add xxx
(feature/xxx)$: git commit -m 'commit comment'
(dev)$: git merge feature/xxx --no-ff          # 把特性分支合并到dev
```
2.修复紧急bug
```
(master)$: git checkout -b hotfix/xxx         # 从master建立hotfix分支
(hotfix/xxx)$: blabla                         # 开发
(hotfix/xxx)$: git add xxx
(hotfix/xxx)$: git commit -m 'commit comment'
(master)$: git merge hotfix/xxx --no-ff       # 把hotfix分支合并到master，并上线到生产环境
(dev)$: git merge hotfix/xxx --no-ff          # 把hotfix分支合并到dev，同步代码
```
3.测试环境代码
```
(release)$: git merge dev --no-ff             # 把dev分支合并到release，然后在测试环境拉取并测试
```
4. 测试
```
(testing)$: git merge release --no-ff          # 测试环境拉取并测试
```
5.生产环境上线
```
(master)$: git merge testing --no-ff          # 把testing测试好的代码合并到master，运维人员操作
(master)$: git tag -a v0.1 -m '部署包版本名'   # 给版本命名，打Tag
(master)$: git push origin v0.1               # 把打好的包推到远程服务器上
```