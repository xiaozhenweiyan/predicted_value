# Checklist

## 物品栏格子
- [x] 格子尺寸 ≥56px（CSS 验证）
- [x] 物品数量显示在右下角
- [x] 数量字体为白色
- [x] 数量背景为透明（无背景框）
- [x] 单个物品（count==1）不显示数量

## 装备栏 7 槽位
- [x] `state.player.equipment` 包含 7 个字段：leftHand/rightHand/head/body/legs/feet/accessory
- [x] `reset()` 同步初始化 7 槽位
- [x] 装备栏 DOM 渲染 7 个格子
- [x] 每个格子标注槽位名称（中英文）

## 8 种新物品
- [x] 删除旧 6 种物品模板
- [x] 新增木剑（weapon, leftHand/rightHand, atk+2）
- [x] 新增恢复药水I（consumable, 恢复20HP, stackable）
- [x] 新增皮盔（armor, head, def+1）
- [x] 新增皮甲（armor, body, def+3）
- [x] 新增皮护腿（armor, legs, def+2）
- [x] 新增皮靴（armor, feet, def+1）
- [x] 新增经验宝石I（consumable, +1经验, stackable）
- [x] 新增攻击戒指（accessory, atk+1）

## 像素画图标
- [x] 实现 `drawItemIcon(ctx, item, x, y, size)` 函数
- [x] 木剑图标：棕色剑身 + 剑柄
- [x] 恢复药水I图标：瓶子 + 红色液体
- [x] 皮盔图标：皮革色帽子
- [x] 皮甲图标：皮革色胸甲
- [x] 皮护腿图标：皮革色护腿
- [x] 皮靴图标：皮革色靴子
- [x] 经验宝石I图标：蓝绿菱形宝石
- [x] 攻击戒指图标：金色环形
- [x] 不使用 emoji/文字作为图标

## 物品详情面板
- [x] 背包区域下方存在详情面板 DOM
- [x] 点击消耗品显示名称/类型/描述/属性 + "使用"按钮
- [x] 点击可穿戴物品显示属性 + "穿戴"按钮
- [x] 点击装备栏物品显示属性 + "取消佩戴"按钮
- [x] 点击空格子显示提示文字
- [x] 无选中时面板清空

## 点击交互
- [x] 点击背包格子高亮选中 + 显示详情
- [x] 点击装备格子高亮选中 + 显示详情
- [x] 二次点击同一格子取消选中
- [x] 先点有物品格子再点另一格子执行交换/移动
- [x] 背包装备到装备槽调用 equipItem
- [x] 装备槽物品移到背包调用 unequipItem
- [x] 点击背包和装备栏以外区域取消选中
- [x] 选中状态有视觉高亮（金色边框）

## 装备逻辑
- [x] `equipItem(index)` 根据物品 slot 字段装备到对应槽位
- [x] `unequipItem(slotKey)` 装备回背包
- [x] `useItem(index)` 消耗品生效（药水恢复HP/经验宝石加经验）
- [x] 背包满时换装阻止或提示
- [x] `getEffectiveAtk()` 累加 7 槽位 atk 加成
- [x] `getEffectiveDef()` 累加 7 槽位 def 加成
- [x] `combatRound` 使用 getEffectiveAtk/Def

## 宝箱掉落
- [x] `openChest` 从新 8 种物品加权随机
- [x] 经验宝石即时消耗加经验
- [x] 其余物品入背包
- [x] 背包满时提示

## UI 显示
- [x] `drawUI` 显示 `ATK 基础+加成` 格式
- [x] `drawUI` 显示 `DEF 基础+加成` 格式
- [x] `drawUI` 右上角显示 `背包 N/16`

## 样式
- [x] `.rpg-inventory-slot` 尺寸 ≥56px
- [x] `.rpg-inventory-slot .count` 右下角白色透明背景
- [x] `.rpg-equipment-slot` 7 格布局样式
- [x] `.rpg-item-details` 详情面板样式
- [x] `.rpg-slot.selected` 选中高亮样式
- [x] 移动端响应式（≤900px 单列）

## i18n
- [x] 7 个装备槽位名称中英文键
- [x] "使用"/"穿戴"/"取消佩戴"按钮中英文键
- [x] 物品名称与描述中英文
- [x] 详情面板提示文字中英文

## 部署
- [x] `service-worker.js` CACHE_VERSION 升级
- [x] README.md 详细更新（物品/装备/交互说明 + 更新日志）
- [x] `node -c js/pixel-rpg.js` 语法检查通过
- [x] `node -c js/i18n.js` 语法检查通过
- [x] git commit + push 成功
