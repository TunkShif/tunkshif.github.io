TITLE: Cross The Wall Through Shadowsocks-Heroku
TAG: linux
Cross The Wall Through Shadowsocks-Heroku
=========================================

> [原项目地址](https://github.com/mrluanma/shadowsocks-heroku)

首先需要在 heroku 上创建好 app, 然后按照上面的操作执行, 先把项目 clone 到 heroku app 内, 然后上传就行,
之后执行 `npm install` 后, 按照以下方式启动

```
node local.js -s still-tor-8707.herokuapp.com -l 1080 -m rc4 -k <key> -r 80
```

由于 chromium 在 linux 下没有 proxy setting, 需要手动通过命令指定代理配置, 按照以下方式启动即可

```
chromium --proxy-server="socks5://127.0.0.1:1080"
```

