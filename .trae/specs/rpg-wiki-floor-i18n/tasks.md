# Tasks

- [x] Task 1: 扫描全站硬编码中文文本（调研阶段）
  - [x] 扫描 `index.html` 所有页面 DOM 中的硬编码中文
  - [x] 扫描 `js/*.js` 所有文件中的硬编码中文字符串（按钮文本/提示/标签/错误信息）
  - [x] 汇总清单：文件路径 + 行号 + 原文 + 建议 i18n 键名

- [x] Task 2: 新增 Wiki 百科按钮 + Wiki 文档页面
  - [x] `index.html`：在 RPG 控制台按钮组新增"Wiki百科"按钮（`id="btn-rpg-wiki"`，`data-i18n="rpg_wiki_btn"`）
  - [x] `index.html`：新增 Wiki 文档页面 DOM 容器（`id="rpg-wiki-panel"`，默认 hidden）
  - [x] `js/pixel-rpg.js`：实现 `showWiki()`/`hideWiki()` 函数，绑定按钮点击事件
  - [x] Wiki 页面含关闭按钮、目录导航、4 大章节内容

- [x] Task 3: 编写 Wiki 文档内容（中英文）
  - [x] `js/i18n.js`：新增 Wiki 相关 i18n 键（`rpg_wiki_title`/`rpg_wiki_toc_*`/`rpg_wiki_section_*`/`rpg_wiki_stats_*`/`rpg_wiki_controls_*`/`rpg_wiki_items_*`/`rpg_wiki_monsters_*`）
  - [x] 基础属性章节：HP/EXP/ATK/DEF 解释
  - [x] 操作指南章节：移动/攻击/重置说明
  - [x] 物品图鉴章节：8 种物品介绍
  - [x] 怪物机制章节：怪物类型/刷新/战斗/奖励

- [x] Task 4: Wiki 文档页面样式
  - [x] `styles/pixel.css`：新增 `.rpg-wiki-panel` 模态层样式（覆盖在游戏上）
  - [x] `.rpg-wiki-toc` 目录导航样式
  - [x] `.rpg-wiki-section` 章节样式
  - [x] `.rpg-wiki-close` 关闭按钮样式
  - [x] 响应式适配（移动端可滚动）

- [x] Task 5: 关卡→层数改造（RPG 内部）
  - [x] `js/pixel-rpg.js`：`drawUI` 中"第 N 关"改为 i18n 键 `rpg_floor_label` + state.level
  - [x] `drawGameOver` 中"你倒在了第 N 关"改为 `rpg_floor_fell` + state.level
  - [x] `reset()` 中提示信息"开始地牢冒险"改为 `rpg_floor_start`
  - [x] 进入下一关提示改为 `rpg_floor_enter`
  - [x] `js/i18n.js`：新增 `rpg_floor_label`/`rpg_floor_fell`/`rpg_floor_start`/`rpg_floor_enter` 中英文键
  - [x] 删除/废弃旧的 `rpg_level_*` 键（如有）

- [x] Task 6: 全站 i18n 完善 - 修改阶段（sub-agent A）
  - [x] 根据 Task 1 清单，在 `js/i18n.js` 中新增所有缺失的 i18n 键（中英文对照）
  - [x] 修改 `index.html`：硬编码中文加 `data-i18n` 属性
  - [x] 修改各 `js/*.js`：硬编码中文改为 `i18n.t('key')` 调用或 `data-i18n` 属性（已完成 5 个目标文件：pixel-rpg.js / math-cards-ext.js / predictors.js / math-cards.js / app.js，含 weights.js/chart.js/overfit.js 显示站点）
  - [x] 保持功能不变，仅接入 i18n

- [x] Task 7: 全站 i18n 完善 - 检查阶段（sub-agent B，最后执行）
  - [x] 扫描所有 HTML/JS 文件，确认无硬编码中文残留（排除注释和 console.log）
  - [x] 切换到英文模式，模拟检查所有页面 UI 文本是否完整翻译
  - [x] 检查 i18n.js 中所有键的 zh/en 是否一一对应
  - [x] 输出检查报告：未翻译项 / 缺失键 / 不一致项
  - [x] 如发现问题，创建修复任务（已修复 4 处 Canvas 绘制文本残留：pixel-clock.js × 1、function-3d.js × 3）

- [x] Task 8: 升级 service-worker.js CACHE_VERSION
  - [x] v23 → v24

- [x] Task 9: 更新 README.md
  - [x] RPG 章节新增 Wiki 百科说明
  - [x] "关卡"改为"层数"
  - [x] 更新日志新增本次改动条目
  - [x] i18n 章节说明全站已支持中英文

- [x] Task 10: 语法检查 + 提交推送
  - [x] `node -c` 检查所有修改的 JS 文件（17 个文件全部通过：app/chart/expression-parser/function-3d/i18n/image-pixelizer/math-cards-ext/math-cards/nn-visualizer/nn/offsetfit/overfit/physics-sandbox/pixel-clock/pixel-rpg/predictors/service-worker）
  - [x] git add + commit
  - [x] git push 到 GitHub

# Task Dependencies
- Task 1（扫描）是 Task 6/7 的前提
- Task 2-5 修改 RPG 相关，可由一个 sub-agent 顺序执行
- Task 6（修改）依赖 Task 1（扫描清单）
- Task 7（检查）依赖 Task 2-6 全部完成，必须最后执行
- Task 8 独立，可并行
- Task 9 依赖 Task 2-7 完成
- Task 10 依赖所有任务完成
- **并行建议**：Task 2-5（RPG Wiki+层数）+ Task 1（扫描）可并行启动；Task 6 依赖 Task 1；Task 7 必须最后
