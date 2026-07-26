# 像素 AI 聊天功能 Spec

## Why
用户希望在学习类中新增一个"像素AI"工具，提供聊天界面，可以选择多种大模型、输入 API Key 与 AI 对话，并显示每次对话的 token 消耗。所有功能支持中英文切换。

## What Changes
- 首页学习类新增「像素AI / Pixel AI」卡片入口
- 新增 `pixel-ai-page` 聊天页面：顶部栏（标题/返回/设置）、消息列表、输入框、发送按钮
- 新增「模型设置」弹窗：选择模型提供商 + 具体模型、输入 API Key
- 支持的模型提供商：OpenAI、Anthropic、Google Gemini、通义千问、文心一言、DeepSeek、Mistral、Groq 等主流大模型
- 消息气泡样式（用户/AI 区分），支持流式输出或整段回复
- 每次对话显示 token 消耗（prompt tokens + completion tokens + total tokens）
- API Key 存储在 `localStorage`，不上传服务器
- 全部功能支持中英文切换（i18n）
- README.md 更新

## Impact
- Affected code: `index.html`（新页面 DOM + 首页卡片）、`styles/pixel.css`（聊天界面样式）、`js/app.js`（页面切换 + 初始化）、`js/pixel-ai.js`（核心聊天逻辑）、`js/i18n.js`（新 i18n 键）、`README.md`（工具目录 + 更新日志）

## ADDED Requirements

### Requirement: 首页卡片入口
学习类新增「像素AI」卡片，标题为「像素AI / Pixel AI」，描述为「AI 对话聊天 · 多模型支持 · Token 消耗统计」。点击卡片进入像素 AI 聊天页面。

#### Scenario: 从首页进入像素 AI
- **WHEN** 用户在首页点击「像素AI」卡片
- **THEN** 页面切换到 pixel-ai-page 聊天界面

### Requirement: 聊天界面布局
聊天页面 SHALL 包含以下元素：
- 顶部栏：返回按钮 + 页面标题「像素AI」 + 设置按钮（齿轮图标，点击打开模型设置）
- 消息列表区域：显示所有对话消息，用户消息在右侧，AI 消息在左侧
- 输入区域：多行文本输入框 + 发送按钮
- Token 状态栏：显示当前对话累计 token 消耗

#### Scenario: 发送消息
- **WHEN** 用户在输入框输入内容并点击发送（或按 Enter / Ctrl+Enter）
- **THEN** 用户消息立即显示在聊天列表右侧，输入框清空，显示"正在思考..."状态
- **AND** AI 回复到达后显示在聊天列表左侧

### Requirement: 模型设置弹窗
点击设置按钮 SHALL 弹出「模型设置」弹窗，包含：
- 模型提供商下拉选择（OpenAI / Anthropic / Google Gemini / 通义千问 / 文心一言 / DeepSeek / Mistral / Groq / 自定义 OpenAI 兼容）
- 具体模型输入框（下拉选择或自定义输入，根据提供商显示可选模型列表）
- API Key 输入框（密码类型，带显示/隐藏切换）
- API Base URL 输入框（自定义提供商时显示）
- 保存按钮 + 取消按钮
- 提示：API Key 仅保存在本地 localStorage，不上传任何服务器

#### Scenario: 保存模型设置
- **WHEN** 用户选择模型提供商、输入 API Key 并点击保存
- **THEN** 设置保存到 localStorage，弹窗关闭
- **AND** 下次打开弹窗时自动回填上次设置

### Requirement: 多模型支持
系统 SHALL 支持以下主流大模型提供商及其常用模型：
- **OpenAI**：gpt-4o, gpt-4o-mini, gpt-4-turbo, gpt-3.5-turbo
- **Anthropic**：claude-3-5-sonnet, claude-3-opus, claude-3-haiku
- **Google Gemini**：gemini-1.5-pro, gemini-1.5-flash, gemini-1.0-pro
- **通义千问（阿里）**：qwen-max, qwen-plus, qwen-turbo, qwen-long
- **文心一言（百度）**：ernie-4.0, ernie-3.5, ernie-lite
- **DeepSeek**：deepseek-chat, deepseek-coder
- **Mistral**：mistral-large, mistral-medium, mistral-small
- **Groq**：llama-3.3-70b, mixtral-8x7b, gemma-7b
- **自定义（OpenAI 兼容）**：用户手动输入模型名 + Base URL

所有模型调用使用对应提供商的 Chat Completions API（或兼容格式）。

#### Scenario: 不同模型提供商调用
- **WHEN** 用户选择不同模型提供商并发送消息
- **THEN** 系统调用对应提供商的 API 接口
- **AND** 自定义提供商使用 OpenAI 兼容格式调用用户指定的 Base URL

### Requirement: Token 消耗显示
每次 AI 回复后 SHALL 显示本次对话的 token 消耗：
- prompt_tokens：输入 token 数
- completion_tokens：输出 token 数
- total_tokens：总 token 数
- 累计总 token 数

Token 数据从 API 返回的 `usage` 字段中读取。若 API 不返回 usage，则显示估算值或"N/A"。

#### Scenario: 显示 token 消耗
- **WHEN** AI 回复完成
- **THEN** 在 AI 消息下方或底部状态栏显示 token 消耗信息

### Requirement: i18n 中英文支持
所有界面文本 SHALL 支持中英文切换：
- 页面标题、按钮、提示文字
- 模型提供商名称和模型名称
- 设置弹窗所有标签
- Token 统计标签
- 错误提示和状态提示

### Requirement: API Key 安全
API Key SHALL 仅保存在浏览器 localStorage 中，不上传任何第三方服务器。
- 存储 key：`pixel_ai_settings`
- 包含字段：provider、model、apiKey、baseUrl
- 绝不通过 console.log 输出 API Key

## MODIFIED Requirements
（无）

## REMOVED Requirements
（无）
