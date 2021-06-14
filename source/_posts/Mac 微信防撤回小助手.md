---
title: Mac微信防撤回小助手教程
date: '2020-03-10 '
tag: 'Mac'
---

1 . 打开终端输入命令:
```
a . 退出微信（杀掉微信进程就好，无需退号）

b . 执行：sudo rm -r -f WeChatExtension-ForMac && git clone --depth=1 https://github.com/MustangYM/WeChatExtension-ForMac && cd WeChatExtension-ForMac/WeChatExtension/Rely && ./Install.sh

c . 回车, 输入密码，安装完毕。

d . 安装xcode，这里需要Mac运行环境，如果有直接下一步。

e . 执行签名：sudo codesign --sign - --force --deep /Applications/WeChat.app

```



开发者Github: [https://github.com/MustangYM/WeChatExtension-ForMac][1]


  [1]: https://github.com/MustangYM/WeChatExtension-ForMac