魔方财务系统搭建教程
步骤一：安装宝塔面板

访问宝塔官网下载页面：https://www.bt.cn/new/download.html

根据服务器系统选择安装命令（以CentOS为例）：

bash
yum install -y wget && wget -O install.sh https://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
安装完成后记录面板登录信息

步骤二：安装LAMP环境

登录宝塔面板

进入"软件商店"

安装以下组件：

MySQL 5.7.40

PHP 7.3

Apache/Nginx（任选其一）

步骤三：部署网站

点击"网站" → "添加站点"

填写域名信息（无域名可用IP地址）

上传源码压缩包到网站根目录（通常为 /www/wwwroot/你的域名）

在文件管理器中解压源码

删除原始压缩包

步骤四：配置伪静态

选择目标网站 → 点击"设置"

选择"伪静态"标签

选择"ThinkPHP"规则或手动输入：

text
<IfModule mod_rewrite.c>
 RewriteEngine on
 RewriteBase /
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteRule ^(.*)$ index.php?s=/$1 [QSA,PT,L]
</IfModule>
保存配置

步骤五：设置运行目录

在网站设置中 → 选择"网站目录"

设置运行目录为：/public

取消勾选"防跨站攻击"

保存配置并重启Web服务

步骤六：配置PHP扩展

进入"软件商店" → 找到PHP 7.3 → 点击"设置"

安装扩展：

ionCube Loader

fileinfo

修改配置文件（php.ini），在文件末尾添加：

text
extension = idcsmart.so
重启PHP服务

步骤七：部署授权文件

打开宝塔"文件管理器"

找到授权文件：/魔方财务破解授权扩展文件/扩展/php7.3/idcsmart.so

复制该文件到PHP扩展目录：

text
/www/server/php/73/lib/php/extensions/no-debug-non-zts-20180731/
注意：扩展目录路径可通过命令查找：

text
php -i | grep 'extension_dir'
设置文件权限为644：

bash
chmod 644 /www/server/php/73/lib/php/extensions/no-debug-non-zts-20180731/idcsmart.so
步骤八：完成安装

清除浏览器缓存（或使用无痕模式）

访问您的域名

按照安装向导完成：

数据库配置（使用步骤二创建的数据库信息）

管理员账号设置

系统初始化

安装完成后：

删除install目录

将网站目录权限设置为755

配置定期备份

关键问题排查指南
运行目录错误（步骤五）

症状：访问显示403 Forbidden

解决：确认运行目录设置为/public

PHP扩展未加载（步骤六）

验证方法：创建phpinfo.php文件，内容为<?php phpinfo(); ?>

访问该文件，搜索"ionCube"和"idcsmart"

授权文件问题（步骤七）

终端验证命令：

bash
php -m | grep idcsmart
应返回"idcsmart"

安装后白屏（步骤八）

检查日志：

bash
tail -f /www/wwwlogs/php_error.log
常见原因：

文件权限不足（需755/644）

数据库连接失败

缺少PHP扩展

伪静态不生效

重启Web服务：

bash
# Nginx
systemctl restart nginx
# Apache
systemctl restart httpd
# PHP
systemctl restart php-fpm73
注意事项
生产环境建议使用正版授权

步骤七中的授权文件路径仅为示例，实际路径根据您的文件存放位置调整

安装完成后建议：

修改默认数据库密码

开启宝塔防火墙

设置SSL证书

禁用不使用的PHP函数

定期更新系统和组件以确保安全
