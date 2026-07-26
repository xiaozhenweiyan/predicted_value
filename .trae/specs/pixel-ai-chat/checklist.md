# Checklist

## 首页卡片入口
- [ ] 学习类有「像素AI」卡片
- [ ] 卡片标题支持中英文（data-i18n）
- [ ] 卡片描述支持中英文
- [ ] 点击卡片进入像素 AI 页面

## 聊天界面
- [ ] 页面有顶部栏（返回按钮 + 标题 + 设置按钮）
- [ ] 消息列表区域存在
- [ ] 用户消息气泡在右侧
- [ ] AI 消息气泡在左侧
- [ ] 输入区有多行文本输入框 + 发送按钮
- [ ] 按 Enter 发送（Shift+Enter 换行）
- [ ] Token 状态栏显示累计消耗

## 模型设置弹窗
- [ ] 设置按钮可打开弹窗
- [ ] 提供商下拉选择（9 家 + 自定义）
- [ ] 模型选择/输入框
- [ ] API Key 输入框（密码类型 + 显示隐藏切换）
- [ ] Base URL 输入框（自定义时显示）
- [ ] 保存按钮和取消按钮
- [ ] 安全提示文本（API Key 仅本地存储）
- [ ] 设置保存到 localStorage
- [ ] 下次打开自动回填

## 多模型支持
- [ ] OpenAI 系列模型可调用
- [ ] Anthropic 系列模型可调用
- [ ] Google Gemini 系列模型可调用
- [ ] 通义千问系列模型可调用
- [ ] 文心一言系列模型可调用
- [ ] DeepSeek 系列模型可调用
- [ ] Mistral 系列模型可调用
- [ ] Groq 系列模型可调用
- [ ] 自定义 OpenAI 兼容格式可调用

## Token 消耗
- [ ] 每次回复显示 prompt_tokens
- [ ] 每次回复显示 completion_tokens
- [ ] 每次回复显示 total_tokens
- [ ] 累计总 token 数显示
- [ ] API 不返回 usage 时有兜底显示

## i18n 支持
- [ ] 所有界面文本支持中英文
- [ ] 模型提供商名称支持中英文
- [ ] 错误提示支持中英文
- [ ] 切换语言后界面文本立即更新

## 安全
- [ ] API Key 仅存储在 localStorage
- [ ] 不在 console.log 中输出 API Key
- [ ] 不上传任何服务器

## 部署
- [ ] service-worker.js CACHE_VERSION 升级
- [ ] 所有修改的 JS 文件 node -c 通过
- [ ] README.md 已更新
- [ ] git commit + push 成功
