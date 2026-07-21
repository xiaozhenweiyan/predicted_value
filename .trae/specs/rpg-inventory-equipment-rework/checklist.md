# Checklist

## 背包格子放大与数量徽标
- [ ] `.rpg-inventory-slot` CSS 尺寸 ≥ 48×48px
- [ ] 数量徽标定位在格子右下角（absolute bottom/right）
- [ ] 数量徽标 `color: #fff; background: transparent;` 无方块底色
- [ ] `renderInventory` 中数量 === 1 时不渲染徽标

## 装备栏 7 槽位
- [ ] `index.html` 装备区有 7 个槽位 DOM（左手/右手/头部/身体/腿/脚/饰品）
- [ ] `state.player.equipment` 含 7 个字段：leftHand / rightHand / head / body / legs / feet / accessory
- [ ] `reset()` 初始化 7 个字段为 null
- [ ] `renderEquipment` 遍历 7 槽位渲染
- [ ] `.rpg-equipment` CSS 改为 7 格布局

## 新物品体系（8 种）
- [ ] `ITEM_TEMPLATES` 含且仅含 8 种物品：wooden_sword / potion_i / leather_helmet / leather_armor / leather_leggings / leather_boots / gem_i / atk_ring
- [ ] 每种物品含 id/name/type/slot/icon/desc 字段
- [ ] 武器（wooden_sword）slot = ['leftHand', 'rightHand']，atk = 2
- [ ] 皮盔 slot = head，def = 1；皮甲 slot = body，def = 2；皮护腿 slot = legs，def = 1；皮靴 slot = feet，def = 1
- [ ] potion_i stackable=true，effect 恢复 30 HP
- [ ] gem_i stackable=true，effect +1 经验
- [ ] atk_ring slot = accessory，atk = 1
- [ ] `equipItem` 校验 item.slot 包含目标 slot，类型不匹配拒绝
- [ ] `unequipItem` 卸下装备回背包，背包满则提示
- [ ] `useItem` 处理 potion_i 与 gem_i
- [ ] `getEffectiveAtk` 遍历 7 槽位累加 atk
- [ ] `getEffectiveDef` 遍历 7 槽位累加 def
- [ ] `openChest` 从新 8 种物品加权随机掉落

## 物品属性区与点击交互
- [ ] `index.html` 背包下方有 `.rpg-item-detail` DOM
- [ ] `.rpg-item-detail` 像素风样式（深空蓝 + 金边 + 硬阴影）
- [ ] `state.ui.selected` 跟踪 { kind, index|slot }
- [ ] `renderItemDetail()` 根据选中状态渲染属性区
- [ ] 选中背包消耗品 → 显示"使用"按钮
- [ ] 选中背包可穿戴物品 → 显示"穿戴"按钮
- [ ] 选中装备栏物品 → 显示"取消佩戴"按钮
- [ ] 背包格点击：先点 A（有物品）再点 B → 交换/移动
- [ ] 背包格点击同一格 → 取消选中
- [ ] 全局 click listener：点击背包/装备栏/属性区以外 → 取消选中
- [ ] "使用"按钮 → 调用 useItem，消耗 1 个并应用效果
- [ ] "穿戴"按钮 → 调用 equipItem，物品进入对应槽位
- [ ] "取消佩戴"按钮 → 调用 unequipItem，物品回背包

## drawUI 装备加成显示
- [ ] ATK 显示为 `ATK base+bonus`（bonus=0 时只显示 base）
- [ ] DEF 显示同理
- [ ] combatRound 使用 getEffectiveAtk()/getEffectiveDef()

## i18n 中英文键
- [ ] 7 个装备槽名称中英文键存在
- [ ] "使用"/"穿戴"/"取消佩戴"按钮文案中英文键存在
- [ ] 8 种物品名+描述中英文键存在

## README 与 SW
- [ ] README.md 更新日志新增本次重构条目
- [ ] service-worker.js CACHE_VERSION 升级（v22 → v23）

## 语法与最终验证
- [ ] `node -c js/pixel-rpg.js` 通过
- [ ] `node -c js/i18n.js` 通过
- [ ] grep 验证 ITEM_TEMPLATES 含 8 种新物品 id
- [ ] grep 验证 equipment 含 7 槽位字段
- [ ] grep 验证 renderItemDetail 已实现
- [ ] grep 验证 index.html 含 .rpg-item-detail DOM
- [ ] grep 验证 CACHE_VERSION=v23
