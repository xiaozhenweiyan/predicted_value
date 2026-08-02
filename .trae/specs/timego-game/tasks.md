# Tasks

- [ ] Task 1: 初始化项目结构与依赖文件
  - [ ] SubTask 1.1: 创建 `timego/` 目录及子目录 `templates/`、`static/`
  - [ ] SubTask 1.2: 创建 `requirements.txt`（flask、flask-socketio、eventlet）
  - [ ] SubTask 1.3: 创建 `server.py` 最小 Flask-SocketIO 启动框架（含 `index` 路由与 `update_state` 广播骨架）

- [ ] Task 2: 实现基础围棋引擎（TimeGoEngine 核心数据结构）
  - [ ] SubTask 2.1: 定义棋盘 25×25 表示、颜色常量、`Stone`/`Timeline`/`HandRecord` 等数据类
  - [ ] SubTask 2.2: 实现棋块识别与气计算（BFS/DFS）
  - [ ] SubTask 2.3: 实现落子合法性校验：占点、自杀禁手（除非提子）、打劫（全局同形再现禁止）
  - [ ] SubTask 2.4: 实现提子逻辑（移除无气对方块）、棋谱记录（坐标/颜色/落子后状态/当前轮）

- [ ] Task 3: 实现回溯权与时间债务
  - [ ] SubTask 3.1: 双方回溯权计数（提对方 1 子 +1，回溯 -1）
  - [ ] SubTask 3.2: 回溯目标合法性（仅己方曾落子且未锁定的历史回合）
  - [ ] SubTask 3.3: 透支机制（最多欠债 3 次、追偿点 = 当前手数 + 20、提子优先还债）
  - [ ] SubTask 3.4: 追偿点惩罚（跳过己方下一回合，每 10 手再触发）
  - [ ] SubTask 3.5: 终局债务罚分（欠债次数 × 3 目）

- [ ] Task 4: 实现时痕子机制
  - [ ] SubTask 4.1: 时痕子数据结构（绑定母手、诞生手数、寿命状态）
  - [ ] SubTask 4.2: 回溯落子判定（空点放置 / 替换己方 / 触发分叉）
  - [ ] SubTask 4.3: 时痕子不可被提（提块时保留时痕子，仅提普通子）
  - [ ] SubTask 4.4: 衰减周期（稳定期 0~20 / 惰性期 21~40 不提供气且断连 / 湮灭 41+）
  - [ ] SubTask 4.5: 充能（紧邻四格内回溯落子重置寿命）
  - [ ] SubTask 4.6: 连锁消失（回溯过去提子导致依赖子消失）与母手锁定消失

- [ ] Task 5: 实现时间线分叉与修复
  - [ ] SubTask 5.1: 时间线列表与活跃时间线 ID 维护
  - [ ] SubTask 5.2: 分叉逻辑（回溯落子撞对方子 → 新时间线继承状态，回溯子作普通子，原时间线冻结，切换战场）
  - [ ] SubTask 5.3: 修复逻辑（新时间线吃掉病灶子 → 跳回原时间线，锁定第 1 手至分叉点，锁定吃病灶手，废弃其余，时痕子消失）
  - [ ] SubTask 5.4: 节点锁定标记与回溯禁止

- [ ] Task 6: 实现毁灭性共振检测
  - [ ] SubTask 6.1: 落子/提子/修复后扫描所有时痕子
  - [ ] SubTask 6.2: 检测横/竖/斜直线 ≥3 枚时痕子
  - [ ] SubTask 6.3: 移除直线上所有棋子并将交叉点标记为永久空洞（不可落子）

- [ ] Task 7: 实现终局判定与数目
  - [ ] SubTask 7.1: 终局条件（认输 / 无合法着法且无可回溯 / 双方连续弃权并声明放弃回溯）
  - [ ] SubTask 7.2: 终局后禁止回溯
  - [ ] SubTask 7.3: 数目算法（领地归属、双活正常数目、时痕子点与其围空计 0 目、扣债务罚分、黑贴 6.5 目）

- [ ] Task 8: 实现 Flask-SocketIO 事件接口
  - [ ] SubTask 8.1: `connect`/`disconnect` 会话维护
  - [ ] SubTask 8.2: `make_move` / `time_back` / `pass_turn` / `resign` / `declare_end` 处理
  - [ ] SubTask 8.3: `update_state` 广播（棋盘、元数据、历史、时间线树）
  - [ ] SubTask 8.4: 模式管理（人机/双人）与 AI 触发钩子

- [ ] Task 9: 实现 MCTS AI 模块
  - [ ] SubTask 9.1: 候选动作生成（曼哈顿距离 ≤2 空点 + 最近 15 手己方回溯热点，总量 200~400）
  - [ ] SubTask 9.2: 手工特征评估函数（领地/厚势/时痕价值/资源/共振威胁）
  - [ ] SubTask 9.3: MCTS 主循环（800~1000 次模拟、UCB1 选择、快速走子贪婪+随机）
  - [ ] SubTask 9.4: 模拟简化（忽略分叉与修复，时痕子直接放置）
  - [ ] SubTask 9.5: AI 落子后广播

- [ ] Task 10: 实现前端 HTML 与 CSS
  - [ ] SubTask 10.1: `templates/index.html` 布局（棋盘 Canvas、状态面板、时间线树、按钮、模式切换）
  - [ ] SubTask 10.2: `static/style.css` 样式（棋盘容器、面板、按钮、时间线节点状态着色）

- [ ] Task 11: 实现前端 JavaScript 交互与渲染
  - [ ] SubTask 11.1: Canvas 绘制 25×25 棋盘与 A~Y 坐标
  - [ ] SubTask 11.2: 绘制普通棋子、时痕子（光圈/半透明 + 剩余寿命）、永久空洞
  - [ ] SubTask 11.3: SocketIO 客户端连接与 `update_state` 处理
  - [ ] SubTask 11.4: 点击落子、回溯流程（选历史手 → 选目标）、弃权/认输/声明终局按钮
  - [ ] SubTask 11.5: 时间线树状图渲染与切换、模式切换

- [ ] Task 12: 本地运行验证与缺陷修复
  - [ ] SubTask 12.1: 安装依赖并启动 `python server.py`，浏览器访问页面
  - [ ] SubTask 12.2: 验证人机对弈落子、提子、回溯、终局基本流程
  - [ ] SubTask 12.3: 修复运行时出现的语法/逻辑错误

- [ ] Task 13: 上传至 GitHub 新仓库
  - [ ] SubTask 13.1: 使用环境中的 GitHub Token 配置认证
  - [ ] SubTask 13.2: 新建仓库（如 `timego`），初始化 git 并提交 `timego/` 全部文件
  - [ ] SubTask 13.3: 推送并返回仓库 URL

# Task Dependencies
- Task 2 依赖 Task 1
- Task 3、Task 4 依赖 Task 2
- Task 5 依赖 Task 4
- Task 6 依赖 Task 4
- Task 7 依赖 Task 3、Task 4、Task 6
- Task 8 依赖 Task 2~7
- Task 9 依赖 Task 8（需可触发 AI）
- Task 10、Task 11 依赖 Task 8（需事件接口确定）
- Task 12 依赖 Task 9、Task 11
- Task 13 依赖 Task 12
