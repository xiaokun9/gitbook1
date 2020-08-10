## git环境准备

### 安装git及配置

1. 安装 [git](http://git-scm.com/download)

   ```
   git config --global user.name "Kun"
   git config --global user.email "596134029@qq.com"
   ```

2. 创建版本库

   ```
   git init
   git add 1.txt
   git commit --message "提交log"
   git status
   git diff 1.txt(也可以通过图形工具查看:kdiff3)
   git rm 1.txt
   git log
   ```

3. Git 协作

   1. 克隆版本库

      ```
      git clone ....
      ```

   2. 获取修改

      ```
      git log --oneline
      git pull (拉取)
      git pull addr master
      git log --graph (图形话log)
      ```

   3. 推送修改

      ```
      git push *.git master
      ```

   4. 

4. 