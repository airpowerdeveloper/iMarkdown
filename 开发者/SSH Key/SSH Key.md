1. 首先检查下否存在 ~/.ssh 这个文件夹，如果存在，全部删除，然后下一步

   ```
   rm -rf ~/.ssh
   ```

2. 设置username和e-mail。打开shell，输入：

   ```
   git config --global user.name "LELE"
   git config --global user.email "airpowerdeveloper@icloud.com"
   ```

3. 生成密钥，打开shell，输入：

   ```
   ssh-keygen -t rsa -C "airpowerdeveloper@icloud.com"
   ```

   生成过程中会让你输入一些东西，不用理会，一路回车即可。

   这样就生成了公钥和私钥，默认存储在 ~/.ssh 里，cd进去可以看见生成的id_rsa和id_rsa.pub两个文件

4. 将新生成的key添加到ssh-agent中，打开shell，输入：

   ```
   cat ~/.ssh/id_rsa.pub
   ```

   可以看见输出了一长串字符，全部复制，这就是公钥。