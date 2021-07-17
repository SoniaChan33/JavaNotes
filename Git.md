# 概念

Git是一个免费的、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的项目。

版本控制：版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统 。

## git本地结构

<img src="C:/Users/Dell/AppData/Roaming/Typora/typora-user-images/image-20210716160147685.png" alt="image-20210716160147685" style="zoom: 33%;" />

## 代码托管中心

我们已经有了本地库，本地库可以帮我们进行版本控制，为什么还需要代码托管中心呢？
它的任务是帮我们维护远程库，下面说一下本地库和远程库的交互方式，也分为两种：

1. 团队内部协作

   <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716160327.png" alt="image-20210716160327295" style="zoom: 50%;" />

2. 跨团队协作

   <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716160355.png" alt="image-20210716160355825" style="zoom:50%;" />

## 初始化本地仓库

* 创建一个文件夹

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716160820.png" alt="image-20210716160820389" style="zoom: 50%;" />

* 打开Git终端：Git Bash Here

* 查看Git安装版本

  ```shell
  git --version
  ```

* 清屏

  ```shell
  clear
  ```

* 设置签名

  ```shell
   git config --global user.name "TeaSea33"
   git config --global user.email "TeaSea33@outlook.com"
  ```

* 本地仓库的初始化操作(目录在本地仓库里)

  ```shell
  git init
  ```

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716162158.png" alt="image-20210716162158453"  />

* 查看.git下的内容

  ![image-20210716162242909](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716162242.png)

  注意事项： .git目录下的本地库相关的子目录和子文件不要删除，不要胡乱修改。

# Git常用命令

## add和commit

- 先创建文件

- 将文件提交到暂存区

  ```shell
  git add Demo2.txt
  ```

- 将暂存区的内容提交到本地库

  ![image-20210716162745742](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716162745.png)

- 注意：

  1. 不放在本地仓库中的文件，git是不进行管理
  2. 即使放在本地仓库的文件，git也不管理，必须通过add,commit命令操作才可以将内容提交到本地库。

## Status

查看工作区和暂存区的状态

- 创建一个文件，然后查看状态

  ```shell
  git status
  ```

  ![image-20210716163201433](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716163201.png)

- 然后将Demo2.txt通过git add命令提交到暂存区

  ```shell
  git add "Demo3.txt"
  ```

- status查看状态

  ![image-20210716163335957](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716163336.png)

- commit提交文件

  ![image-20210716163613057](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716163613.png)

- 修改文件 并查看状态

  ![image-20210716163744634](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716163744.png)

- 重新添加至暂存区 并查看状态

  ![image-20210716163903519](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716163903.png)

- 提交以后查看状态

  ![image-20210716164259073](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716164259.png)

  ![image-20210716164315251](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716164315.png)

## log

可以查看提交的日志，显示最近到最远的日志

### 日志展示方式

1. ```shell
   git log 
   ```

   ![image-20210716164533294](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716165040.png)

   ---> 分页

   当历史记录过多的时候使用**空格**翻页，**b**  回到上一页，到尾页显示END,

   退出：**q**

2. ```shell
   git log --pretty=oneline
   ```

   ![image-20210716165022038](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716165022.png)

3. ```shell
   git log --oneline
   ```

   ![image-20210716165147993](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716165148.png)

4. ```shell
   git reflog
   ```

   多了信息：HEAD@{数字}

   数字：指针回到当前这个历史版本需要多少步

   ![image-20210716165500983](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716165501.png)

## reset

前进或者后退历史版本

- 先查看历史log状态

![image-20210716172535073](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716172535.png)

- 将文件后退到版本1

  ![image-20210716173722034](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716173722.png)

- 再次查看

  ![image-20210716173804875](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716173804.png)

### 三个参数(hard\mixed\soft)

**git reset --[参数] [索引]**

- hard

  本地库的指针移动的同时，重置暂存区，重置工作区

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716190012.png" alt="image-20210716190011958" style="zoom: 33%;" />

- mixed

  本地库的指针移动的同时，重置暂存区，但是工作区不动

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716190039.png" alt="image-20210716190039546" style="zoom:33%;" />

- soft

  本地库的指针移动的时候，暂存区，工作区都不动

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716190052.png" alt="image-20210716190052572" style="zoom:33%;" />

**用得最多的就是第一种hard参数**



### hard

#### 1.找回本地库删除的文件

- 新建一个Test2.txt文件 并将它add到暂存区中

  ![image-20210716191040843](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716191040.png)

- 再通过commit提交到本地库

  ![image-20210716191249904](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716191249.png)

- 删除工作区中的Test2.txt 并将删除操作同步到暂存区

  ![image-20210716191505941](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716191505.png)

- 将删除操作同步到本地库

  ![image-20210716191516088](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716191516.png)

- 查看日志

  ![image-20210716191540549](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716191540.png)

- 找到本地库中删除的文件，实际上就是将历史版本切换到刚才添加文件的那个版本即可

  ![image-20210716191642335](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716191642.png)

#### 2.找回暂存区删除的文件

- 删除工作区数据

  ![image-20210716192257627](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716192257.png)

- 同步到缓存区

  ![image-20210716192305650](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716192305.png)

- 查看日志

  ![image-20210716192331458](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716192331.png)

- 找到当前指针 然后返回到该状态

  ![image-20210716192952612](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716192952.png)

  ![image-20210716193047918](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716193047.png)

## diff

1. 比对**工作区**和**暂存区**

   ```shell
   git diff [文件名]
   ```

   - 添加并提交一个内容为aaaa的Test3.txt	

   ![image-20210716195249530](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716195249.png)

   - 更改工作区的文件加入bbb，再用dif进行比对，发现不同

   ![image-20210716195504098](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716195504.png)

   

   - 如果修改了多个文件，多个文件的比对命令直接是git diff

   ![image-20210716195715409](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716195715.png	)

2. 比对**暂存区**和**本地库** ：

   ```shell
   git diff [历史版本] [文件]
   ```

   - 文件添加不提交到本地库，此时暂存区就和工作区相同为aaaabbb

   - 查看日志

     ![image-20210716202947616](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716202947.png)

     此刻的本地库还停留在之前第一次修改

   - 比对暂存区和本地库中的HEAD当前指针状态

     ![image-20210716203056977](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716203057.png)

     发现不一样，说明没有提交的时候就是不一样的

   - HEAD部分可以改成其他历史版本的标号

     ![image-20210716203405603](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716203405.png)

# 分支

查看分支

![image-20210716205522520](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716205522.png)

创建新分支

![image-20210716205639017](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716205639.png)

切换分支

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716205742.png" alt="image-20210716205742388" style="zoom: 67%;" />

进入branch01分支，增加内容

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716210648.png" alt="image-20210716210648550" style="zoom:50%;" />

![image-20210716210051334](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716210051.png)

再次查看两个分支发现主分支没有改变

![image-20210716210157353](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716210157.png)

查看文件

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716210515.png" alt="image-20210716210515711" style="zoom:50%;" />

将branch01合并到主分支

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716211526.png" alt="image-20210716211526286" style="zoom:50%;" />

因为在同一文件的同一位置修改了文件

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716211737.png" alt="image-20210716211737215" style="zoom:50%;" />

解决冲突：

人为决定，留下想要的就可以，然后再添加

![image-20210716212101854](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716212101.png)

提交冲突，不能带文件名

![](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716212214.png)

![image-20210716212214847](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210716212214.png)





# Github

创建远程库

<img src="C:/Users/Dell/AppData/Roaming/Typora/typora-user-images/image-20210717114129976.png" alt="image-20210717114129976" style="zoom:50%;" />

为远程库取别名

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717114246.png" alt="image-20210717114246916" style="zoom:67%;" />

### 将本地库数据推送到远程库中

![image-20210717114612323](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717114612.png)

![image-20210717114625397](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717114625.png)

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717114728.png" alt="image-20210717114728691" style="zoom:67%;" />

查看远程库

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717114756.png" alt="image-20210717114756656" style="zoom:67%;" />





# SSH免密操作

1. 进入用户的主目录中：

   <img src="C:/Users/Dell/AppData/Roaming/Typora/typora-user-images/image-20210717204258431.png" alt="image-20210717204258431" style="zoom: 67%;" />

2. 执行命令，生成一个.ssh目录

   <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717204402.png" alt="image-20210717204402583" style="zoom: 67%;" />

3. 发现.ssh目录下有两个文件

   ![image-20210717204425794](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717204425.png)

4. 第二个文件里面的密钥复制，去github下新建密钥

   ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDBSmguMvizjJA2oH/DhpjB7O70RE270mUTN+DUCJkJ7FUhMdFvoSoME++YXLMguCwiSXmVXefD/8M+sxoSSlhtiK53+EckxLNiOP/sgogg+L2omyNO2GieV4IcP0JaUNR0iPI5satGNGdbV3lmfca2PmNOjSRwDx3aesLnAgii7m1/z4BtlEqhmmcfMdjgchPQp7jwXvHl9OXjJEcAc5GcB0hjXCTYqsGJTSv7DgeGnxCsV+QsxjS+QJinALtGAd+A555IfkLXGpyU/n1WzCzo65CwVi5Sds1Zhy9KD+PHOPTY2bLgB92k/dIHcFWudxAUIffZl1sIG3AdrzN4fPyMFfDcGMAn+BoNALwiD1kOoql0s1mZD2QlKcNNH0BfBqBk64/NkfYdX/8IGywR5N3VgbcxP0qLh/8NOc39XztIRiqjCoApjx1VlqplC8arSNs5zNSsG5Hx/c4L5p1CXNcOYL9L8uHvVpXW4bvyePUedTwW+JeRh6MrivfnUeAxiss= soniachan33@126.com

5. 密钥生成：

   ![image-20210717144958526](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210717144958.png)

免密操作使每次需要push仓库的时候，都不用再输入密码