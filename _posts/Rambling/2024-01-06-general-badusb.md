---
layout: post
title: 使用badusb绕过粘贴限制
category: 技术
tags:
  - 其他
keywords: Other
---
obsidian都已经打开了，顺便把前两天的一个小操作写出来。

事情起因是遇到了一个不允许右键及复制粘贴操作的文本框，如果是Web的话一般这种时候就几个操作：停js/改css/装插件，但如果对方是个应用，又没调试环境，就麻爪了。我遇到的就是这个情况，还好没有啥复制需求，仅需要解决大段英文文本不想手打偷懒粘贴的问题。

果然偷懒是人类进步的原动力啊！

想了想，要么软件模拟输入法输入，要么模拟键盘输入。前者的话要上网去找软件，懒，想起来抽屉里有badusb，那干脆就用badusb实现后者好了。

打开Arduino，好久没碰过了打开之后还是以前捣鼓ESP32摄像头监控家里的项目，保存关掉，开发板切成Arduino Leonardo，硬盘里找到badusb的脚本库，把唤起记事本打印Hello World的改一下。

```
#include <Keyboard.h>

void setup() {
  Keyboard.begin();
  delay(3000); // 延时

  Keyboard.println("text");
  Keyboard.println("text");
  Keyboard.end(); // 结束键盘通讯
}

void loop() {
  // 无操作
}
```

验证，烧写，光标挪到待输入的文本框，懒得复制的大段文本哗啦哗啦的就输进去了，齐活。

可惜没办法写中文，所以也只是个应用面极窄的奇技淫巧而已。