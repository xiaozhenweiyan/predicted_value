# Pixel Tools 模块 i18n 完善 Spec

## Why
`/workspace/pixel-tools/` 项目已建立 `window.i18n.t(key, params)` 双语国际化基础设施，并已在 `js/i18n.js` 中注册好所需的中英文键、在 `index.html` 中接入 `data-i18n`/`data-i18n-title`/`data-i18n-aria-label` 属性。但各业务 JS 模块（表达式解析器、3D 函数、像素时钟、图表、神经网络、偏移拟合、过拟合、物理沙盒、图像像素化）中仍存在大量硬编码中文字符串（错误消息、轴标签、图例、按钮文本、图层标签、函数名等），切换英文时这些字符串不会更新。需将这些字符串接入 i18n 系统，保持功能不变。

## What Changes
- **`js/expression-parser.js`**：将所有 `throw new Error('中文')` 改为 `throw new Error(i18n.t('key') || '中文')`，带变量的用 `i18n.t('key', {val: ...})` 形式，采用防御性 `(typeof window !== 'undefined' && window.i18n && window.i18n.t(...)) || '中文fallback'` 模式
- **`js/function-3d.js`**：10 处错误消息 + 3 处 x/y/z 轴标签 i18n 化
- **`js/pixel-clock.js`**：约 15 处，含 WEEKDAY_CN 数组、月份名、星期串、字体切换提示、日历事件提示、工作/休息时间、番茄钟标签与按钮
- **`js/chart.js`**：8 处，含图例（输入序列/融合预测/各方法预测/拟合曲线）、tooltip 标签、方法权重标题
- **`js/nn-visualizer.js`**：8 处，含图层标签（输入层/隐藏层/输出层）、轮次/损失标签、损失曲线标题、等待训练提示、网络结构错误
- **`js/nn.js`**：5 处，含 3 个图层标签 + 误差标签（仅任务指定的 5 处，训练阶段消息保持不动）
- **`js/offsetfit.js`**：11 处 `functionName` 字段新增 `functionNameKey` 字段，并在 `js/app.js` 第 1353 行显示处用 `i18n.t(best.functionNameKey)` 替代 `best.functionName`
- **`js/overfit.js`**：1 处，新增 `METHOD_NAME_KEYS` 数组，显示处用 `i18n.t(METHOD_NAME_KEYS[m])` 替代 `METHOD_NAMES[m]`
- **`js/physics-sandbox.js`**：1 处，新增 `ELEMENT_NAME_KEYS`，`getElement(id).name` 显示处改为运行时 `i18n.t(ELEMENT_NAME_KEYS[id])`
- **`js/image-pixelizer.js`**：3 处 Promise reject 错误消息 i18n 化

## Impact
- Affected specs: 无（独立 i18n 完善任务）
- Affected code:
  - `/workspace/pixel-tools/js/expression-parser.js`（19 处错误消息）
  - `/workspace/pixel-tools/js/function-3d.js`（13 处）
  - `/workspace/pixel-tools/js/pixel-clock.js`（约 15 处）
  - `/workspace/pixel-tools/js/chart.js`（8 处）
  - `/workspace/pixel-tools/js/nn-visualizer.js`（8 处）
  - `/workspace/pixel-tools/js/nn.js`（5 处）
  - `/workspace/pixel-tools/js/offsetfit.js`（11 处 functionName → functionNameKey）
  - `/workspace/pixel-tools/js/overfit.js`（1 处 METHOD_NAMES → METHOD_NAME_KEYS）
  - `/workspace/pixel-tools/js/physics-sandbox.js`（1 处 ELEMENT_NAMES → ELEMENT_NAME_KEYS）
  - `/workspace/pixel-tools/js/image-pixelizer.js`（3 处 reject 错误）
  - `/workspace/pixel-tools/js/app.js`（第 1353 行 offsetfit 显示处配合修改）

## ADDED Requirements

### Requirement: 错误消息 i18n 化
系统 SHALL 将 `expression-parser.js`、`function-3d.js`、`nn-visualizer.js`、`image-pixelizer.js` 中所有 `throw new Error('中文')` 和 `reject(new Error('中文'))` 替换为 i18n 调用，并保留中文 fallback 以应对 i18n 未加载场景。

#### Scenario: i18n 已加载时显示翻译
- **WHEN** i18n 模块已加载且用户切换到英文
- **THEN** 抛出的错误消息显示英文翻译（如 "Expression must be a string"）

#### Scenario: i18n 未加载时显示中文 fallback
- **WHEN** i18n 模块未加载（如单元测试环境）
- **THEN** 错误消息回退到原中文字符串，不抛出 TypeError

#### Scenario: 带变量的错误消息
- **WHEN** 错误消息含动态值（如 `无效数字: abc`）
- **THEN** 使用 `i18n.t('expr_error_invalid_number', {val: numStr})` 形式调用
- **AND** 中文翻译模板为 `无效数字: {val}`，英文为 `Invalid number: {val}`

### Requirement: 轴/图层/图例标签 i18n 化
系统 SHALL 将 `function-3d.js` 的 x/y/z 轴标签、`nn.js` 和 `nn-visualizer.js` 的图层标签、`chart.js` 的图例与 tooltip 标签接入 i18n。

#### Scenario: 3D 坐标轴标签
- **WHEN** 渲染 3D 函数图像
- **THEN** x/y/z 轴标签显示 `i18n.t('f3d_axis_label_x')` 等翻译结果
- **AND** 中文为 "X 轴"/"Y 轴"/"Z 轴"，英文为 "X Axis"/"Y Axis"/"Z Axis"

#### Scenario: 神经网络图层标签
- **WHEN** 渲染神经网络可视化
- **THEN** 图层标签显示 `i18n.t('nnvis_layer_input', {n})` 等翻译结果
- **AND** 中文为 "输入层 (N)"/"隐藏层 K (N)"/"输出层 (N)"，英文为 "Input Layer (N)"/"Hidden Layer K (N)"/"Output Layer (N)"

### Requirement: 数据层数组 i18n 化
系统 SHALL 为 `offsetfit.js` 的 11 个 functionName、`overfit.js` 的 METHOD_NAMES、`physics-sandbox.js` 的 ELEMENT_NAMES 新增对应的 key 字段/数组，并在显示处用 `i18n.t(key)` 替代直接读取中文字段。

#### Scenario: offsetfit 函数名显示
- **WHEN** 偏移算法显示最佳函数类型
- **THEN** `app.js` 第 1353 行调用 `i18n.t(best.functionNameKey)` 而非 `best.functionName`
- **AND** 中文模式显示 "常数函数"/"线性函数" 等，英文模式显示 "Constant"/"Linear" 等

#### Scenario: overfit 方法名显示
- **WHEN** 过拟合算法渲染方法图例
- **THEN** 调用 `i18n.t(METHOD_NAME_KEYS[m])` 而非 `METHOD_NAMES[m]`
- **AND** 中文显示 "最高次插值" 等，英文显示 "Highest-degree Interpolation" 等

#### Scenario: physics 元素名显示
- **WHEN** 物理沙盒获取元素显示名
- **THEN** 调用 `i18n.t(ELEMENT_NAME_KEYS[id])` 替代 `ELEMENT_NAMES[id]`
- **AND** 中文显示 "橡皮"/"水"/"氢气"，英文显示 "Rubber"/"Water"/"Hydrogen"

### Requirement: 像素时钟标签 i18n 化
系统 SHALL 将 `pixel-clock.js` 中的 WEEKDAY_CN 数组、月份名、星期串拼接、字体切换提示、日历事件提示、工作/休息时间文本、番茄钟标签与按钮文本接入 i18n。

#### Scenario: 星期显示
- **WHEN** 像素时钟渲染日期
- **THEN** 星期几显示 `i18n.t('clock_weekday_' + n)` 而非 `WEEKDAY_CN[n]`
- **AND** 中文显示 "一"/"二"...，英文显示 "Mon"/"Tue"...

#### Scenario: 番茄钟按钮
- **WHEN** 渲染番茄钟控制按钮
- **THEN** 按钮文本调用 `i18n.t('clock_pomodoro_start')`/`pause`/`reset`
- **AND** 中文显示 "开始"/"暂停"/"重置"，英文显示 "Start"/"Pause"/"Reset"

## MODIFIED Requirements

### Requirement: i18n 模块完整性
原 i18n 模块已在 `js/i18n.js` 注册所有新键（zh/en 对照），并提供 `t(key, params)` 翻译函数与 `applyToDOM()` 自动更新 DOM。本次完善确保各业务模块实际调用这些键，而非仅注册不用。

## REMOVED Requirements
无
