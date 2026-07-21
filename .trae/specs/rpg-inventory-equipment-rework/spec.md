# RPG 物品栏/装备栏交互重构 Spec

## Why
上一版 RPG 的物品栏格子偏小、装备槽只有 weapon/armor 两个分类、物品体系不完整，且缺乏点击物品查看属性/穿戴/使用/交换/卸下等核心交互。本次重构要让 RPG 的物品与装备系统具备完整、可玩、像素风一致的交互体验。

## What Changes
- **背包格子放大**：每格尺寸从原 ~32px 增大到 ~48px（CSS 调整），物品数量徽标固定在格子右下角，白色字体、透明背景、无方块底色。
- **装备栏重置为 7 个槽位**：左手 / 右手 / 头部 / 身体 / 腿 / 脚 / 饰品。**BREAKING**：废弃旧 `equipment: { weapon, armor }` 结构，改为 `equipment: { leftHand, rightHand, head, body, legs, feet, accessory }`。
- **物品体系全部替换**：删除旧 6 种物品模板，新增 8 种：
  1. 木剑（武器·单手，木质像素图标，+ATK）
  2. 恢复药水 I（消耗品，瓶身 + 红色液体，使用恢复 HP）
  3. 皮盔（头部防具，皮革帽样式，+DEF）
  4. 皮甲（身体防具，皮革胸甲样式，+DEF）
  5. 皮护腿（腿部防具，皮革护腿样式，+DEF）
  6. 皮靴（脚部防具，皮革靴样式，+DEF）
  7. 经验宝石 I（消耗品，使用 +1 经验）
  8. 攻击戒指（饰品，+1 ATK）
- **物品交互完整化**：
  - 点击背包物品 → 在背包下方"物品属性区"显示该物品的名称/类型/属性描述。
  - 若物品为可使用（消耗品）→ 属性区出现"使用"按钮。
  - 若物品为可穿戴（武器/防具/饰品）→ 属性区出现"穿戴"按钮。
  - 选中一个有物品的格子后再点击另一格子（无论对方有无物品）→ 执行交换/移动。
  - 选中后点击背包以外的任何区域 → 取消选中。
  - 点击装备栏物品 → 属性区显示该装备信息，并出现"取消佩戴"按钮。
- **README.md 更新**：在更新日志中新增本次重构条目，详细说明背包放大/装备槽重置/物品体系替换/交互流程。

## Impact
- Affected code:
  - `js/pixel-rpg.js`：`ITEM_TEMPLATES` 重写、`state.player.equipment` 结构变更、`equipItem`/`unequipItem`/`useItem`/`renderInventory`/`renderEquipment`/`getEffectiveAtk`/`getEffectiveDef`/`drawUI`/`openChest`/`reset` 全部同步修改；新增"物品属性区"渲染与点击交互逻辑。
  - `index.html`：RPG 页面 DOM 调整——装备栏改为 7 槽位、背包下方新增"物品属性区"DOM。
  - `styles/pixel.css`：背包格子放大、数量徽标样式、装备槽 7 格布局、属性区样式。
  - `js/i18n.js`：新增装备槽名称（左手/右手/头部/身体/腿/脚/饰品）、按钮文案（使用/穿戴/取消佩戴）、物品名/描述中英文。
  - `service-worker.js`：升级 `CACHE_VERSION`。
  - `README.md`：更新日志。

## ADDED Requirements

### Requirement: 背包格子放大与数量徽标
系统 SHALL 将背包每格尺寸增大到至少 48×48px（CSS），物品数量徽标 SHALL 固定显示在格子右下角，字体白色、背景透明、无方块底色。

#### Scenario: 数量徽标显示
- **WHEN** 背包格子中物品数量 > 1
- **THEN** 在该格子右下角显示白色数字，背景完全透明，不遮挡物品图标

#### Scenario: 数量为 1 不显示徽标
- **WHEN** 物品数量 === 1
- **THEN** 不显示数量徽标

### Requirement: 装备栏 7 槽位
系统 SHALL 提供以下 7 个装备槽位：左手、右手、头部、身体、腿、脚、饰品。每个槽位只能装备对应 slot 类型的物品。

#### Scenario: 双手武器占用双手
- **WHEN** 玩家穿戴一把单手木剑到左手槽
- **THEN** 右手槽仍空着，可装备另一件单手物品

#### Scenario: 类型不匹配拒绝装备
- **WHEN** 玩家试图将皮盔（head 类型）装备到身体槽
- **THEN** 装备失败，提示"类型不匹配"

### Requirement: 新物品体系
系统 SHALL 提供以下 8 种物品，每种含 `id/name/type/slot/icon/desc` 字段及对应属性（武器含 atk、防具含 def、消耗品含 effect）：
1. `wooden_sword` 木剑（weapon, leftHand/rightHand, +2 ATK）
2. `potion_i` 恢复药水 I（consumable, 恢复 30 HP, stackable）
3. `leather_helmet` 皮盔（armor, head, +1 DEF）
4. `leather_armor` 皮甲（armor, body, +2 DEF）
5. `leather_leggings` 皮护腿（armor, legs, +1 DEF）
6. `leather_boots` 皮靴（armor, feet, +1 DEF）
7. `gem_i` 经验宝石 I（consumable, +1 经验, stackable）
8. `atk_ring` 攻击戒指（accessory, accessory slot, +1 ATK）

#### Scenario: 宝箱掉落新物品
- **WHEN** 玩家打开宝箱
- **THEN** 从上述 8 种物品中加权随机掉落一件，按类型入背包或即时消耗

### Requirement: 物品属性区与交互
系统 SHALL 在背包区域下方渲染"物品属性区"，根据当前选中状态显示不同内容。

#### Scenario: 选中背包可使用物品
- **WHEN** 玩家点击背包中的恢复药水
- **THEN** 属性区显示物品名/类型/描述，并出现"使用"按钮；点击使用则消耗 1 个并应用效果

#### Scenario: 选中背包可穿戴物品
- **WHEN** 玩家点击背包中的皮盔
- **THEN** 属性区显示物品属性，并出现"穿戴"按钮；点击穿戴则进入对应装备槽

#### Scenario: 背包格之间交换
- **WHEN** 玩家先点击格子 A（有物品），再点击格子 B（空或有物品）
- **THEN** A 与 B 内容交换；若 B 中已有不同类型堆叠物品则不合并，直接占位

#### Scenario: 点击外部取消选中
- **WHEN** 玩家选中一个有物品的格子后，点击背包以外的任何区域
- **THEN** 取消选中，属性区清空

#### Scenario: 选中装备栏物品
- **WHEN** 玩家点击装备栏中已装备的物品
- **THEN** 属性区显示该装备信息，并出现"取消佩戴"按钮；点击则卸下并放回背包（背包满则提示）

### Requirement: README 更新
README.md SHALL 在更新日志中新增本次重构条目，详细说明 5 项改动：格子放大、7 装备槽、8 种新物品、完整交互流程、CACHE_VERSION 升级。

## MODIFIED Requirements

### Requirement: getEffectiveAtk / getEffectiveDef
计算玩家有效攻击/防御时，SHALL 遍历 7 个装备槽位累加装备的 atk/def 加成（旧版只读 weapon/armor 两个字段）。

### Requirement: drawUI 装备加成显示
`drawUI` 中 ATK/DEF 显示 SHALL 显示为 `基础值 + 装备加成` 格式（如 `ATK 5+2`），加成为 0 时只显示基础值。

## REMOVED Requirements

### Requirement: 旧 6 种物品模板
**Reason**: 替换为新的 8 种物品体系，旧模板不再使用。
**Migration**: `ITEM_TEMPLATES` 常量完全重写，旧存档中持有的旧物品将无法识别（RPG 无持久化，无迁移负担）。

### Requirement: 旧 equipment: { weapon, armor } 结构
**Reason**: 替换为 7 槽位结构。
**Migration**: `state.player.equipment` 重置为 `{ leftHand, rightHand, head, body, legs, feet, accessory }`，`reset()` 同步更新。
