---
layout: post
title: 自己动手丰衣足食
category: 资源
tags:
  - Android
  - Game
keywords: Game
---
一直在玩一个小游戏叫像素地牢[00-Evan/shattered-pixel-dungeon](https://github.com/00-Evan/shattered-pixel-dungeon)，Roguelike的，是个开源游戏，奈何太坐牢，通关很少。

因为是个开源游戏，所以有众多修改版，但去网上找也找不到我想要的轻度修改版本。干脆去fork了源码，自己改。Android开发是一窍不通，跟ChatGPT聊完知道应该改哪部分文件，把升级卷轴和力量药剂的生成数量加了一些
```java

public static boolean souNeeded() {
    int souLeftThisSet;
    // 3 SOU（升级卷轴）每个地牢区域
    souLeftThisSet = 3 - (LimitedDrops.UPGRADE_SCROLLS.count - (depth / 5) * 3);
    if (souLeftThisSet <= 0) return false;

    int floorThisSet = (depth % 5);
    // 掉落概率 = 剩余层数 / 卷轴剩余数量
    return Random.Int(5 - floorThisSet) < souLeftThisSet;
}

public static boolean posNeeded() {
    // 计算当前这组楼层（每 5 层为一组）还应该掉落的药剂数量
    int posLeftThisSet = 8 - (LimitedDropsSTREN.GTH_POTIONS.count - (depth / 5) * 4);
    if (posLeftThisSet <= 0) return false;  // 如果已经掉落足够多，就不再掉落

    int floorThisSet = (depth % 5);

    // 控制掉落的层数：药剂大约每 2 层掉一次
    int targetPOSLeft = 4 - floorThisSet / 2;
    if (floorThisSet % 2 == 1 && Random.Int(2) == 0) targetPOSLeft--;  // 有 50% 概率减少掉落

    return targetPOSLeft < posLeftThisSet;
}

```
怕增加的掉落数量占据物品池，又把整个物品池加了一点。
```java
protected void createItems() {
    // 默认情况下，60% 概率生成 3 件物品，30% 概率生成 4 件，10% 概率生成 5 件
    int nItems = 3 + Random.chances(new float[]{6, 3, 1});

    if (feeling == Feeling.LARGE){
        nItems += 2;
    }
```
改好了在GitHub上用Actions打包，拿来自己玩。

文件暂存[诺诺网盘](https://pan.15926.tech/d/%F0%9F%94%91Onedrive/%F0%9F%8E%AEGame/shattered-pixel-dungeon_1.1.apk)