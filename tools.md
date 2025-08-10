[TOC]

## Tools

### Git

#### Git基本配置

下层覆盖上层

- 系统配置 git安装目录下 `git config --system`
- 用户配置 用户目录下 `git config --global`
- 仓库配置 工作目录中 `git config --local`

配置个人身份
在git仓库提交的修改信息中体现
但与认证用的密码无关

`git config --global user.name "xxx"`
`git config --global user.email "xxx"`

**认证配置**

1.生成公钥
`ssh-keygen -t rsa -C xxx@xxx.com`

2.进入代码平台添加公钥

#### Git基本概念

**工程区域**

1.版本库(本地仓库)

2.工作区

3.暂存区

**三种状态**

1.已提交 committed

2.已修改 modified(还没提交)

3.已暂存 staged(把已经修改的文件放在下次准备提交的分区中)

#### 常用命令

1.跟踪文件/存入暂存区

`git add <name>`

2.取消跟踪

`git rm --cache <name>`

3.取消暂存状态

`git reset HEAD <name>`

4.提交

`git commit -m 'xxx'`

5.查看文件状态和当前分支

`git status`

6.添加远程仓库地址

`git remote add origin xxx`

`origin`是给这个远程仓库起的**别名**

7.列出所有已添加的远程仓库别名

`git remote`

8.推送本地master分支的代码到别名origin的远程仓库

`git push origin master`

9.显示当前所有分支

`git branch --list`

10.切换到分支xxx

`git checkout xxx`

11.新建分支xxx

`git branch xxx`

12.新建并切换到这个分支

`git checkout -b xxx`

13.当前分支合并另一个分支xxx

`git merge xxx`

14.当前分支推送到远程仓库master
将这个分支和远程分支master建立绑定关系

`git push -u origin master`

之后只用`git push`即可

15.从远程仓库获取分支

`git fetch origin feature/xrxm`

16.切换分支并在现有分支与指定的远程分支之间建立追踪关系

`git checkout -b feature/xrxm origin/feature/xrxm`

17.当前自己的开发分支xxx合并远程 master 最新修改
(当前还没有未commit的内容)

1.切换到本地master并拉取更新

2.当前xxx分支合并已拉到本地的最新master

```
git checkout master
git pull
git checkout xxx
git merge master
```

18.撤销commit(reset)

`git reset head~ --soft`

head~ 指的是上次的提交

--soft 指的是只是撤销commit,add操作的结果仍然存在

如果是 --hard 则不止撤销commit,还把这次修改的内容丢了(彻底回到上一次提交)

19.rebase

1.让提交历史更干净,更线性,更易读

当你开发完一个功能分支 `feature-x` 时,直接用 `merge` 合并主分支,会产生一条合并记录（merge commit）,历史会出现多条分叉

使用`git rebase master`

可以把你的 `feature-x` 分支“搬运”到 `master` 最新提交之后,仿佛你的开发基于最新代码开始,历史变得更直

2.在多人code review之前

用`git rebase -i HEAD~N`对本地 `N` 个提交进行交互式整理,包括

- 合并多个提交squash
- 改写提交信息reword
- 删除无意义的提交drop

> 这样可以让 PR / MR 更清晰,便于团队审查

20.查看提交记录

`git log`

21.解决冲突

```bash
# 保留自己的 选“ours”
git checkout --ours <filename>

# 放弃自己的 选“theirs”
git checkout --theirs <filename>

# 手动编辑完冲突文件后
git add <filename>
git rebase --continue   # or git merge --continue
```

- Accept Yours (`--ours`)

  被合并的代码中冲突处是旧的,无效的,错误的
  自己在这个分支中写的是对的

- Accept Theirs (`--theirs`)

  rebase到远程主干
  别人的是对的

- 手动合并

  多人修改了同一个文件
  如 config/config.js

#### 代码上库流程

1.对项目仓库Fork一个对应的个人仓

2.进入个人仓库
基于个人仓库clone下载至本地IDE开发

3.开发调试可以基于个人仓库的master
也可以基于个人仓创建分支开发

包括git分支创建,commit和push,以及和个人仓的远程仓库绑定

4.本地开发调试完成后
先commit和push到个人仓库远端

5.需要通过MR的方式将代码合入主仓库

- 先创建MR(合并请求)
- 选择源分支和目标分支(主仓库的master或特性分支)
- 填写MR信息

6.上库需要关联需求AR编码/问题单单号

### Docker

- 镜像 image 包含了你要部署的应用程序和它所关联的库
- 容器 container 其中是一个运行着的镜像
- dockerfile 自动化脚本 创建镜像

```dockerfile
# 加载基础镜像(已经包含了一些包)
FROM python 3.8-slim-bluster

# 指定所有docker命令的工作路径
WORKDIR /app

# 调用COPY命令将所有程序拷贝到DOCKER镜像中
COPY ..

# 运行镜像
RUN pip3 install -r requirements.txt

# 指定容器运行起来以后执行的命令
CMD ["python3", "app.py"]
```

然后用`docker build`命令建立镜像

之后可以用`docker run -p 80:5000 -d my-finance`启动一个容器
(-p配置容器和本地的端口映射 -d表示后台运行)

想保留容器中的数据:使用Docker提供的数据卷
使用`docker volume create`来创建一个数据卷
之后启动容器时通过-v指定将数据卷挂载到容器中哪个路径上

**多个容器协作docker-compose**

需要`docker-compose.yml`

一键启动`docker compose up -d --build`

### Python

空值None

```python
# 列表list相当于 C++ 的 vector
lst = [1, 2, 3]
lst.append(4)
lst[0] = 10
print(lst)  

# 列表推导式
nums = [x*x for x in range(5)]

# 元组tuple类似固定大小的数组,不可变
t = (1, 2, 3)

# 字典dict相当于unordered_map
m = {"a": 1, "b": 2}
m["c"] = 3

# 集合set相当于unordered_set
s = {1, 2, 3}
s.add(4)

# if
if x > 0:
    print("positive")
elif x < 0:
    print("negative")
else:
    print("zero")

# for 遍历
for i in range(5):  # 0~4
    print(i)

# while
while x > 0:
    x -= 1
    
# function
def add(a, b=1):
    return a + b

# class
class Person:
    def __init__(self, name):
        self.name = name
    
    def greet(self):
        print("Hi, I'm", self.name)

p = Person("Alice")
p.greet()

# __main__在 Python 里是一个特殊的模块名,主要用来区分:
# 这个文件是直接用python命令运行的,还是用来被其它文件导入的
# 每个.py文件在运行时都有一个内置变量 __name__
# 如果这个文件是直接运行的，__name__ == "__main__"
# 如果这个文件是被其他文件import，__name__ 就是模块名

if __name__ == "__main__":
# 人为定义了“只有直接运行时才执行这里的代码”
# 类比cpp的main函数
```





