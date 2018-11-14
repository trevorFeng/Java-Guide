### Linux用户间免密登录
##### 普通用户免密登录自己

注意：若没有.ssh目录则ssh localhost创建一个

- cd  .ssh
- ls

authorized_keys  id_rsa  id_rsa.pub  known_hosts
- rm -rf  *
- cd ~
- ssh-keygen -t rsa     #执行该命令，一路回车即可，什么都不输入
- cd .ssh
- cp id_rsa.pub authorized_keys 
- chmod 600 authorized_keys  #一定要修改其权限
- ssh localhost            #第一次要求确认，后面可以直接登陆
---
##### 两台机器用户间的免密登录
user1免密登录远程user2
- 将user1的公钥拷贝到user2的任意目录
```
scp ~/.ssh/id_rsa.pub user2@node2:~/Dowanloads/
```
- 将公钥追加到user2的秘钥里

```
cat ~/Dowanloads/id_rsa.pub >> ~/.ssh/authorized_keys
```
