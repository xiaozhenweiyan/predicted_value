# Tasks

- [x] Task 1: 克隆 GitHub 仓库
  - [x] 克隆 `https://github.com/xiaozhenweiyan/pixel-tools.git` 到 `/workspace/pixel-tools/`
  - [x] 验证 `js/pixel-rpg.js`、`index.html`、`styles/pixel.css`、`js/i18n.js` 存在

- [x] Task 2: 重构 `state.player.equipment` 为 7 槽位
  - [x] 修改 `state.player` 初始化：`equipment: { leftHand: null, rightHand: null, head: null, body: null, legs: null, feet: null, accessory: null }`
  - [x] 修改 `reset()` 同步初始化 7 槽位

- [x] Task 3: 替换 `ITEM_TEMPLATES` 为 8 种新物品
  - [x] 删除旧 6 种物品模板
  - [x] 新增 8 种：木剑/恢复药水I/皮盔/皮甲/皮护腿/皮靴/经验宝石I/攻击戒指
  - [x] 每种物品定义 `id/name/type/slot/icon/atkbak/def/desc/effect/stackable`

- [x] Task 4: 实现像素画图标渲染函数 `drawItemIcon(ctx, item, x, y, size)`
  - [x] 根据 `item.id` 分支绘制不同像素画
  - [x] 木剑：棕色剑身 + 剑柄
  - [x] 恢复药水I：瓶子 + 红色液体
  - [x] 皮盔/皮甲/皮护腿/皮靴：皮革色对应形状
  - [x] 经验宝石I：蓝绿菱形宝石
  - [x] 攻击戒指：金色环形

- [x] Task 5: 更新 `renderInventory` —— 格子放大 + 数量角标
  - [x] 格子尺寸 ≥56px
  - [x] 数量显示在右下角，白色字体，透明背景
  - [x] 使用 canvas 绘制物品图标
  - [x] 选中状态高亮（金色边框）

- [x] Task 6: 更新 `renderEquipment` —— 7 槽位
  - [x] 渲染 7 个装备格子，每个标注槽位名称
  - [x] 使用 canvas 绘制装备图标
  - [x] 选中状态高亮

- [x] Task 7: 实现物品详情面板 `renderItemDetails`
  - [x] 在背包区域下方新增详情面板 DOM
  - [x] 显示物品名称/类型/描述/属性
  - [x] 消耗品显示"使用"按钮
  - [x] 可穿戴物品显示"穿戴"按钮
  - [x] 装备栏物品显示"取消佩戴"按钮
  - [x] 空选中时显示提示文字

- [x] Task 8: 实现点击交互逻辑
  - [x] 点击背包格子：选中（高亮）+ 显示详情
  - [x] 点击装备格子：选中（高亮）+ 显示详情
  - [x] 二次点击同一格子：取消选中
  - [x] 先点有物品格子再点另一格子：交换/移动物品
  - [x] 背包装备到装备槽：调用 equipItem
  - [x] 装备槽物品移到背包：调用 unequipItem
  - [x] 点击外部区域：取消选中（document click 监听）

- [x] Task 9: 更新 `equipItem`/`unequipItem`/`useItem`
  - [x] `equipItem(index)`：根据物品 `slot` 字段装备到对应槽位
  - [x] `unequipItem(slotKey)`：装备回背包
  - [x] `useItem(index)`：消耗品生效（药水恢复 HP/经验宝石加经验）
  - [x] 背包满时处理（换装时旧装备回背包失败则阻止）

- [x] Task 10: 更新 `getEffectiveAtk`/`getEffectiveDef`
  - [x] 遍历 7 个装备槽位累加 atk/def

- [x] Task 11: 更新 `openChest`
  - [x] 从新 8 种物品模板加权随机
  - [x] 经验宝石即时消耗
  - [x] 其余入背包

- [x] Task 12: 更新 `index.html` RPG DOM 结构
  - [x] 装备栏改为 7 槽位 DOM
  - [x] 背包区域下方新增详情面板容器 `rpg-item-details`
  - [x] 保持控制按钮在上、地图在下的布局

- [x] Task 13: 更新 `styles/pixel.css` RPG 样式
  - [x] `.rpg-inventory-slot` 尺寸 ≥56px
  - [x] `.rpg-inventory-slot .count` 右下角白色字体透明背景
  - [x] `.rpg-equipment-slot` 7 格布局
  - [x] `.rpg-item-details` 详情面板样式
  - [x] `.rpg-slot.selected` 选中高亮（金色边框）
  - [x] 移动端响应式适配

- [x] Task 14: 添加 i18n 中英文键
  - [x] 装备槽位名称：`rpg_slot_leftHand`/`rpg_slot_rightHand`/`rpg_slot_head`/`rpg_slot_body`/`rpg_slot_legs`/`rpg_slot_feet`/`rpg_slot_accessory`
  - [x] 按钮文案：`rpg_btn_use`/`rpg_btn_equip`/`rpg_btn_unequip`
  - [x] 物品名称与描述中英文
  - [x] 详情面板提示文字

- [x] Task 15: 更新 `service-worker.js` CACHE_VERSION 升级

- [x] Task 16: 更新 `README.md` 详细记录
  - [x] RPG 物品栏/装备栏系统说明
  - [x] 8 种物品列表与属性
  - [x] 7 槽位装备系统
  - [x] 点击交互说明（选中/交换/使用/穿戴/卸下）
  - [x] 更新日志条目

- [x] Task 17: 语法检查 + 提交推送
  - [x] `node -c js/pixel-rpg.js` 语法检查
  - [x] `node -c js/i18n.js` 语法检查
  - [x] git add + commit
  - [x] git push 到 GitHub

# Task Dependencies
- Task 1（克隆）是所有后续任务的前提
- Task 2-11 修改 `js/pixel-rpg.js`，建议由同一 sub-agent 顺序执行避免冲突
- Task 12-13 修改 `index.html` 和 `pixel.css`，可与 Task 2-11 并行（不同文件）
- Task 14 修改 `i18n.js`，可与 Task 2-11 并行
- Task 15 独立，可并行
- Task 16（README）依赖 Task 2-14 完成
- Task 17（提交推送）依赖所有任务完成
