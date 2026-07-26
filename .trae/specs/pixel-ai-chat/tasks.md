# Tasks

- [ ] Task 1: i18n 键新增
  - [ ] `js/i18n.js`：新增所有像素 AI 相关 i18n 键（中英文）
  - [ ] 覆盖：页面标题/按钮/提示/设置弹窗/token统计/错误信息
  - [ ] 模型提供商名称和模型名称键

- [ ] Task 2: 首页卡片 + 页面 DOM
  - [ ] `index.html`：学习类新增「像素AI」卡片
  - [ ] `index.html`：新增 `pixel-ai-page` 页面 DOM（顶部栏/消息列表/输入区/状态栏）
  - [ ] `index.html`：新增「模型设置」弹窗 DOM

- [ ] Task 3: 聊天页面样式
  - [ ] `styles/pixel.css`：像素 AI 聊天页面整体布局样式
  - [ ] 用户/AI 消息气泡样式
  - [ ] 输入区样式（输入框 + 发送按钮）
  - [ ] 设置弹窗样式
  - [ ] Token 状态栏样式
  - [ ] 移动端响应式

- [ ] Task 4: 核心聊天逻辑（`js/pixel-ai.js`）
  - [ ] 模块初始化（PixelAI.init/start/stop）
  - [ ] localStorage 存取设置（provider/model/apiKey/baseUrl）
  - [ ] 9 家模型提供商的 API 调用封装
  - [ ] 消息发送与接收（整段返回 + 流式可选）
  - [ ] token 消耗统计与显示
  - [ ] 消息历史管理（messages 数组）
  - [ ] 清空对话功能
  - [ ] 错误处理（网络错误/API Key 无效/限流等）
  - [ ] 设置弹窗逻辑（提供商切换 → 模型列表更新）

- [ ] Task 5: `js/app.js` 接入
  - [ ] PAGE_IDS / ACTIVE_PAGES / RECENT_TRACKED_PAGES 新增 pixel-ai-page
  - [ ] 首页卡片点击事件绑定（showPixelAI 函数）
  - [ ] 返回按钮事件绑定
  - [ ] 教程按钮 i18n 支持
  - [ ] languagechange 时更新 UI 文本

- [ ] Task 6: README.md 更新
  - [ ] 工具目录学习类新增「像素AI」条目
  - [ ] 工具说明章节新增像素 AI 介绍
  - [ ] 更新日志新增条目

- [ ] Task 7: 语法检查 + 部署
  - [ ] `node -c` 检查所有修改的 JS 文件
  - [ ] 升级 service-worker.js CACHE_VERSION
  - [ ] git add + commit + push

# Task Dependencies
- Task 1（i18n 键）是其他所有任务的前提
- Task 2（DOM）+ Task 3（CSS）可并行
- Task 4（核心逻辑）依赖 Task 1 和 Task 2（DOM 存在）
- Task 5（app.js 接入）依赖 Task 1-4
- Task 6（README）独立，可并行
- Task 7 依赖所有任务完成
- **并行建议**：Task 1 先做 → Task 2+3+6 并行 → Task 4 → Task 5 → Task 7
