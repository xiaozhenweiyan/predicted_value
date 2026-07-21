# RPG Wiki 百科 + 层数改造 + 全站 i18n 完善 Spec

## Why
当前 RPG 游戏缺乏新手引导文档（玩家不知道 HP/EXP/ATK/DEF 含义、移动方式、物品/怪物机制）；"关卡"一词不够贴合地牢探险主题（应为"层数"）；网站部分硬编码中文文本未接入 i18n 系统，切换到英文时仍显示中文。

## What Changes
- **RPG 卡片新增"Wiki百科"按钮**（中文"Wiki百科"，英文"Wikipedia"），点击进入 Wiki 文档页面
- **Wiki 文档页面**：类文档式布局，含目录导航，介绍 HP/EXP/ATK/DEF 含义、移动操作、8 种物品介绍、怪物刷新机制
- **关卡→层数改造**（**BREAKING**）：所有"第 N 关"/"关卡"文本改为"第 N 层"/"层数"；`state.level` 字段保留但 UI 显示和 i18n 键改为 floor
- **全站 i18n 完善**：扫描所有 HTML/JS 文件中的硬编码中文文本，补充 i18n 键并接入 `data-i18n` 属性或 `i18n.t()` 调用
- 更新 `service-worker.js` CACHE_VERSION
- 更新 `README.md`

## Impact
- Affected code:
  - `js/pixel-rpg.js`：新增 Wiki 按钮和 Wiki 文档渲染逻辑；`state.level` 显示改为"第 N 层"；`drawUI`/`drawGameOver`/`drawMessage` 中"关"改为"层"
  - `index.html`：新增 Wiki 文档页面 DOM 容器；扫描所有硬编码中文文本并加 `data-i18n`
  - `js/i18n.js`：新增 Wiki 内容中英文键、层数相关键、扫描出的硬编码中文对应键
  - `js/app.js`：扫描硬编码中文（showToast/showTutorial/设置页等）并接入 i18n
  - 其他 `js/*.js`（预测器/函数系统/计算器/学习卡片/物理沙盒/像素艺术/绘图编辑器/音乐合成器/时钟等）：扫描硬编码中文按钮/提示/标签
  - `styles/pixel.css`：Wiki 文档页面样式
  - `service-worker.js`：CACHE_VERSION 升级
  - `README.md`：更新说明

## ADDED Requirements

### Requirement: RPG Wiki 百科按钮
The system SHALL add a "Wiki百科" button (English label "Wikipedia") in the RPG control panel alongside existing 开始/停止/重置 buttons.

#### Scenario: 显示 Wiki 按钮
- **WHEN** 用户进入 RPG 页面
- **THEN** 控制台区域显示"Wiki百科"按钮（中文模式）或"Wikipedia"按钮（英文模式）
- **AND** 按钮支持 `data-i18n` 属性，切换语言实时更新

#### Scenario: 点击 Wiki 按钮
- **WHEN** 用户点击"Wiki百科"按钮
- **THEN** 弹出 Wiki 文档页面（覆盖在游戏画面上或作为模态层）
- **AND** 文档页面有关闭按钮返回游戏

### Requirement: Wiki 文档内容
The system SHALL display a documentation-style Wiki page with a table of contents and the following sections:

1. **基础属性 / Basic Stats**
   - HP（生命值）：角色生命值，归零则游戏结束
   - EXP（经验值）：击败怪物获得，满经验升级
   - ATK（攻击力）：战斗中造成的伤害基数
   - DEF（防御力）：战斗中减免的伤害
2. **操作指南 / Controls**
   - 方向键 / WASD 移动
   - 点击/触摸地图自动寻路（BFS）
   - 空格 / J 攻击
   - 点击怪物自动攻击
   - R 重置
3. **物品图鉴 / Items**
   - 木剑：武器，ATK+2
   - 恢复药水I：消耗品，恢复 20 HP
   - 皮盔：头部防具，DEF+1
   - 皮甲：身体防具，DEF+3
   - 皮护腿：腿部防具，DEF+2
   - 皮靴：脚部防具，DEF+1
   - 经验宝石I：消耗品，+1 经验
   - 攻击戒指：饰品，ATK+1
4. **怪物机制 / Monster Mechanics**
   - 怪物类型：史莱姆/骷髅/哥布林
   - 刷新机制：每进入新层随机生成怪物
   - 战斗方式：回合制
   - 击败奖励：经验值

#### Scenario: 目录导航
- **WHEN** Wiki 文档显示
- **THEN** 顶部显示目录（4 个章节链接）
- **AND** 点击目录链接滚动到对应章节

#### Scenario: 中英文切换
- **WHEN** 用户在 Wiki 页面切换语言
- **THEN** 所有 Wiki 内容实时切换为中/英文

### Requirement: 关卡改为层数
The system SHALL replace all "关卡"/"第 N 关" terminology with "层数"/"第 N 层" throughout the RPG UI.

#### Scenario: drawUI 层数显示
- **WHEN** 渲染游戏画面顶部信息
- **THEN** 显示"第 N 层"（中文）或"Floor N"（英文），不再显示"第 N 关"

#### Scenario: 进入下一层提示
- **WHEN** 玩家走到出口进入下一关
- **THEN** 提示信息显示"进入第 N 层"（中文）或"Entering Floor N"（英文）

#### Scenario: 游戏结束提示
- **WHEN** 游戏结束
- **THEN** 显示"你倒在了第 N 层"（中文）或"You fell on Floor N"（英文）

#### Scenario: 重置提示
- **WHEN** 玩家点击重置
- **THEN** 提示"开始地牢冒险! 找到向下走廊进入下一层"（中文）

### Requirement: 全站 i18n 完善
The system SHALL ensure all user-facing Chinese text in HTML and JS files is registered in the i18n translation table and accessed via `data-i18n` attribute or `i18n.t()` calls.

#### Scenario: 切换到英文无中文残留
- **WHEN** 用户切换语言到英文
- **THEN** 所有可见 UI 文本（按钮、标签、提示、教程、错误信息）显示英文
- **AND** 不应有任何硬编码中文文本残留

#### Scenario: 扫描覆盖范围
- **WHEN** i18n 完善任务执行
- **THEN** 扫描以下文件中的硬编码中文：
  - `index.html`（所有页面 DOM）
  - `js/app.js`（showToast/showTutorial/设置页/参数面板）
  - `js/pixel-rpg.js`（Wiki 内容/层数/消息）
  - `js/predictors.js`/`js/weights.js`/`js/chart.js`（预测器提示）
  - `js/function-plotter.js`/`js/function-3d.js`（函数系统）
  - `js/expression-parser.js`（计算器错误）
  - `js/math-cards.js`/`js/math-cards-ext.js`（学习卡片标签）
  - `js/maze-generator.js`/`js/nn-visualizer.js`（像素编程）
  - `js/pixel-art.js`（像素艺术）
  - `js/pixel-drawing-editor.js`（绘图编辑器）
  - `js/pixel-music.js`（音乐合成器）
  - `js/physics-sandbox.js`（物理沙盒）
  - `js/image-pixelizer.js`（图像像素化）
  - `js/pixel-clock.js`（时钟）

## MODIFIED Requirements

### Requirement: RPG 控制台按钮组
RPG 控制台现有按钮：开始游戏 / 停止 / 重置。新增"Wiki百科"按钮，排列顺序：开始游戏 / 停止 / 重置 / Wiki百科。

### Requirement: RPG drawUI
`drawUI` 中"第 N 关"显示改为"第 N 层"（中文）/"Floor N"（英文），使用 i18n 键 `rpg_floor_label`。

## REMOVED Requirements

### Requirement: "关卡"相关 i18n 键
**Reason**: 替换为"层数"相关键
**Migration**: `rpg_level_label`/`rpg_level_enter` 等键改为 `rpg_floor_label`/`rpg_floor_enter`
