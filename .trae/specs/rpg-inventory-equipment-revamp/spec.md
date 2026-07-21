# RPG 物品栏/装备栏系统重做 Spec

## Why
当前 RPG 的物品栏格子太小、物品数量显示不清晰；装备栏只有 weapon/armor 两个槽位，无法支持丰富的装备类型；物品模板需要重新设计以匹配新装备槽位结构；点击交互缺乏直观的物品详情展示和交换功能。

## What Changes
- **物品栏格子放大**（≥56px），物品数量显示在右下角（白色字体、透明背景）
- **装备栏重构为 7 个槽位**：左手、右手、头部、身体、腿、脚、饰品
- **删除所有现有物品模板**，新增 8 种物品（木剑、恢复药水I、皮盔、皮甲、皮护腿、皮靴、经验宝石I、攻击戒指）
- **物品图标改为像素画绘制**（canvas 渲染），不再使用 emoji/文字
- **点击交互系统**：点击物品显示属性面板 + 使用/穿戴按钮；点击两个格子交换物品；点击外部取消选中；点击装备显示卸下按钮
- **更新 README.md** 详细记录新系统
- **BREAKING**: `state.player.equipment` 结构从 `{ weapon, armor }` 改为 `{ leftHand, rightHand, head, body, legs, feet, accessory }`

## Impact
- Affected code:
  - `js/pixel-rpg.js`：`ITEM_TEMPLATES` 重写、`state.player.equipment` 结构变更、`equipItem`/`unequipItem`/`useItem`/`getEffectiveAtk`/`getEffectiveDef`/`openChest`/`renderInventory`/`renderEquipment`/`drawUI` 修改、新增点击交互逻辑与详情面板、新增像素画图标渲染函数
  - `index.html`：RPG 侧栏 DOM 结构（装备 7 槽位、背包网格、详情面板容器）
  - `styles/pixel.css`：格子放大样式、数量角标样式、7 槽位装备栏布局、详情面板样式、选中高亮样式
  - `js/i18n.js`：新增装备槽位名称、按钮文案中英文键
  - `README.md`：详细更新日志
  - `service-worker.js`：`CACHE_VERSION` 升级

## ADDED Requirements

### Requirement: 物品栏格子放大与数量显示
The system SHALL display inventory slots at a larger size (≥56px square) with item count shown in the bottom-right corner using white font on transparent background.

#### Scenario: 物品堆叠显示
- **WHEN** 背包中存在堆叠物品（count > 1）
- **THEN** 格子右下角显示数量数字，白色字体，无背景框，字体大小约 12-14px

#### Scenario: 单个物品
- **WHEN** 背包中物品 count == 1
- **THEN** 不显示数量角标

### Requirement: 7 槽位装备栏
The system SHALL provide 7 equipment slots with the following keys and labels:
- `leftHand`（左手）
- `rightHand`（右手）
- `head`（头部）
- `body`（身体）
- `legs`（腿）
- `feet`（脚）
- `accessory`（饰品）

#### Scenario: 装备穿戴到对应槽位
- **WHEN** 用户点击"穿戴"按钮
- **THEN** 物品移动到其 `slot` 字段对应的装备槽位
- **AND** 若该槽位已有装备，旧装备回到背包
- **AND** 若背包满且需要换装，提示"背包已满"

#### Scenario: 武器双手 wielding
- **WHEN** 物品 slot 为 `leftHand` 或 `rightHand`
- **THEN** 可装备到对应手部槽位，另一只手可装备其他武器或盾牌

### Requirement: 8 种新物品模板
The system SHALL define exactly 8 item templates:

| # | id | 名称 | 类型 | slot | 属性 |
|---|-----|------|------|------|------|
| 1 | `wooden_sword` | 木剑 | weapon | leftHand/rightHand | atk +2 |
| 2 | `potion_hp_i` | 恢复药水I | consumable | - | 恢复 20 HP，stackable |
| 3 | `leather_helmet` | 皮盔 | armor | head | def +1 |
| 4 | `leather_armor` | 皮甲 | armor | body | def +3 |
| 5 | `leather_leggings` | 皮护腿 | armor | legs | def +2 |
| 6 | `leather_boots` | 皮靴 | armor | feet | def +1 |
| 7 | `exp_gem_i` | 经验宝石I | consumable | - | +1 经验，stackable |
| 8 | `attack_ring` | 攻击戒指 | accessory | accessory | atk +1 |

#### Scenario: 宝箱掉落
- **WHEN** 玩家打开宝箱
- **THEN** 从 8 种模板中加权随机选一个
- **AND** 经验宝石即时消耗（直接加经验），其余入背包
- **AND** 背包满时提示

### Requirement: 像素画物品图标
The system SHALL render item icons as pixel art on mini canvases (not emoji or text characters).

#### Scenario: 图标渲染
- **WHEN** 背包/装备格子包含物品
- **THEN** 在格子内绘制像素画图标，每种物品有独特视觉：
  - 木剑：棕色剑身 + 棕色剑柄
  - 恢复药水I：瓶子轮廓 + 红色液体
  - 皮盔：皮革色帽子形状
  - 皮甲：皮革色胸部护甲形状
  - 皮护腿：皮革色腿部护甲形状
  - 皮靴：皮革色靴子形状
  - 经验宝石I：蓝绿色菱形宝石
  - 攻击戒指：金色环形

### Requirement: 物品详情面板
The system SHALL display an item details panel below the inventory area.

#### Scenario: 点击消耗品
- **WHEN** 用户点击背包中的消耗品
- **THEN** 详情面板显示：物品名称、类型、描述、属性效果
- **AND** 面板内出现"使用"按钮

#### Scenario: 点击可穿戴物品
- **WHEN** 用户点击背包中的防具/武器/饰品
- **THEN** 详情面板显示：物品名称、类型、描述、属性加成、可装备槽位
- **AND** 面板内出现"穿戴"按钮

#### Scenario: 点击装备栏物品
- **WHEN** 用户点击装备栏中的已装备物品
- **THEN** 详情面板显示物品属性
- **AND** 面板内出现"取消佩戴"按钮

#### Scenario: 点击空格子
- **WHEN** 用户点击空背包格或空装备槽
- **THEN** 详情面板清空或显示提示文字"点击物品查看详情"

### Requirement: 交换/移动物品
The system SHALL support item swap between slots via click-to-select-then-click-target.

#### Scenario: 交换物品
- **WHEN** 用户先点击有物品的格子（高亮选中），再点击另一个格子（无论有无物品）
- **THEN** 两个格子的物品交换
- **AND** 如果目标格子原本为空，则物品移动到目标格子

#### Scenario: 装备与背包交换
- **WHEN** 用户点击背包中的可穿戴物品，再点击对应装备槽位
- **THEN** 背包装备到对应槽位，原装备（若有）回到背包该格子

#### Scenario: 取消选中
- **WHEN** 用户已选中一个格子，再点击背包和装备栏以外的任何区域
- **THEN** 取消选中，清除高亮

#### Scenario: 同一格子二次点击
- **WHEN** 用户点击已选中的格子
- **THEN** 取消选中

## MODIFIED Requirements

### Requirement: 装备加成计算
`getEffectiveAtk()` SHALL return `state.player.atk` + sum of `atk` bonuses from all 7 equipment slots.
`getEffectiveDef()` SHALL return `state.player.def` + sum of `def` bonuses from all 7 equipment slots.

### Requirement: drawUI 属性显示
`drawUI` SHALL display `ATK 基础+加成` and `DEF 基础+加成` format, and inventory count `背包 N/16` in the top-right corner.

### Requirement: combatRound 使用有效属性
`combatRound` SHALL use `getEffectiveAtk()` and `getEffectiveDef()` for damage calculation.

## REMOVED Requirements

### Requirement: 旧物品模板（6 种）
**Reason**: 替换为 8 种新物品以匹配新装备槽位
**Migration**: 删除 `ITEM_TEMPLATES` 中的 HP药水/经验宝石/铁剑/攻击戒指/钢甲/防御护符，替换为新的 8 种

### Requirement: 旧装备槽位（weapon/armor）
**Reason**: 扩展为 7 槽位系统
**Migration**: `equipment: { weapon: null, armor: null }` → `equipment: { leftHand: null, rightHand: null, head: null, body: null, legs: null, feet: null, accessory: null }`
