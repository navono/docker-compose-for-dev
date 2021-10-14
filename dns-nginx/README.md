# 局域网域名

使用 `dnsmasq` 做域名解析；`nginx` 做反向代理。

## dnsmasq

主要配置在 `dnsmasq.conf` 文件。域名和 IP 映射有两种方式。

- 一种是直接写在配置文件中，如
  > address=/gitlab.ping.com/192.168.1.100

  这样，同时在 `Web` 界面也可修改
- 另外一种是用额外独立的文件，如
  > addn-hosts=/etc/dnsmasqhosts

  格式是这样：
  > 192.168.1.100 gitlab.ping.com react.ping.com

  缺点是无法在 `Web` 界面修改。

上述配置的域名对应的 `IP 地址` 即 `DNS` 服务的地址。在局域网中如果需要通过域名访问，那么需要在其相应的网卡中的 `DNS 服务器地址`中填入上述的 `IP 地址`。

__`注意`__：如果有开发机开启了全局代理，需要将其关闭。或者将域名加入其 `bypass` 列表。


## nginx

所有的配置文件在 `conf.d` 目录下，按需增加即可。
