# Checklist

## Wiki 百科按钮
- [x] RPG 控制台新增"Wiki百科"按钮（中文）/ "Wikipedia"（英文）
- [x] 按钮含 `data-i18n="rpg_wiki_btn"` 属性
- [x] 按钮点击触发 `showWiki()` 函数
- [x] 按钮排列在"重置"按钮之后

## Wiki 文档页面
- [x] 存在 Wiki 文档 DOM 容器（`id="rpg-wiki-panel"`）
- [x] 默认隐藏，点击按钮显示
- [x] 含关闭按钮返回游戏
- [x] 含目录导航（4 个章节链接）
- [x] 点击目录链接滚动到对应章节
- [x] 基础属性章节：HP/EXP/ATK/DEF 解释
- [x] 操作指南章节：移动/攻击/重置说明
- [x] 物品图鉴章节：8 种物品介绍
- [x] 怪物机制章节：怪物类型/刷新/战斗/奖励
- [x] 所有内容支持中英文切换

## Wiki 样式
- [x] `.rpg-wiki-panel` 模态层样式（覆盖游戏画面）
- [x] `.rpg-wiki-toc` 目录导航样式
- [x] `.rpg-wiki-section` 章节样式
- [x] `.rpg-wiki-close` 关闭按钮样式
- [x] 移动端响应式（可滚动）

## 层数改造
- [x] `drawUI` 显示"第 N 层"（中文）/"Floor N"（英文）
- [x] `drawGameOver` 显示"你倒在了第 N 层"
- [x] `reset()` 提示"开始地牢冒险! 找到向下走廊进入下一层"
- [x] 进入下一关提示改为"进入第 N 层"
- [x] i18n 新增 `rpg_floor_label`/`rpg_floor_fell`/`rpg_floor_start`/`rpg_floor_enter` 中英文键
- [x] 无"关卡"/"第 N 关"残留文本

## 全站 i18n 完善 - 扫描
- [x] `index.html` 扫描完成，硬编码中文清单已生成
- [x] `js/app.js` 扫描完成
- [x] `js/pixel-rpg.js` 扫描完成
- [x] `js/predictors.js`/`js/weights.js`/`js/chart.js` 扫描完成
- [x] `js/function-plotter.js`/`js/function-3d.js` 扫描完成
- [x] `js/expression-parser.js` 扫描完成
- [x] `js/math-cards.js`/`js/math-cards-ext.js` 扫描完成
- [x] `js/maze-generator.js`/`js/nn-visualizer.js` 扫描完成
- [x] `js/pixel-art.js` 扫描完成
- [x] `js/pixel-drawing-editor.js` 扫描完成
- [x] `js/pixel-music.js` 扫描完成
- [x] `js/physics-sandbox.js` 扫描完成
- [x] `js/image-pixelizer.js` 扫描完成
- [x] `js/pixel-clock.js` 扫描完成

## 全站 i18n 完善 - 修改
- [x] `js/i18n.js` 新增所有缺失 i18n 键（中英文对照）
- [x] `index.html` 硬编码中文加 `data-i18n` 属性
- [x] 各 `js/*.js` 硬编码中文改为 `i18n.t()` 或 `data-i18n`（5 个目标文件已完成：pixel-rpg.js / math-cards-ext.js / predictors.js / math-cards.js / app.js；weights.js/chart.js/overfit.js 显示站点已接入）
- [x] 功能保持不变

## 全站 i18n 完善 - 检查
- [x] 扫描所有 HTML/JS 无硬编码中文残留（排除注释/console.log）—— 4 处 Canvas 绘制文本残留已全部修复
- [x] i18n.js 中 zh/en 键一一对应
- [x] 切换英文模式所有页面 UI 文本完整翻译
- [x] 检查报告生成，无未翻译项

## 部署
- [x] `service-worker.js` CACHE_VERSION 升级（v23 → v24）
- [x] README.md 更新（Wiki/层数/i18n 说明 + 更新日志）
- [x] 所有修改的 JS 文件 `node -c` 语法检查通过（17 个文件：app/chart/expression-parser/function-3d/i18n/image-pixelizer/math-cards-ext/math-cards/nn-visualizer/nn/offsetfit/overfit/physics-sandbox/pixel-clock/pixel-rpg/predictors/service-worker）
- [x] git commit + push 成功
