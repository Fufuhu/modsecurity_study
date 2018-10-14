
# Nginxへのインストール

Nginxには動的なモジュール読み込みの仕組みがない。
ModSecurityはメインサーバのソースコードを使ってコンパイルする必要がある。

## NginxへのModSecurityモジュールの導入

- 事前準備
- インストール
- 設定

### 事前準備

```bash
$ apt-get install apache2-threaded-dev libxml2-dev
```

#### libModSecurityの構築

```bash
$ sudo apt-get install g++ flex bison curl doxygen libyajl-dev libgeoip-dev libtool dh-autoreconf libcurl4-gnutls-dev libxml2 libpcre++-dev libxml2-dev
$ cd /opt/
$ git clone https://github.com/SpiderLabs/ModSecurity
$ cd ModSecurity/
$ git checkout -b v3/master origin/v3/master
$ sh build.sh
$ git submodule init
$ git submodule update #[for bindings/python, others/libinjection, test/test-cases/secrules-language-tests]
$ ./configure
$ make
$ make install
```

#### nginx-connectorの構築

```bash
cd /opt/
git clone https://github.com/SpiderLabs/ModSecurity-nginx
wget http://nginx.org/download/nginx-1.9.2.tar.gz
tar -xvzf nginx-1.9.2.tar.gz
cd /opt/nginx-1.9.2
./configure --add-module=/opt/ModSecurity-nginx 
make
make install
```

from https://github.com/SpiderLabs/ModSecurity/wiki/Compilation-recipes-for-v3.x#ubuntu-1504

### インストール

スタンドアローンモジュールのコンパイルを行う。


```bash
$ ./configure --enable-standalone-module --disable-mlogc
$ make
```

スタンドアローンライブラリのビルドが成功したら、
nginxサーバのビルドを行う。

```bash
$ ./configure --add-module=../mod_security/nginx/modsecurity
$ make
$ sudo make install
```

### 設定


# 参考

https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual-%28v2.x%29#Installation_for_NGINX