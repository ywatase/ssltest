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
- 一番新しいマイナーバージョンでも失敗した場合、opensslの更新を行い実行する
- 使用しているdocker imageは [https://hub.docker.com/r/ywatase/centos/](https://hub.docker.com/r/ywatase/centos/) にあるものを使用する
	- docker imageにはcurl / yum-utils (repoquery コマンド) パッケージが必要
- `repoquery`の結果及びrpmは、スクリプトが存在するディレクトリに`cache`ディレクトリが作成され保存される

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
  O         : use with c option. upgrade openssl. it can't use with -C.
  4         : test centos4
  5         : test centos5
  6         : test centos6
```
