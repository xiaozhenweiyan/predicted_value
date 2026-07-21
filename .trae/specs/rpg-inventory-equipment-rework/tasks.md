# Tasks

- [ ] Task 1: 背包格子放大 + 数量徽标样式
  - [ ] SubTask 1.1: `styles/pixel.css` 中 `.rpg-inventory-slot` 尺寸从 ~32px 改为 ≥48×48px，物品图标相应放大
  - [ ] SubTask 1.2: 数量徽标定位到格子右下角，`color: #fff; background: transparent;` 无方块底色；数量===1 时不渲染徽标
  - [ ] SubTask 1.3: `js/pixel-rpg.js` 的 `renderInventory` 同步更新徽标生成逻辑（数量 ≤1 不渲染）

- [ ] Task 2: 装备栏重置为 7 槽位
  - [ ] SubTask 2.1: `index.html` RPG 侧栏装备区改为 7 个槽位 DOM（左手/右手/头部/身体/腿/脚/饰品），每个槽位含图标 + 空槽提示文字 + data-slot 属性
  - [ ] SubTask 2.2: `js/pixel-rpg.js` 中 `state.player.equipment` 重写为 `{ leftHand, rightHand, head, body, legs, feet, accessory }`；`reset()` 同步初始化
  - [ ] SubTask 2.3: `renderEquipment` 重写，遍历 7 槽位渲染图标或空槽提示
  - [ ] SubTask 2.4: `styles/pixel.css` 中 `.rpg-equipment` 改为 7 格布局（可用 grid 2 列或 flex wrap），每格 ≥40×40px

- [ ] Task 3: 新物品体系（8 种物品）
  - [ ] SubTask 3.1: 重写 `ITEM_TEMPLATES` 常量，删除旧 6 种，新增 8 种：wooden_sword / potion_i / leather_helmet / leather_armor / leather_leggings / leather_boots / gem_i / atk_ring；每项含 id/name/type/slot/icon(像素绘制函数或字符)/desc/atk/def/effect/stackable
  - [ ] SubTask 3.2: 像素图标用 canvas 绘制函数或 CSS 字符表达：木剑=木质剑形、药水=瓶+红液、皮盔/甲/腿/靴=皮革色块对应部位、经验宝石=绿色菱形、攻击戒指=金色环
  - [ ] SubTask 3.3: `equipItem(slot, item)` 校验 item.slot === slot（左手/右手允许 weapon 类型），类型不匹配提示
  - [ ] SubTask 3.4: `unequipItem(slot)` 卸下装备回背包（背包满则提示并阻止）
  - [ ] SubTask 3.5: `useItem(index)` 处理 potion_i（恢复 30 HP）和 gem_i（+1 经验，触发升级检查）
  - [ ] SubTask 3.6: `getEffectiveAtk()` / `getEffectiveDef()` 遍历 7 槽位累加 atk/def
  - [ ] SubTask 3.7: `openChest` 改为从新 8 种物品加权随机掉落

- [ ] Task 4: 物品属性区与完整点击交互
  - [ ] SubTask 4.1: `index.html` 背包网格下方新增 `.rpg-item-detail` DOM（含名称/类型/描述行 + 按钮容器）
  - [ ] SubTask 4.2: `styles/pixel.css` 新增 `.rpg-item-detail` 像素风样式（深空蓝面板 + 金色边框 + 硬阴影）
  - [ ] SubTask 4.3: `js/pixel-rpg.js` 新增 `state.ui.selected = { kind: 'inventory'|'equipment', index|slot }` 跟踪选中状态
  - [ ] SubTask 4.4: 新增 `renderItemDetail()` 根据选中状态渲染属性区：消耗品显示"使用"按钮、可穿戴显示"穿戴"按钮、装备栏选中显示"取消佩戴"按钮
  - [ ] SubTask 4.5: 背包格点击 handler：第一次点击有物品 → 选中；第二次点击另一格 → 交换（同类型可堆叠则合并到 max stack，否则占位交换）；点击同一格 → 取消选中
  - [ ] SubTask 4.6: 装备槽点击 handler：选中装备 → 属性区显示"取消佩戴"按钮
  - [ ] SubTask 4.7: 全局 document click listener：点击背包/装备栏/属性区以外的任何区域 → 取消选中，清空属性区
  - [ ] SubTask 4.8: "使用"/"穿戴"/"取消佩戴"按钮点击 handler 分别调用 useItem/equipItem/unequipItem，操作后刷新背包/装备/属性区

- [ ] Task 5: i18n 中英文键
  - [ ] SubTask 5.1: `js/i18n.js` 新增装备槽名称：rpg_slot_leftHand / rpg_slot_rightHand / rpg_slot_head / rpg_slot_body / rpg_slot_legs / rpg_slot_feet / rpg_slot_accessory（中英文）
  - [ ] SubTask 5.2: 新增按钮文案：rpg_btn_use（使用/Use）、rpg_btn_equip（穿戴/Equip）、rpg_btn_unequip（取消佩戴/Unequip）
  - [ ] SubTask 5.3: 新增 8 种物品名+描述中英文键（rpg_item_wooden_sword 等）

- [ ] Task 6: drawUI 装备加成显示与 combatRound 适配
  - [ ] SubTask 6.1: `drawUI` 中 ATK 显示改为 `ATK base+bonus`（bonus=0 时只显示 base）
  - [ ] SubTask 6.2: DEF 显示同理
  - [ ] SubTask 6.3: `combatRound` 使用 `getEffectiveAtk()`/`getEffectiveDef()`（已有，验证未回归）

- [ ] Task 7: README.md + service-worker.js
  - [ ] SubTask 7.1: `README.md` 更新日志新增"2026-07 · RPG 物品栏/装备栏交互重构"条目，5 项改动详细说明
  - [ ] SubTask 7.2: `service-worker.js` 升级 `CACHE_VERSION`（v22 → v23）

- [ ] Task 8: 语法检查与验证
  - [ ] SubTask 8.1: `node -c js/pixel-rpg.js` 语法检查通过
  - [ ] SubTask 8.2: `node -c js/i18n.js` 语法检查通过
  - [ ] SubTask 8.3: grep 验证：ITEM_TEMPLATES 含 8 种新物品 id、equipment 含 7 槽位字段、renderItemDetail 已实现、属性区 DOM 已在 index.html、CACHE_VERSION=v23

# Task Dependencies
- Task 2 依赖 Task 1（布局先行）
- Task 3 依赖 Task 2（物品 slot 字段需对应装备槽）
- Task 4 依赖 Task 2 + Task 3（交互依赖新结构与物品）
- Task 5 可与 Task 2/3 并行
- Task 6 依赖 Task 3（getEffectiveAtk/Def 遍历新槽位）
- Task 7 依赖所有前序任务完成
- Task 8 是最终验证，依赖所有任务完成
