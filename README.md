ssltest
====

CentOS のdockerイメージを用いてcurlでHTTPSを叩いてみるテスト

### 使い方

```
ssltest  https://example.com
```

https://example.comに対して、curlでアクセスする

デフォルトでは、

CentOS 5.1..5.11, 6.1..6.8 で実行して試す

### 主な仕様

- 成功した場合、より新しいマイナーバージョンについては確認しない。
- 失敗した場合、より古いマイナーバージョンについては確認しない。
- 一番新しいマイナーバージョンでも失敗した場合、opensslの更新を行い実行する
- 使用しているdocker imageは [https://hub.docker.com/r/ywatase/centos/](https://hub.docker.com/r/ywatase/centos/) にあるものを使用する
	- docker imageにはcurl / yum-utils (repoquery コマンド) パッケージが必要
- `repoquery`の結果及びrpmは、スクリプトが存在するディレクトリに`cache`ディレクトリが作成され保存される
- curlの依存する暗号化ライブラリがCentOS6からnssのため、CentOS6以降はopensslではなくnssのバージョンを表示する

### Requirement

- docker
- bash

## オプション

```
usage: ssltest [-CcOpv456] URL
  v         : verbose mode
  p         : pull docker images
  c version : centos version (cf. 5.11, 6.7, ...)
  C         : use with c option. upgrade ca-certificates. it can't use with -O.
  O         : use with c option. upgrade openssl (or nss). it can't use with -C.
  4         : test centos4
  5         : test centos5
  6         : test centos6
  7         : test centos7
```

## 実行例


```
./ssltest -5 https://google.com/
centos:5.11 / openssl-0.9.8e-27.el5_10.4 / openssl-0.9.8e-27.el5_10.4 : OK
centos:5.2  / openssl-0.9.8b-10.el5 / openssl-0.9.8b-10.el5 : NG
centos:5.3  / openssl-0.9.8e-7.el5 / openssl-0.9.8e-7.el5 : NG
centos:5.4  / openssl-0.9.8e-12.el5 / openssl-0.9.8e-12.el5 : NG
centos:5.5  / openssl-0.9.8e-12.el5_4.6 / openssl-0.9.8e-12.el5_4.6 : NG
centos:5.6  / openssl-0.9.8e-12.el5_5.7 / openssl-0.9.8e-12.el5_5.7 : NG
centos:5.7  / openssl-0.9.8e-20.el5 / openssl-0.9.8e-20.el5 : OK
```

```
./ssltest -6 https://google.com/
centos:6.8  / nss-3.21.0-8.el6           / ca-certificates-2015.2.6-65.0.1.el6_7 : OK
centos:6.1  / nss-3.12.9-9.el6           / ca-certificates-2010.63-3.el6         : OK
```

```
./ssltest -56 https://www.yahoo.com/
centos:5.11 / openssl-0.9.8e-27.el5_10.4 / openssl-0.9.8e-27.el5_10.4 : OK
centos:5.2  / openssl-0.9.8b-10.el5 / openssl-0.9.8b-10.el5 : NG
centos:5.3  / openssl-0.9.8e-7.el5 / openssl-0.9.8e-7.el5 : NG
centos:5.4  / openssl-0.9.8e-12.el5 / openssl-0.9.8e-12.el5 : NG
centos:5.5  / openssl-0.9.8e-12.el5_4.6 / openssl-0.9.8e-12.el5_4.6 : NG
centos:5.6  / openssl-0.9.8e-12.el5_5.7 / openssl-0.9.8e-12.el5_5.7 : NG
centos:5.7  / openssl-0.9.8e-20.el5 / openssl-0.9.8e-20.el5 : NG
centos:5.8  / openssl-0.9.8e-22.el5 / openssl-0.9.8e-22.el5 : OK
centos:6.8  / nss-3.21.0-8.el6           / ca-certificates-2015.2.6-65.0.1.el6_7 : OK
centos:6.1  / nss-3.12.9-9.el6           / ca-certificates-2010.63-3.el6         : OK
```
