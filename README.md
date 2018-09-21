# shadowsocks-proxychains4-openvpn

文章最早发在知乎上，被和谐了。在这里做个备份

## Shadowsocks

#### 安装

为了兼容 OpenSSL 1.1, 使用 3.0.0 版本

```bash
$ pip install -U git+https://github.com/shadowsocks/shadowsocks.git@master
```

pypi 里是 2.8.2，与OpenSSL 1.1 版本不兼容。
执行报错: [run sslocal raise error · Issue #646 · shadowsocks/shadowsocks](https://github.com/shadowsocks/shadowsocks/issues/646#issuecomment-267977330)

#### 配置文件

创建配置文件 `/etc/shadowsocks/config.json`

```bash
{
    "server":"139.162.80.666",
    "server_port":1081,
    "local_port":1082,
    "password":"xxxx",
    "timeout":600,
    "method":"aes-256-cfb"
}
```

#### Server 端启动

```bash
ssserver -c /etc/shadowsocks/config.json -d start 后台启动
ssserver -c /etc/shadowsocks/config.json -d stop 后台停止
```

新版本，可能要注意 server 的 IP 配置


#### Client 端启动

```bash
sslocal -c /etc/shadowsocks/config.json
```


## proxychains

最早安装的时候，apt 装的版本有问题。
编译安装 proxychains-ng，很不错。
注意，编译安装的，是 proxychains4, 最后多了个 “4”

现在依旧采用编译安装的方法


```bash
$ git clone https://github.com/rofl0r/proxychains-ng.git
$ cd proxychains-ng/
$ ./configure --prefix=/usr --sysconfdir=/etc
$ make
$ sudo make install
$ sudo make install-config
```

`/etc/proxychains.conf` 的最后一行 `socks4 127.0.0.1 9095` 改为

```bash
socks5 127.0.0.1 1082
```

最后的 1082 是 shadowsocks 中 local 的端口。

#### 使用方法

```bash
$ proxychains4 wget google.com
```
