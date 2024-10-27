# 项目部署

!> LyApi 暂不支持 Composer 快速安装 & 下载。



## 手动部署

> 自己动手，丰衣足食。



#### 下载

在安装前，请确保您的电脑已经安装好了 [GIT](https://git-scm.com/) 工具

从 GitHub 下载 LyApi 最新版本：

```command
git clone https://github.com/lolly-studio/lyapi.git
```



#### 安装

你需要先安装 Composer，然后运行：

```
composer install
```




#### ⏫部署

根据自己所使用的服务器（ Apache or Nginx ）创建相应的服务！

!> 注意：请将根目录设置为 **public** 文件夹，其他目录将无法运行！

#### 伪静态

接下来则需要配置伪静态！

Apache 伪静态配置：

```apache
<IfModule mod_rewrite.c>
 RewriteEngine on
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>
```

Nginx 伪静态配置：

```nginx
if (!-f $request_filename){
	set $rule_0 1$rule_0;
}
if ($rule_0 = "1"){
	rewrite ^/(.*)$ /index.php/$1 last;
}
```

#### 完成

接下来，试着访问本地服务器，如果页面正常的加载出来了！则说明安装已经完成！