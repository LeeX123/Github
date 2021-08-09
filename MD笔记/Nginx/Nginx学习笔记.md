# Nginx 学习笔记

## 必备软件

### 1.GCC编译器

安装GCC编译器

```bash
yum install -y gcc
```

安装G++编译器

```bash
yum install -y gcc-c++
```

### 2.PCRE库

PCRE（Perl Compatible Regular Expressions，Perl兼容正则表达式）是由Philip Hazel开发 的函数库，目前为很多软件所使用，该库支持正则表达式。

PCRE（Perl Compatible Regular Expressions，Perl兼容正则表达式）是由Philip Hazel开发 的函数库，目前为很多软件所使用，该库支持正则表达式。

yum安装

```bash
yum install -y pcre pcre-devel
```

### 3.zlib库

zlib库用于对HTTP包的内容做gzip格式的压缩，如果我们在nginx.conf里配置了gzip on， 并指定对于某些类型（content-type）的HTTP响应使用gzip来进行压缩以减少网络传输量，那 么，在编译时就必须把zlib编译进Nginx。

yum安装

```bash
yum install -y zlib zlib-devel
```

### 4.OpenSSL开发库

如果我们的服务器不只是要支持HTTP，还需要在更安全的SSL协议上传输HTTP，那么 就需要拥有OpenSSL了。另外，如果我们想使用MD5、SHA1等散列函数，那么也需要安装它。

yum安装

```bash
yum install -y openssl openssl-devel
```

## Linux内核优化

针对最通用的、使Nginx支持更多并发请求的TCP网络参数修改/etc/stsctl.conf，如下：

```bash
fs.file-max = 999999  // 限制最大并发连接数
net.ipv4.tcp_tw_reuse = 1  // 允许将time-wait状态的socket重新用于tcp连接
net.ipv4.tcp_keepalive_time = 600  // tcp发送keepalive消息的频度
net.ipv4.tcp_fin_timeout = 30  // 当服务器主动关闭连接时，socket保持在fin-wait2状态的最大时间
net.ipv4.tcp_max_tw_buckets = 5000  // time_wait套接字最大值
net.ipv4.ip_local_port_range = 1024 61000  // udp和tcp连接中本地端口的取值范围
net.ipv4.tcp_rmem = 4096 32768 262142  // tcp接收缓存（用于tcp接收滑动窗口）的最小值、默认值、最大值
net.ipv4.tcp_wmem = 4096 32768 262142  // tcp发送缓存（用于tcp发送滑动窗口）的最小值、默认值、最大值
net.core.netdev_max_backlog = 8096  // 当网卡接收数据包的速度大于内核处理的速度时，会有一个队列保存这些数据包。这个参数表示队列的最大值。
net.core.rmem_default = 262144  // 内核套接字接收缓存区默认大小
net.core.wmem_default = 262144  // 内核套接字发送缓存区默认大小
net.core.rmem_max = 2097152  // 接收缓存区的最大大小
net.core.wmem_max = 2097152  // 发送缓存区的最大大小
net.ipv4.tcp_syncookies = 1  // 与性能无关，用于解决tcp的syn攻击
net.ipv4.tcp_max_syn_backlog=1024  // 书上是错误的，书中是syn.backlog，表示tcp三次握手建立阶段接收syn请求队列的最大长度
```

## 安装位置

主机中我的nginx-1.20.1安装位置：/usr/local/src/nginx-1.20.1

## 编译安装nginx

进入安装目录后，执行以下命令

```bash
./configure  // 检测系统内核和已经安装的软件，参数的解析，中间目录的生成以及根据各种参数生成一些c源码文件、Makefile文件等
make  // 根据configure命令生成的Makefile文件编译Nginx工程，并生成目标文件、最终的二进制文件
make install  // 根据configure命令执行时的参数将Nginx部署到指定的安装目录，包括相关目录的建立和二进制文件、配置文件的复制。
```

## Nginx的配置

### 运行中的Nginx进程间关系

一个master进程管理多个worker进程，一般情况下，worker进程的数量与服务器上的cpu核心数相等

### 配置项的语法格式

配置项名 配置项值 1 配置项值 2 … ;

> 如果配置项值中包括语法符号，比如空格符，那么需要 使用单引号或双引号括住配置项值，否则Nginx会报语法错误。

### 配置项注释

加“#”

### 配置项的单位

指定空间大小

- K或者k千字节（KiloByte，KB ）

- M或者m兆字节（MegaByte，MB）

指定时间时

- ms（毫秒），s（秒），m（分钟），h（小时），d（天）， w（周，包含7天），M（月，包含30天），y（年，包含365天）。

> 配置项后的值究竟是否可以使用这些单位，取决于解析 该配置项的模块。

### 使用变量

加“$”

### Nginx服务的基本配置

Nginx在运行时，至少必须加载几个核心模块和一个事件类模块。 这些模块运行时所支持的配置项称为基本配置——所有其他模块执行时 都依赖的配置项。

把它们按照用 户使用时的预期功能分成了以下4类：

- 用于调试、定位问题的配置项。 
- 正常运行的必备配置项。 
- 优化性能的配置项。 
- 事件类配置项（有些事件类配置项归纳到优化性能类，这是因为 它们虽然也属于events{}块，但作用是优化性能）。

还有一些不会默认配置的配置项

### 用于调试进程和定位问题的配置项

（1）是否以守护进程方式运行Nginx 语法： 

daemon on|off; 

默认：daemon on;

（2）是否以守护进程方式运行Nginx 语法： 

daemon on|off; 

默认： daemon on;

（3）error日志的设置 语法： 

error_log/path/file level; 

默认： error_log logs/error.log error;

/path/file参数可以是一个具体的文件，例如，默认情况下是 logs/error.log文件，最好将它放到一个磁盘空间足够大的位置；/path/file 也可以是/dev/null，这样就不会输出任何日志了，这也是关闭error日志 的唯一手段；/path/file也可以是stderr，这样日志会输出到标准错误文件中。

level是日志的输出级别，取值范围是debug、info、notice、warn、 error、crit、alert、emerg，从左至右级别依次增大。当设定为一个级别 时，大于或等于该级别的日志都会被输出到/path/file文件中，小于该级别的日志则不会输出。例如，当设定为error级别时，error、crit、alert、 emerg级别的日志都会输出。

### SSL加速

**SSL加速**是一种减轻处理器过多参与[传输层安全协议](https://baike.baidu.com/item/传输层安全协议/15691557)（TLS）的[公开密钥加密](https://baike.baidu.com/item/公开密钥加密)所产生负担的方法，它的前身是[安全套接层](https://baike.baidu.com/item/安全套接层/9442234)（SSL）的[硬件加速](https://baike.baidu.com/item/硬件加速/5702432)器。通常来说，这是一种独立的扩展卡插入到计算机的[PCI插槽](https://baike.baidu.com/item/PCI插槽)，其中包括一个或多个[辅助处理器](https://baike.baidu.com/item/辅助处理器)处理大量的SSL所需计算。SSL加速器可以使用现成的CPU，但大多数是使用[ASIC](https://baike.baidu.com/item/ASIC)和[RISC](https://baike.baidu.com/item/RISC)板卡来完成最艰难的计算工作。

## 用HTTP核心模块配置一个静态服务器

Nginx为配置一个完整的静态Web服务器提供了非常多的功能，下面会把这些配置项分为以下8类进行详述：虚拟主机与请求的分发、文件路径的定义、内存及磁盘资源的分配、网络连接的设置、MIME类型的设置、对客户端请求的限制、文件操作的优化、对客户端请求的特殊处理。

> 如果遇到配置完成，但是访问不了，也没有显示404、403等错误，可能是防火墙没有关闭，centos7使用的firewalld进程，关闭后重启nginx进程即可

### 文件路径定义

1. root方式设置资源路径

   >语法：root path;
   >
   >默认：root html;
   >
   >配置块：http、server、location、if
   >
   >例如，定义资源文件**相对于**HTTP请求的根目录。
   >
   >```nginx
   >location /download/ {
   >	root /opt/web/html/;
   >}
   >```
   >
   >在上面的配置中，如果有一个请求的URI是/download/index/test.html，那么Web服务器将会返回服务器上/opt/wen/html/download/index/test.html文件的内容。

2. 以alias方式设置资源路径（别名）

   > 语法：alias path;
   >
   > 配置块：location
   >
   > alias将会把请求的文件路径直接替换为path，而不会是`path/请求路径`，这是alias只能放置到location块中的原因。

3. 访问首页

   > 语法： index file...; 
   >
   > 默认： index index.html; 
   >
   > 配置块： http、server、location
   >
   > 例如：
   >
   > ```nginx
   > location / {
   > 	root path;
   > 	index /index.html /html/index.php /index.php;  // 从后向前
   > }
   > ```
   >
   > 接收到请求后，Nginx首先会尝试访问path/index.php文件，如果可 以访问，就直接返回文件内容结束请求，否则再试图返回 path/html/index.php文件的内容，依此类推。

4. 根据HTTP返回码重定向页面

   > 语法： error_page code[code...][=|=answer-code]uri|@named_location 
   >
   > 配置块： http、server、location、if
   >
   > 当对于某个请求返回错误码时，如果匹配上了error_page中设置的 code，则重定向到新的URI中。例如：
   >
   > ```nginx
   > error_page 404 /404.html;
   > error_page 502 503 504 /50x.html;
   > error_page 403 http://example.com/forbidden.html
   > ;
   > error_page 404 = @fetch
   > ```

5. 是否允许递归使用errot_page

   > 语法： recursive_error_pages[on|off];
   >
   >  默认： recursive_error_pages off; 
   >
   > 配置块： http、server、location 
   >
   > 确定是否允许递归地定义error_page。

6. try_files

   > 语法： try_files path1[path2]uri; 
   >
   > 配置块： server、location
   >
   > try_files后要跟若干路径，如path1 path2...，而且最后必须要有uri参 数，意义如下：尝试按照顺序访问每一个path，如果可以有效地读取， 就直接向用户返回这个path对应的文件结束请求，否则继续向下访问。 如果所有的path都找不到有效的文件，就重定向到最后的参数uri上。因此，最后这个参数uri必须存在，而且它应该是可以有效重定向的。如：
   >
   > ```nginx
   > try_files /system/maintenance.html $uri $uri/index.html $uri.html @other;
   > location @other {
   > 	proxy_pass http://backend;
   > }
   > ```
   >
   > 上面这段代码表示如果前面的路径，如/system/maintenance.html 等，都找不到，就会反向代理到http://backend 服务上。还可以用指定错 误码的方式与error_page配合使用，例如：
   >
   > ```nginx
   > location / {
   > 	try_files $uri $uri/ /error.phpc=404 =404;
   > }
   > ```
   >
   > 

7. 
