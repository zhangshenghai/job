# mac php configure 参数

```
$ uname -a
Darwin MacPro.local 13.3.0 Darwin Kernel Version 13.3.0: Tue Jun  3 21:27:35 PDT 2014; root:xnu-2422.110.17~1/RELEASE_X86_64 x86_64

$ ./configure --prefix=/usr \
    --mandir=/usr/share/man \
    --infodir=/usr/share/info \
    --disable-dependency-tracking \
    --sysconfdir=/private/etc \
    --with-apxs2=/usr/sbin/apxs \
    --enable-cli \
    --with-config-file-path=/etc \
    --with-config-file-scan-dir=/Library/Server/Web/Config/php \
    --with-libxml-dir=/usr \
    --with-openssl=/usr \
    --with-kerberos=/usr \
    --with-zlib=/usr \
    --enable-bcmath \
    --with-bz2=/usr \
    --enable-calendar \
    --disable-cgi \
    --with-curl=/usr \
    --enable-dba \
    --enable-ndbm=/usr \
    --enable-exif \
    --enable-fpm \
    --enable-ftp \
    --with-gd \
    --with-freetype-dir=/BinaryCache/apache_mod_php/apache_mod_php-87.2~1/Root/usr/local \
    --with-jpeg-dir=/BinaryCache/apache_mod_php/apache_mod_php-87.2~1/Root/usr/local \
    --with-png-dir=/BinaryCache/apache_mod_php/apache_mod_php-87.2~1/Root/usr/local \
    --enable-gd-native-ttf \
    --with-icu-dir=/usr \
    --with-ldap=/usr \
    --with-ldap-sasl=/usr \
    --with-libedit=/usr \
    --enable-mbstring \
    --enable-mbregex \
    --with-mysql=mysqlnd \
    --with-mysqli=mysqlnd \
    --without-pear \
    --with-pdo-mysql=mysqlnd \
    --with-mysql-sock=/var/mysql/mysql.sock \
    --with-readline=/usr \
    --enable-shmop \
    --with-snmp=/usr \
    --enable-soap \
    --enable-sockets \
    --enable-sqlite-utf8 \
    --enable-sysvmsg \
    --enable-sysvsem \
    --enable-sysvshm \
    --with-tidy \
    --enable-wddx \
    --with-xmlrpc \
    --with-iconv-dir=/usr \
    --with-xsl=/usr \
    --enable-zend-multibyte \
    --enable-zip \
    --with-pcre-regex=/usr
```

```
开发环境的 php 编译参数
./configure --prefix=/home/work/php \
    --enable-fpm \
    --enable-bcmath \
    --enable-ftp \
    --enable-mbstring \
    --enable-soap \
    --enable-sockets \
    --enable-zip \
    --enable-mysqlnd \
    --enable-exif \
    --with-libxml-dir=/usr/lib \
    --with-openssl=/usr/local/ssl \
    --with-pcre-regex=/usr \
    --with-pcre-dir=/usr \
    --with-zlib=/usr \
    --with-bz2=/usr \
    --with-gd=/usr \
    --with-vpx-dir=/usr \
    --with-xpm-dir=/usr \
    --with-jpeg-dir=/usr \
    --with-freetype-dir=/usr \
    --with-mysql=/usr \
    --with-mysqli=/usr/bin/mysql_config \
    --with-pdo-mysql=/usr/bin/mysql_config \
    --enable-sysvsem \
    --enable-sysvmsg
```

```
$ uname -a
Darwin MacPro.local 14.0.0 Darwin Kernel Version 14.0.0: Fri Sep 19 00:26:44 PDT 2014; root:xnu-2782.1.97~2/RELEASE_X86_64 x86_64
./configure --prefix=/usr \
    --mandir=/usr/share/man \
    --infodir=/usr/share/info \
    --disable-dependency-tracking \
    --sysconfdir=/private/etc \
    --with-apxs2=/usr/sbin/apxs \
    --enable-cli \
    --with-config-file-path=/etc \
    --with-config-file-scan-dir=/Library/Server/Web/Config/php \
    --with-libxml-dir=/usr \
    --with-openssl=/usr \
    --with-kerberos=/usr \
    --with-zlib=/usr \
    --enable-bcmath \
    --with-bz2=/usr \
    --enable-calendar \
    --disable-cgi \
    --with-curl=/usr \
    --enable-dba \
    --with-ndbm=/usr \
    --enable-exif \
    --enable-fpm \
    --enable-ftp \
    --with-png-dir=no \
    --with-gd \
    --with-jpeg-dir=/BinaryCache/apache_mod_php/apache_mod_php-93~55/Root/usr/local \
    --enable-gd-native-ttf \
    --with-icu-dir=/usr \
    --with-ldap=/usr \
    --with-ldap-sasl=/usr \
    --with-libedit=/usr \
    --enable-mbstring \
    --enable-mbregex \
    --with-mysql=mysqlnd \
    --with-mysqli=mysqlnd \
    --without-pear \
    --with-pear=no \
    --with-pdo-mysql=mysqlnd \
    --with-mysql-sock=/var/mysql/mysql.sock \
    --with-readline=/usr \
    --enable-shmop \
    --with-snmp=/usr \
    --enable-soap \
    --enable-sockets \
    --enable-sysvmsg \
    --enable-sysvsem \
    --enable-sysvshm \
    --with-tidy \
    --enable-wddx \
    --with-xmlrpc \
    --with-iconv-dir=/usr \
    --with-xsl=/usr \
    --enable-zend-multibyte \
    --enable-zip \
    --with-pcre-regex=/usr
```

php 版本升级变化,编译参数会有变化,粘别人的编译参数,或者沿用旧的记录都有问题,从网上粘的问题就更大了.需要视当前版本支持的参数变动.

php5.3 | php5.5
------ | ------
--enable-ndbm |  --with-ndbm
--without-pear | --without-pear <br /> --with-pear=no

# php 开发接口时空数组和空对象

现象描述:
和后端约定 ext 字段是对象,但初始化为 array(),如果后端有没有结果,最终给 app 输出 json 时 ext 的值变成了 [],即变成数组了.

解决方法:
将 ext 初始化对象 new ArrayObject().

# php 低版兼容

> 不用也可,只做个记录,毕竟用低版的概率很小了

```php
if (PHP_VERSION < '4.1.0')
{
    $_GET    = & $HTTP_GET_VARS;
    $_POST   = & $HTTP_POST_VARS;
    $_COOKIE = & $HTTP_COOKIE_VARS;
    $_SERVER = & $HTTP_SERVER_VARS;
    $_ENV    = & $HTTP_ENV_VARS;
    $_FILES  = & $HTTP_POST_FILES;
}
```

# php 中验证电子邮箱地址是否合法

一上来很自然会想到正则,今天刚好有个同事遇到 phpmailer 的一个问题,就顺便翻了下其源码,发现原来除了正则,还有更好的方法.源码摘抄如下:

```php
function ValidateAddress($address)
{
    if (function_exists('filter_var'))
    { //Introduced in PHP 5.2
        if(filter_var($address, FILTER_VALIDATE_EMAIL) === FALSE)
        {
            return false;
        }
        else
        {
            return true;
        }
    }
    else
    {
        return preg_match('/^(?:[\w\!\#\$\%\&\'\*\+\-\/\=\?\^\`\{\|\}\~]+\.)*[\w\!\#\$\%\&\'\*\+\-\/\=\?\^\`\{\|\}\~]+@(?:(?:(?:[a-zA-Z0-9_](?:[a-zA-Z0-9_\-](?!\.)){0,61}[a-zA-Z0-9_-]?\.)+[a-zA-Z0-9_](?:[a-zA-Z0-9_\-](?!$)){0,61}[a-zA-Z0-9_]?)|(?:\[(?:(?:[01]?\d{1,2}|2[0-4]\d|25[0-5])\.){3}(?:[01]?\d{1,2}|2[0-4]\d|25[0-5])\]))$/', $address);
    }
}
```

# 为 composer 提速
[gc_disable()](http://php.net/manual/zh/function.gc-disable.php)
[参考](http://segmentfault.com/q/1010000002402696)
[github commit](https://github.com/composer/composer/commit/ac676f47f7bbc619678a29deae097b6b0710b799)

Can be very useful for big projects, when you create a lot of objects that should stay in memory. So GC can't clean them up and just wasting CPU time.

composer 在运行的时候会创建大量的对象，这些对象会触发 GC 机制，而这些对象需要被使用，所以 GC 无法清除，因此，使用 gc_disable 禁用 GC 之后，会节省 cpu 时间，效率更高。

# mac下编译 php5.6

```
编译参数:
./configure --prefix=/Users/hy0kl/php \
    #--disable-dependency-tracking \
    --enable-cli \
    --with-libxml-dir=/usr \
    --with-openssl=/usr \
    --with-kerberos=/usr \
    --with-zlib=/usr \
    --enable-bcmath \
    --with-bz2=/usr \
    --enable-calendar \
    --disable-cgi \
    --with-curl=/usr \
    --enable-dba \
    --with-ndbm=/usr \
    --enable-exif \
    --enable-fpm \
    --enable-ftp \
    --with-gd \
    --with-jpeg-dir=/Users/hy0kl/local \
    --with-png-dir=/Users/hy0kl/local \
    --enable-gd-native-ttf \
    --with-icu-dir=/usr \
    --with-ldap=/usr \
    --with-ldap-sasl=/usr \
    --with-libedit=/usr \
    --enable-mbstring \
    --enable-mbregex \
    --with-mysql=mysqlnd \
    --with-mysqli=mysqlnd \
    --without-pear \
    --with-pear=no \
    --with-pdo-mysql=mysqlnd \
    --with-readline=/usr \
    --enable-shmop \
    --with-snmp=/usr \
    --enable-soap \
    --enable-sockets \
    --enable-sysvmsg \
    --enable-sysvsem \
    --enable-sysvshm \
    --with-tidy \
    --enable-wddx \
    --with-xmlrpc \
    --with-iconv-dir=/usr \
    --with-xsl=/usr \
    #--enable-zend-multibyte \
    --enable-zip \
    --with-mcrypt=/Users/hy0kl/local \
    --with-pcre-regex=/Users/hy0kl/local

configure出错:
checking whether to enable JIS-mapped Japanese font support in GD... no
If configure fails try --with-vpx-dir=<DIR>
configure: error: jpeglib.h not found.

1. 下载 libjpeg http://libjpeg.sourceforge.net/
2. ./configure --enable-shared
编译时报个错: configure: error: cannot find macro directory `m4'
创建个 m4 的目录即可.源码包里面没带这个目录
但安装成功后出以下错:
$ pbcopy -v
dyld: Symbol not found: __cg_jpeg_resync_to_restart
  Referenced from: /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libTIFF.dylib
  Expected in: /Users/hy0kl/local/lib/libJPEG.dylib
 in /System/Library/Frameworks/ImageIO.framework/Versions/A/Resources/libTIFF.dylib
Trace/BPT trap: 5
貌似其他命令没有受到影响.

3. If configure fails try --with-vpx-dir=<DIR>
checking for jpeg_read_header in -ljpeg... yes
configure: error: png.h not found.

下载 libpng http://libpng.sourceforge.net/index.html
编译安装即可

4. checking for mcrypt support... yes
configure: error: mcrypt.h not found. Please reinstall libmcrypt.
下载对应的 lib 库 ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt
标准安装.

执行完报个警告:
configure: WARNING: unrecognized options: --disable-dependency-tracking, --enable-zend-multibyte
查看 php5.6 的编译参数时,发现这两个参数已经不存了.拿掉不支持参数再走一遍.

configure 过了后 make 通常问题不大,但若真出问题,那可不好折腾了,换思想吧.
```

# yaf 相关点收集

## yaf 下注册本地类

在 Bootstrap.php 中初始化

```php
/* 注册本地类名前缀, 这部分类名将会在本地类库查找 */
Yaf_Loader::getInstance()->registerLocalNameSpace(
    array(
         'Tool',
         'Cache',
         'Page',
         'Model',
    )
);
```

## yaf 导入预置类

在 Bootstrap.php 中初始化

```php
Yaf_loader::import(APPLICATION_PATH . '/application/actions/BaseAction.php');
```

## yaf 禁止自动渲染模板

```php
Yaf_Dispatcher::getInstance()->disableView();
```

也可以在 `Bootstrap.php` 的 _init 方法中设置

```php
    // 禁止自动渲染
    Yaf_Dispatcher::getInstance()->autoRender(false);
```

## yaf 中 ajax, iframe 页面出现反复刷新或闪动现象

```
原因是 yaf 框架代理页面时,没有遵行浏览器头缓存协议,在对应的 action 中加入以下代码可解决:
$this->getView()->setLayout(null);
```

## yaf 从 controller 层关闭自动渲染模板

在 controller 加入以下代码

```php
    public function init()
    {
        Yaf_Dispatcher::getInstance()->disableView();
    }
```

或者在 action 执行完成后 `return false;`

## [Yaf 多模块开发](https://segmentfault.com/a/1190000002599259)

### 新建模块
在目录`application/`下新建目录`modules`。除了默认模块，其他模块都放在`application/modules/`下。

新建一个模块，模块名自定义。假设我的新模块叫Api吧。

创建目录`application/modules/Api`。

修改项目配置文件`conf/application.ini`：

```ini
; 多个模块，使用逗号分隔
application.modules = "Index,Api"
```

### 在新模块下创建控制器
在目录`application/modules/Api/`下创建控制器目录`controllers`，用于存放模块`Api`下的控制器文件。

新建文件`application/modules/Api/controllers/Passport.php`：

```php
<?php

class PassportController extends Yaf_Controller_Abstract {

    public function loginAction() {
        echo '我是登录接口';
        return false;
    }

}
```

### 效果
浏览器访问：`http://127.0.0.1/api/passport/login`

输出：我是登录接口

# libsvm

```
----------------------------------------------------------------------
Libraries have been installed in:
   /Users/hy0kl/src/php-ext/svm-0.1.9/modules

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the `DYLD_LIBRARY_PATH' environment variable
     during execution

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
```

# PDO 和单例模式

低版本 mysql, pdo,使用数据库单例模式,出现多条 sql 执行后,后续的 sql 不报错,不抛异常,却拿不到执行结果.将 db 类改为非单例模式,则问题解决.

高版本的 mysql, pdo, db 又必须为单例模式.

环境问题,老大难.

# 优雅操作 php-fpm 的方法

[php-fpm 开启 关闭 重启](http://www.netingcn.com/php-fpm-start-stop-reload.html)

自php5.3.3开始，php源码中包含了php-fpm，不需要单独通过补丁的方式安装php-fpm，在源码安装的时候直接 configure 中增加参数 –enable-fpm 即可。

所以启动、关闭和重新加载的方式和以前不同，需要使用信号控制：

php-fpm master 进程可以理解一下信号：

```
SIGINT, SIGTERM 立刻终止
SIGQUIT 平滑终止
SIGUSR1 重新打开日志文件
SIGUSR2 平滑重载所有worker进程并重新载入配置和二进制模块

例如：关闭php-fpm
kill -SIGINT `cat /usr/local/php/var/run/php-fpm.pid`

php-fpm 重启
kill -SIGUSR2 `cat /usr/local/php/var/run/php-fpm.pid`

注意：/usr/local/php/var/run/php-fpm.pid 指存储master进程号的文件，这里是默认地址，在配置中可以修改，另外可以使用ps命令找到master的进程号，然后使用 kill 信号 进程号 的方式。
```

# php-5.6.9 开发环境编译参数

```
$ uname -a
Linux work-dev 3.19.0-16-generic #16-Ubuntu SMP Thu Apr 30 16:09:58 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

./configure --prefix=/home/work/php \
    --enable-fpm \
    --enable-bcmath \
    --enable-ftp \
    --enable-mbstring \
    --enable-soap \
    --enable-sockets \
    --enable-zip \
    --enable-mysqlnd \
    --enable-exif \
    --with-libxml-dir=/usr \
    --with-openssl \
    --with-pcre-regex \
    --with-pcre-dir \
    --with-zlib=/usr \
    --with-bz2=/usr \
    --with-gd \
    --with-xpm-dir \
    --with-jpeg-dir \
    --with-freetype-dir \
    --with-mysql \
    --with-mysqli \
    --with-pdo-mysql \
    --enable-sysvsem \
    --enable-sysvmsg
```

# http authorization

```
经分析, http authorization 的 header 头这样构造的:

Authorization: Basic base64_encode(user:password)
```

# php curl post 的坑

```
nginx 可以 post php 数组
apache 需要将数组 http_build_query() 才行.
```

# phMyAdmin wrong

```
"Wrong permissions on configuration file, should not be world writable!"
```

[see](http://stackoverflow.com/questions/18923347/phpmyadmin-permissions-for-config-inc-php)

```
$ chmod 755 config.inc.php
```

# php7 开发环境编译参数

```
./configure --prefix=/home/work/php7 \
    --enable-fpm \
    --enable-bcmath \
    --enable-ftp \
    --enable-mbstring \
    --enable-soap \
    --enable-sockets \
    --enable-zip \
    --enable-mysqlnd \
    --enable-exif \
    --with-libxml-dir=/usr/lib \
    --with-openssl \
    --with-pcre-regex \
    --with-pcre-dir \
    --with-zlib=/usr \
    --with-bz2=/usr \
    --with-gd \
    --with-curl \
    --with-xpm-dir \
    --with-jpeg-dir \
    --with-freetype-dir \
    --with-mysqli \
    --with-pdo-mysql \
    --enable-sysvsem \
    --enable-sysvmsg
```

# [zend_mm_heap corrupted](http://stackoverflow.com/questions/2247977/what-does-zend-mm-heap-corrupted-mean)

```
After much trial and error, I found that if I increase the output_buffering value in the php.ini file, this error goes away
```

# windows 开发中的坑

utf-8编码还分有BOM和无BOM,坑的不要不要的,好在`git diff`能查看到.windows,你值得远离!!!

# APM

[https://pecl.php.net/package/apm](https://pecl.php.net/package/apm) [https://github.com/patrickallaert/php-apm](https://github.com/patrickallaert/php-apm)
