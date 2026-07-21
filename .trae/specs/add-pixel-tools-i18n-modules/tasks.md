# Tasks

- [x] Task 1: 扩展 `js/i18n.js` 基础设施
  - [x] 扩展 `applyToDOM()` 支持独立 `data-i18n-title` 和 `data-i18n-aria-label` 属性
  - [x] 在 `[data-i18n]` 主循环中新增 aria-label 同步
  - [x] 移除硬编码 `floatingSettings` 元素 handler（已被通用属性 handler 覆盖）
  - [x] 新增所有模块所需 i18n 键（zh/en 对照，按模块组织带注释分隔）：`expr_error_*`(23) / `f3d_error_*`+`f3d_axis_label_*`(13) / `clock_*`(~30) / `chart_*`(10) / `nnvis_*`(9) / `nn_*`(4) / `offsetfit_func_*`(11) / `overfit_method_*`(3) / `physics_element_*`(3) / `pixelizer_error_*`(3)
  - [x] 不重复添加已存在的键（如 `arithmetic_hint_add`/`floating_settings_title`/`floating_avatar_title`/`btn_reload_title`）

- [x] Task 2: 修改 `index.html` 接入 data-i18n 属性
  - [x] 第 546 行：`arithmetic-hint` div 添加 `data-i18n="arithmetic_hint_add"`
  - [x] 第 708 行：predict-count input 添加 `data-i18n-title="predictor_predict_count_title"`
  - [x] 第 853-863 行：函数参数面板 h3/param-empty-hint/param-add-btn 加 `data-i18n`，play 按钮加 `data-i18n-title`，speed-label 加 `data-i18n`
  - [x] 第 1580 行：back-home 按钮加 `data-i18n-aria-label="btn_back_home_aria"`
  - [x] 第 1651 行：floating-settings 按钮加 `data-i18n-title` 和 `data-i18n-aria-label`
  - [x] 第 1659 行：floating-avatar 按钮加 `data-i18n-title` 和 `data-i18n-aria-label`

- [ ] Task 3: 修改 `js/expression-parser.js`（19 处错误消息）
  - [ ] Line 89: `throw new Error('表达式必须是字符串')` → `expr_error_must_be_string`
  - [ ] Line 92: `throw new Error('包含非法字符')` → `expr_error_illegal_char`
  - [ ] Line 136: `throw new Error('无效数字: ' + numStr)` → `expr_error_invalid_number` with `{val}`
  - [ ] Line 173: `throw new Error('未知标识符: ' + ident)` → `expr_error_unknown_identifier` with `{val}`
  - [ ] Line 197: `throw new Error('无法识别的字符: ' + ch)` → `expr_error_unrecognized_char` with `{val}`
  - [ ] Line 230: `expect(TokenType.X, '期望 X，得到 Y')` → `expr_error_expected_got` with `{expected}`/`{got}`
  - [ ] Line 237: `throw new Error('表达式嵌套过深')` → `expr_error_nested_too_deep`
  - [ ] Line 357: `expect(..., '函数调用后必须有左括号')` → `expr_error_func_lparen`
  - [ ] Line 359: `expect(..., '函数调用后必须有右括号')` → `expr_error_func_rparen`
  - [ ] Line 367: `expect(..., '缺少右括号')` → `expr_error_missing_rparen`
  - [ ] Line 372: `throw new Error('意外的 token: ' + ...)` → `expr_error_unexpected_token` with `{val}`
  - [ ] Line 379: `throw new Error('表达式有多余内容')` → `expr_error_extra_content`
  - [ ] Line 388: `throw new Error('无效的 AST 节点')` → `expr_error_invalid_ast`
  - [ ] Line 399: `throw new Error('变量 x 未定义')` → `expr_error_x_undefined`
  - [ ] Line 403: `throw new Error('未知变量: ' + node.name)` → `expr_error_unknown_variable` with `{val}`
  - [ ] Line 409: `throw new Error('参数 X 不是数字')` → `expr_error_param_not_number` with `{val}`
  - [ ] Line 413: `throw new Error('未定义参数: ' + node.name)` → `expr_error_undefined_param` with `{val}`
  - [ ] Line 419: `throw new Error('未知常量: ' + node.name)` → `expr_error_unknown_constant` with `{val}`
  - [ ] Line 425: `throw new Error('未知一元运算符: ' + node.op)` → `expr_error_unknown_unary_op` with `{val}`
  - [ ] Line 436: `throw new Error('除数不能为零')` → `expr_error_div_zero`
  - [ ] Line 441: `throw new Error('未知二元运算符: ' + node.op)` → `expr_error_unknown_binary_op` with `{val}`
  - [ ] Line 449: `throw new Error('未知函数: ' + node.name)` → `expr_error_unknown_function` with `{val}`
  - [ ] Line 455: `throw new Error('未知 AST 节点类型: ' + node.type)` → `expr_error_unknown_ast_type` with `{val}`
  - [ ] Line 466: `e.message || '解析错误'` → `expr_error_parse`
  - [ ] 采用防御性模式：`(typeof window !== 'undefined' && window.i18n && window.i18n.t('key', {val})) || '中文fallback'`

- [ ] Task 4: 修改 `js/function-3d.js`（13 处：10 错误 + 3 轴标签）
  - [ ] Line 417: x 轴标签 → `f3d_axis_label_x`
  - [ ] Line 420: y 轴标签 → `f3d_axis_label_y`
  - [ ] Line 423: z 轴标签 → `f3d_axis_label_z`
  - [ ] 10 处 `throw new Error('中文')` → 对应 `f3d_error_*` 键，带变量的用 `{val}` 占位符
  - [ ] 采用防御性 i18n 模式

- [ ] Task 5: 修改 `js/pixel-clock.js`（约 15 处）
  - [ ] Line 45: `WEEKDAY_CN` 数组 → 运行时 `i18n.t('clock_weekday_' + n)`
  - [ ] Line 582: `'星期' + week` → `i18n.t('clock_weekday_prefix')` 拼接或独立键
  - [ ] Line 593-594: 字体切换提示文本 → `clock_font_*` 键
  - [ ] Lines 727-728: `monthNames` 数组 → 运行时 `i18n.t('clock_month_' + n)`
  - [ ] Line 733: `year + '年 ' + monthNames[month]` → `i18n.t('clock_year_month', {year, month})`
  - [ ] Line 800: 日历事件提示 → `clock_event_hint`
  - [ ] Line 942: `'工作时间'`/`'休息时间'` 三元 → `clock_work_time`/`clock_rest_time`
  - [ ] Line 998: `'已完成 ' + n + ' 个番茄'` → `i18n.t('clock_pomodoro_done', {n})`
  - [ ] Lines 1034/1036/1038: `'开始'`/`'暂停'`/`'重置'` 按钮 → `clock_pomodoro_start`/`pause`/`reset`

- [ ] Task 6: 修改 `js/chart.js`（8 处）
  - [ ] Lines 817-819: 图例 `'输入序列'`/`'融合预测'`/`'各方法预测'` → `chart_legend_input`/`ensemble`/`methods`
  - [ ] Line 824: `'拟合曲线'` → `chart_legend_fit`
  - [ ] Lines 866-871: tooltip `'索引: '`/`'融合预测: '`/`'预测: '`/`'训练预测: '`/`'值: '` → 对应 `chart_tooltip_*` 键
  - [ ] Line 1345: `'方法权重 (降序)'` → `chart_weights_title`

- [ ] Task 7: 修改 `js/nn-visualizer.js`（8 处）
  - [ ] Lines 363-365: 图层标签 `'输入层 (N)'`/`'输出层 (N)'`/`'隐藏层 K (N)'` → `nnvis_layer_input`/`output`/`hidden` with `{n}`/`{idx}`
  - [ ] Lines 373/375: `'轮次: '`/`'损失: '` → `nnvis_epoch`/`nnvis_loss`
  - [ ] Line 432: `'损失曲线 (Loss Curve)'` → `nnvis_loss_curve_title`
  - [ ] Line 439: `'等待训练开始...'` → `nnvis_waiting`
  - [ ] Lines 892/896: 错误消息 → `nnvis_error_min_layers`/`nnvis_error_min_neurons`

- [ ] Task 8: 修改 `js/nn.js`（5 处，仅任务指定）
  - [ ] Lines 516-518: `'输入层(N)'`/`'隐藏层(N)'`/`'输出层'` → `nn_layer_input`/`hidden`/`output` with `{n}`
  - [ ] Line 532: `'误差: ' + options.loss.toFixed(4)` → `i18n.t('nn_loss_label', {val})`
  - [ ] 注意：训练阶段消息（如 `'训练完成'`/`'步骤 X/Y'`）不在本次任务范围，保持不动

- [ ] Task 9: 修改 `js/offsetfit.js`（11 处）+ `js/app.js`（1 处显示）
  - [ ] Line 169: `functionName: '常数函数'` → 新增 `functionNameKey: 'offsetfit_func_constant'`
  - [ ] Line 209: `'线性函数'` → `offsetfit_func_linear`
  - [ ] Line 257: `'二次函数'` → `offsetfit_func_quadratic`
  - [ ] Line 310: `'三次函数'` → `offsetfit_func_cubic`
  - [ ] Line 396: `'绝对值函数'` → `offsetfit_func_abs`
  - [ ] Line 476: `'倒数函数'` → `offsetfit_func_reciprocal`
  - [ ] Line 568: `'指数函数'` → `offsetfit_func_exponential`
  - [ ] Line 678: `'幂函数'` → `offsetfit_func_power`
  - [ ] Line 786: `'正弦函数'` → `offsetfit_func_sine`
  - [ ] Line 854: `'平方根绝对值'` → `offsetfit_func_sqrt_abs`
  - [ ] Line 948: `'双曲正切'` → `offsetfit_func_tanh`
  - [ ] Line 1013: `functionName: c.functionName` 复制逻辑同步加 `functionNameKey: c.functionNameKey`
  - [ ] `js/app.js` 第 1353 行：`best.functionName` → `(typeof window !== 'undefined' && window.i18n && window.i18n.t(best.functionNameKey)) || best.functionName`

- [ ] Task 10: 修改 `js/overfit.js`（1 处）
  - [ ] Line 419: 新增 `var METHOD_NAME_KEYS = ['overfit_method_high_degree', 'overfit_method_avg_high', 'overfit_method_cubic_spline']`
  - [ ] Line 441: `var name = METHOD_NAMES[m]` → `var name = (typeof window !== 'undefined' && window.i18n && window.i18n.t(METHOD_NAME_KEYS[m])) || METHOD_NAMES[m]`

- [ ] Task 11: 修改 `js/physics-sandbox.js`（1 处）
  - [ ] Line 43 后新增 `const ELEMENT_NAME_KEYS = ['physics_element_rubber', 'physics_element_water', 'physics_element_hydrogen']`
  - [ ] Line 442: `name: ELEMENT_NAMES[id]` → `name: (typeof window !== 'undefined' && window.i18n && window.i18n.t(ELEMENT_NAME_KEYS[id])) || ELEMENT_NAMES[id]`

- [ ] Task 12: 修改 `js/image-pixelizer.js`（3 处）
  - [ ] Line 269: `reject(new Error('未初始化，请先调用 init()'))` → `pixelizer_error_not_init`
  - [ ] Line 273: `reject(new Error('未提供文件'))` → `pixelizer_error_no_file`
  - [ ] Line 294: `reject(new Error('图片加载失败'))` → `pixelizer_error_load_failed`
  - [ ] 采用防御性 i18n 模式

- [ ] Task 13: 语法检查 + 验证
  - [ ] 对所有修改的 JS 文件运行 `node -c` 验证语法
  - [ ] 检查 i18n.js 中本次涉及的所有键 zh/en 一一对应
  - [ ] 抽样检查关键场景：错误消息带变量、图层标签带变量、函数名显示

# Task Dependencies
- Task 1、Task 2 已完成（i18n.js 基础设施和 index.html 属性接入）
- Task 3-12 互相独立，可并行执行（每个文件独立修改）
- Task 9 中 offsetfit.js 和 app.js 第 1353 行需同步修改
- Task 13（语法检查）依赖 Task 3-12 全部完成，必须最后执行
- **并行建议**：Task 3/4/5/6/7/8/10/11/12 完全独立，可分派给多个 sub-agent 并行；Task 9 单独处理（涉及两个文件联动）；Task 13 最后统一执行
