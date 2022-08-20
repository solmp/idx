
> Linux 命令(使用 CentOS 为例)

## mini版命令
- 清屏：ctrl + l
- 历史命令：history
- 显示文件列表：ls 
    - l: 显示详细文件列表
- 创建文件夹：mkdir
- 删除文件/文件夹：`rm -rf`
    - r: 删除文件夹
    - f: 强制删除，不提示
- 下载文件：curl
    - `curl -o <保存文件名> <文件链接>`

### yum
使用: `yum install -y xxx xxx`
- y: answer yes for all questions

#### 报错
1. `Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist`
- Centos8将于2021年年底停止服务
- 注释掉mirrorlist并更新镜像源，使用vault.centos.org代替mirror.centos.org
~~~bash
sed -i "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*
sed -i "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*
~~~

### 解压缩tar文件：tar
解压文件：`tar -zxvf xxx.tar.gz -C xxx`
- z: filter the archive through gzip
- x: extract files from an archive
- v: verbosely list files processed 
- f: 指定要解压的 tar 包的包名
- C(大写): 指定存放解压后文件的文件夹名，需要是已存在的文件夹

压缩文件：`tar -zcvf <压缩后文件名>.tar.gz <待压缩文件>`
- 例：`tar -zcvf xxx.tar.gz xxx xxx`


## 常用基础命令
### 下载工具：wget
安装：`yum install -y wget`
使用：`wget -O xxx.tar.gz <下载链接>`、`wget -P /tmp/ <下载链接>`
- O: 指定保存名
- P: 指定保存路径，如果已指定保存名则不生效

### netstat：查看当前占用端口
安装：`yum install -y net-tools`
查看当前占用端口：`netstat -ntlp`
- n: don't resolve names —— 直接显示端口号
- l: display listening server sockets
- a: display all sockets (default: connected)
- p: display PID/Program name for sockets
- t: tcp
- u: udp

### unzip/zip：解压缩zip文件
安装：`yum install - y unzip zip`
解压文件：`unzip xxx.zip -d <文件夹名>`
压缩文件：`zip -r <压缩包名> <待压缩文件>`
- r: recurse into directories
- 例：`zip -r xxx.zip a.xx b.xx xxx/`



# git
- 官网：https://git-scm.com/download/linux

Ubuntu: `apt-get install git`
CenteOS: `yum install git`

验证安装（显示版本号表示安装成功）：`git --version`


# nginx
查看版本：https://nginx.org/en/download.html

## 下载 nginx
~~~bash
## 法1：使用wget
# yum install -y wget
# wget http://nginx.org/download/nginx-1.21.6.tar.gz

## 法2：使用curl
curl -o nginx-1.21.6.tar.gz http://nginx.org/download/nginx-1.21.6.tar.gz
~~~

## 安装
~~~bash
# 解压
tar -zxvf nginx-1.21.6.tar.gz
cd nginx-1.21.6

# 配置
## 安装依赖
yum install -y gcc make pcre pcre-devel openssl openssl-devel
./configure --with-http_ssl_module --with-http_v2_module --with-stream

#### 输出配置信息 ####
# Configuration summary
#   + using system PCRE2 library
#   + using system OpenSSL library
#   + using system zlib library

#   nginx path prefix: "/usr/local/nginx"
#   nginx binary file: "/usr/local/nginx/sbin/nginx"
#   nginx modules path: "/usr/local/nginx/modules"
#   nginx configuration prefix: "/usr/local/nginx/conf"
#   nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
#   nginx pid file: "/usr/local/nginx/logs/nginx.pid"
#   nginx error log file: "/usr/local/nginx/logs/error.log"
#   nginx http access log file: "/usr/local/nginx/logs/access.log"
#   nginx http client request body temporary files: "client_body_temp"
#   nginx http proxy temporary files: "proxy_temp"
#   nginx http fastcgi temporary files: "fastcgi_temp"
#   nginx http uwsgi temporary files: "uwsgi_temp"
#   nginx http scgi temporary files: "scgi_temp"
######################

# 编译并安装
make && make install
~~~

## 配置环境变量
echo "export PATH=$PATH:/usr/local/nginx/sbin" >> /etc/bashrc
source /etc/bashrc

验证安装：nginx -v

## 修改nginx配置
参考：https://www.runoob.com/w3cnote/nginx-setup-intro.html

~~~bash
# 备份配置文件
cp /usr/local/nginx/conf/nginx.conf /usr/local/nginx/conf/nginx.conf.default
~~~

~~~conf
#### 默认配置 ####

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
~~~

