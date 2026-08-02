# 时空围棋 (TimeGo) Spec

## Why
用户需要一个完整可运行的《时空围棋》游戏：在传统围棋规则基础上叠加“时间回溯、时痕子、时间线分叉与修复、共振”等全新机制。项目需要前后端分离（Flask + SocketIO 后端 / HTML5 Canvas 前端），包含一个不依赖神经网络的 MCTS AI，最终代码需上传至 GitHub 新仓库。

## What Changes
- 新建项目目录 `timego/`，包含 `server.py`、`templates/index.html`、`static/style.css`、`static/script.js`、`requirements.txt`
- 后端实现 `TimeGoEngine` 核心引擎：25×25 棋盘、气/块/提子/打劫/禁自杀、棋谱存储
- 实现时间机制：回溯权、回溯落子判定、时间债务、时痕子（属性/衰减/充能/连锁消失）、时间线分叉与修复、毁灭性共振
- 实现终局判定与数目（时痕子目数处理、债务罚分、黑贴 6.5 目）
- 实现 Flask-SocketIO 事件接口：`connect/disconnect`、`make_move`、`time_back`、`pass_turn`、`resign`、`declare_end`，广播 `update_state`
- 实现 MCTS AI（执白）：候选动作生成、手工特征评估、800~1000 次模拟、快速走子策略
- 前端 Canvas 绘制 25×25 棋盘（A~Y 坐标）、时痕子特殊标记与剩余寿命、时间线树状图、状态面板
- 前端交互：常规落子、回溯流程、弃权、认输、声明终局、人机/双人模式切换
- 在 GitHub 新建仓库并推送全部代码（token 已在环境中）

## Impact
- 受影响范围：新建独立项目，不影响现有仓库其他模块
- 关键文件：`timego/server.py`（引擎 + 事件 + AI）、`timego/templates/index.html`、`timego/static/style.css`、`timego/static/script.js`、`timego/requirements.txt`
- 依赖：flask、flask-socketio、eventlet
- 外部依赖：GitHub CLI / git，环境变量中的 GitHub Token

## ADDED Requirements

### Requirement: 项目结构与依赖
系统 SHALL 在 `timego/` 目录下提供完整可运行的项目结构。

#### Scenario: 目录结构正确
- **GIVEN** 项目已创建
- **THEN** 存在 `server.py`、`templates/index.html`、`static/style.css`、`static/script.js`、`requirements.txt`
- **AND** `requirements.txt` 内容为 `flask`、`flask-socketio`、`eventlet`

#### Scenario: 一键运行
- **GIVEN** 已 `pip install -r requirements.txt`
- **WHEN** 执行 `python server.py`
- **THEN** 服务在本地端口启动，浏览器访问可进入游戏页面

### Requirement: 基础围棋规则
系统 SHALL 实现完整 25×25 围棋基础规则。

#### Scenario: 棋盘与落子顺序
- **GIVEN** 新对局开始
- **THEN** 棋盘为 25×25 空棋盘，黑先白后交替，黑贴 6.5 目

#### Scenario: 气/块/提子
- **WHEN** 一方落子使对方某棋块无气
- **THEN** 提掉该棋块所有棋子；若落子后己方无气且未提对方子，则禁止自杀

#### Scenario: 打劫禁止全局同形
- **WHEN** 一手棋使全局棋盘状态与历史某状态完全相同
- **THEN** 该手非法

#### Scenario: 棋谱存储
- **WHEN** 每一步落子完成
- **THEN** 记录坐标、颜色、落子后棋盘状态、当前轮谁下

### Requirement: 回溯权与时间债务
系统 SHALL 按规则管理回溯权与时间债务。

#### Scenario: 回溯权获得
- **WHEN** 己方提掉对方 1 子
- **THEN** 己方回溯权 +1，可累积

#### Scenario: 回溯权消耗
- **GIVEN** 轮到己方且回溯权 ≥ 1
- **WHEN** 己方选择回溯（代替常规落子）
- **THEN** 消耗 1 次回溯权

#### Scenario: 回溯目标限制
- **WHEN** 选择回溯目标
- **THEN** 只能回溯到己方曾落子的历史回合，且该节点未被锁定

#### Scenario: 透支与追偿点
- **GIVEN** 回溯权为 0
- **WHEN** 选择透支回溯
- **THEN** 最多欠债 3 次，标记追偿点 = 当前锋数 + 20
- **AND** 欠债期间提子优先还债

#### Scenario: 追偿点未还清惩罚
- **WHEN** 到达追偿点仍未还清债务
- **THEN** 跳过己方下一回合（对方连走两手），之后每 10 手再触发一次追偿

#### Scenario: 终局债务罚分
- **WHEN** 终局时仍欠债
- **THEN** 该方领地扣除（欠债次数 × 3 目）

### Requirement: 时痕子属性与放置
系统 SHALL 按“时痕子”规则处理回溯落下的棋子。

#### Scenario: 回溯落子判定
- **WHEN** 在当前活跃时间线最新棋盘上对目标坐标进行回溯落子
- **THEN** 若为空点：时痕子直接放置；若为己方棋子：时痕子替代该棋子；若为对方棋子：触发时间线分叉

#### Scenario: 时痕子不可被提
- **WHEN** 时痕子所在棋块无气
- **THEN** 普通棋子正常被提，时痕子保留原位

#### Scenario: 终局数目处理
- **WHEN** 终局数目
- **THEN** 时痕子占据的点计 0 目，它单独围住的空点也计 0 目

### Requirement: 时痕子衰减与充能
系统 SHALL 实现时痕子寿命周期。

#### Scenario: 稳定期
- **GIVEN** 时痕子诞生后 0~20 手
- **THEN** 全功能且不可被提

#### Scenario: 惰性期
- **GIVEN** 时痕子诞生后 21~40 手
- **THEN** 仍不可被提，但不再提供气，连接断开

#### Scenario: 湮灭
- **WHEN** 时痕子诞生达第 41 手起
- **THEN** 自动消失，交叉点变回空点

#### Scenario: 充能
- **WHEN** 在惰性期/湮灭前，于时痕子紧邻四格内使用一次回溯落子
- **THEN** 该时痕子寿命重置为稳定期

#### Scenario: 连锁消失
- **WHEN** 在同一时间线内通过回溯在过去提掉某子
- **THEN** 当前时间线未来所有依赖该子的棋子（含时痕子）全部消失

#### Scenario: 母手锁定消失
- **WHEN** 时痕子的母手被锁定为既定事实
- **THEN** 该时痕子立即消失

### Requirement: 时间线分叉与修复
系统 SHALL 实现时间线分叉与时空修复。

#### Scenario: 分叉触发
- **WHEN** 回溯落子目标当前被对方棋子占据
- **THEN** 创建新时间线，从回溯目标手开始继承当时状态；回溯子在新时间线作为普通棋子落下；原时间线冻结；战场切换到新时间线

#### Scenario: 修复触发
- **GIVEN** 新时间线中对方吃掉导致分叉的那枚棋子（普通子）
- **WHEN** 触发修复
- **THEN** 战场跳回原时间线冻结局面；原时间线第 1 手至分叉点（含）锁定为既定事实；新时间线吃病灶的那一手也锁定；新时间线其余废弃；导致分叉的时痕子因母手锁定而消失

### Requirement: 毁灭性共振
系统 SHALL 在每次落子/提子/修复后检测共振。

#### Scenario: 共振触发
- **WHEN** 任意 ≥3 枚时痕子位于同一直线（横/竖/斜，间距不限）
- **THEN** 立即移除该直线上所有棋子，交叉点变为永久空洞（不可落子），不可逆

### Requirement: 终局与胜负
系统 SHALL 按规则判定终局与数目。

#### Scenario: 终局条件
- **WHEN** 一方认输；或一方无合法着法且无可回溯节点；或双方连续弃权并声明放弃回溯
- **THEN** 对局终局，终局后禁止回溯

#### Scenario: 数目规则
- **WHEN** 终局数目
- **THEN** 按围棋规则数目，时痕子及其围空计 0 目，双活正常数目后减去时痕子目数，扣除债务罚分，黑贴 6.5 目，领地大者胜

### Requirement: 后端事件接口
系统 SHALL 通过 Flask-SocketIO 提供事件接口。

#### Scenario: 连接
- **WHEN** 客户端连接/断开
- **THEN** 后端处理 `connect`/`disconnect` 并维护会话

#### Scenario: 落子事件
- **WHEN** 客户端发送 `make_move(data)`
- **THEN** 后端执行常规落子并广播 `update_state`

#### Scenario: 回溯事件
- **WHEN** 客户端发送 `time_back(data)` 含 `{target_hand_index, coord}`
- **THEN** 后端执行回溯流程并广播 `update_state`

#### Scenario: 控制事件
- **WHEN** 客户端发送 `pass_turn`/`resign`/`declare_end`
- **THEN** 后端执行对应逻辑并广播 `update_state`

#### Scenario: 广播状态
- **WHEN** 任意事件处理完成
- **THEN** 广播 `update_state`，包含当前棋盘、元数据（轮次、回溯权、债务）、历史列表、时间线列表

#### Scenario: AI 自动落子
- **WHEN** 轮到 AI（白方）
- **THEN** 后端自动调用 AI 模块获取下一步并执行、广播

### Requirement: MCTS AI 模块
系统 SHALL 提供不依赖神经网络的 MCTS AI（执白）。

#### Scenario: 候选动作生成
- **WHEN** AI 生成候选
- **THEN** 常规落子仅考虑与现有棋子曼哈顿距离 ≤2 的空点；回溯候选最多最近 15 手内己方回合且目标点限制为热点区域；总量控制在 200~400

#### Scenario: 评估函数
- **WHEN** 评估局面
- **THEN** 使用手工特征：领地估算（距离衰减影响）、厚势/安全分（眼位与气）、时痕子价值分、资源分（回溯权与债务）、共振威胁分

#### Scenario: MCTS 模拟
- **WHEN** AI 决策
- **THEN** 每步执行 800~1000 次模拟，快速走子策略使用贪婪+随机；模拟中忽略分叉与修复，时痕子直接放置

### Requirement: 前端 Canvas 渲染
系统 SHALL 使用 Canvas 绘制棋盘与状态。

#### Scenario: 棋盘绘制
- **THEN** 绘制 25×25 棋盘，带 A~Y 坐标标签

#### Scenario: 状态面板
- **THEN** 显示当前活跃时间线手数、双方回溯权、债务状态

#### Scenario: 时痕子标记
- **THEN** 时痕子用特殊标记（光圈/半透明）并标注剩余寿命

#### Scenario: 时间线树状图
- **THEN** 显示时间线树（活跃/冻结/锁定节点）

### Requirement: 前端交互
系统 SHALL 提供完整交互。

#### Scenario: 常规落子
- **WHEN** 点击空点
- **THEN** 若合法则发送 `make_move`

#### Scenario: 回溯流程
- **WHEN** 点击“回溯”按钮 → 选择历史手 → 点击目标位置
- **THEN** 发送 `time_back`

#### Scenario: 控制按钮
- **THEN** 提供“弃权”“认输”“声明终局”按钮

#### Scenario: 模式切换
- **THEN** 支持人机对弈（玩家执黑）与双人本地对弈切换

#### Scenario: 接收广播更新
- **WHEN** 收到后端 `update_state` 广播
- **THEN** 更新棋盘与所有面板

### Requirement: GitHub 上传
系统 SHALL 将完整代码上传至 GitHub 新仓库。

#### Scenario: 新建仓库并推送
- **GIVEN** 环境中已配置 GitHub Token
- **WHEN** 执行上传流程
- **THEN** 新建 GitHub 仓库，推送 `timego/` 全部文件，返回仓库地址

## REMOVED Requirements
（无移除项，本 spec 为全新项目）
