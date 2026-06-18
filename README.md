# CS2 WeaponPaints - Drop Knife Skin Fix

基于 [Nereziel/cs2-WeaponPaints](https://github.com/Nereziel/cs2-WeaponPaints) v3.3a 修改版。

## 修复内容

修复了 **drop 刀后捡起皮肤丢失** 的问题。

### 原问题

当玩家 A 换了刀皮肤（如多普勒蝴蝶刀），玩家 B 换了不同的刀皮肤（如多普勒爪子刀），A 通过 drop knife 插件丢出蝴蝶刀给 B 捡起后：

- 刀皮肤消失，变成原皮
- 刀模型被切换成 B 自己的刀型
- 同刀型时 nametag 等也被覆盖

### 修改原理

原代码在 `OnItemPickup` → `GivePlayerWeaponSkin` 流程中，只以**当前持有者**的皮肤数据来覆盖武器外观。捡起别人的刀时，持有者没有该武器 defindex 的皮肤配置，导致皮肤被清空或切换失败。

本修改通过 **`OnItemPickup` defindex 比对** 和 **`_pickingUpKnife` 标记机制**，在捡刀路径中跳过 `GivePlayerWeaponSkin` 的皮肤覆盖，保留原主人的刀型和皮肤。

### 修改文件

| 文件 | 改动 |
|------|------|
| `Events.cs` | `OnItemPickup` 增加不同刀型 defindex 比对 + 捡刀 slot 标记 |
| `WeaponAction.cs` | `GivePlayerWeaponSkin` 两个刀分支增加捡刀标记检测 |
| `Variables.cs` | 新增 `_pickingUpKnife` HashSet |

## 编译

```bash
dotnet build
```

需要 .NET 8.0 SDK。

## 安装

将 `bin/Debug/net8.0/WeaponPaints.dll` 复制到 CS2 服务器的 CounterStrikeSharp 插件目录，替换原文件。

## 配合使用

需配合 drop knife 插件（如 [CS2DropKnife](https://github.com/HSKrz/CS2DropKnife)）使用。

## 版本

基于 WeaponPaints v3.3a，修改时间 2026-06-19。

## 原始项目

- [Nereziel/cs2-WeaponPaints](https://github.com/Nereziel/cs2-WeaponPaints)
- [LielXD/CS2-WeaponPaints-Website](https://github.com/LielXD/CS2-WeaponPaints-Website)
