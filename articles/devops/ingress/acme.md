<!-- title: -->
<!-- created: 2020-04-08 12:00:00 -->
<!-- updated:  -->
<!-- categories:   -->
<!-- tags:  https, ssl, acme, let's encrypt -->

# HTTPS 之 ACME

![](acme-https.jpg)

> acme.sh 是一个 ACME(自动化证书管理环境) 脚本，可以从 letsencrypt 生成免费的证书

<!-- more -->

官方文档 : [https://wiki.acme.sh](https://wiki.acme.sh)

## 安装

```shell
curl https://get.acme.sh | sh
~/.acme.sh/acme.sh --upgrade --auto-upgrade
```

## 配置 DNS API

```shell
# 使用 DnsPod API
export DP_Id=""
export DP_Key=""
# 使用 CloudXNS API
# export CX_Key=""
# export CX_Secret=""
```

## 申请证书

```shell
nohup ~/.acme.sh/acme.sh \
        --issue --force \
        --dns dns_dp \
        -d gankcode.com \
        -d *.gankcode.com \
        -d example.com\
        -d *.example.com \
        &

# --dns dns_dp 指定 使用 dnspod api
# -d 可以添加多个域名

tail -f nohup.out

# 成功后证书生成到 ~/.acme.sh/${第一个域名名称}/...
```

## 配置 Nginx

```txt
ssl_certificate      /root/.acme.sh/gankcode.com/fullchain.cer;
ssl_certificate_key  /root/.acme.sh/gankcode.com/kluster.cn.key;
```

## 限制

> 每次申请的let's encrypt证书有效期为 3 个月, 也就是说每三个月要申请一次并更新到服务器配置

## 设置ACME自动更新

```shell
acme.sh --upgrade --auto-upgrade
```

## 检查SSL证书级别

> [https://www.ssllabs.com/ssltest/index.html](https://www.ssllabs.com/ssltest/index.html)