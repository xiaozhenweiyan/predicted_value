# Checklist

## 基础设施（已完成）
- [x] `js/i18n.js` 的 `applyToDOM()` 支持独立 `data-i18n-title` 属性（不要求元素同时有 `data-i18n`）
- [x] `js/i18n.js` 的 `applyToDOM()` 支持独立 `data-i18n-aria-label` 属性
- [x] `[data-i18n]` 主循环中新增 aria-label 同步逻辑
- [x] 移除硬编码 `floatingSettings` handler（被通用属性 handler 覆盖）
- [x] zh 翻译对象新增所有模块键（expr_error_* / f3d_* / clock_* / chart_* / nnvis_* / nn_* / offsetfit_func_* / overfit_method_* / physics_element_* / pixelizer_error_*）
- [x] en 翻译对象新增对应英文键
- [x] 未重复添加已存在的键
- [x] `index.html` 第 546/708/853-863/1580/1651/1659 行已添加 data-i18n 相关属性

## expression-parser.js
- [ ] Line 89 `expr_error_must_be_string` 已接入（防御性模式 + 中文 fallback）
- [ ] Line 92 `expr_error_illegal_char` 已接入
- [ ] Line 136 `expr_error_invalid_number` 已接入，使用 `{val}` 占位符
- [ ] Line 173 `expr_error_unknown_identifier` 已接入，使用 `{val}`
- [ ] Line 197 `expr_error_unrecognized_char` 已接入，使用 `{val}`
- [ ] Line 230 `expr_error_expected_got` 已接入，使用 `{expected}`/`{got}`
- [ ] Line 237 `expr_error_nested_too_deep` 已接入
- [ ] Line 357 `expr_error_func_lparen` 已接入
- [ ] Line 359 `expr_error_func_rparen` 已接入
- [ ] Line 367 `expr_error_missing_rparen` 已接入
- [ ] Line 372 `expr_error_unexpected_token` 已接入，使用 `{val}`
- [ ] Line 379 `expr_error_extra_content` 已接入
- [ ] Line 388 `expr_error_invalid_ast` 已接入
- [ ] Line 399 `expr_error_x_undefined` 已接入
- [ ] Line 403 `expr_error_unknown_variable` 已接入，使用 `{val}`
- [ ] Line 409 `expr_error_param_not_number` 已接入，使用 `{val}`
- [ ] Line 413 `expr_error_undefined_param` 已接入，使用 `{val}`
- [ ] Line 419 `expr_error_unknown_constant` 已接入，使用 `{val}`
- [ ] Line 425 `expr_error_unknown_unary_op` 已接入，使用 `{val}`
- [ ] Line 436 `expr_error_div_zero` 已接入
- [ ] Line 441 `expr_error_unknown_binary_op` 已接入，使用 `{val}`
- [ ] Line 449 `expr_error_unknown_function` 已接入，使用 `{val}`
- [ ] Line 455 `expr_error_unknown_ast_type` 已接入，使用 `{val}`
- [ ] Line 466 `expr_error_parse` 已接入
- [ ] 所有错误调用使用 `(typeof window !== 'undefined' && window.i18n && window.i18n.t(...)) || '中文fallback'` 模式
- [ ] 注释未改动

## function-3d.js
- [ ] Line 417 x 轴标签 `f3d_axis_label_x` 已接入
- [ ] Line 420 y 轴标签 `f3d_axis_label_y` 已接入
- [ ] Line 423 z 轴标签 `f3d_axis_label_z` 已接入
- [ ] 10 处错误消息已接入对应 `f3d_error_*` 键
- [ ] 带变量的错误使用 `{val}` 占位符
- [ ] 注释未改动

## pixel-clock.js
- [ ] WEEKDAY_CN 数组运行时改为 `i18n.t('clock_weekday_' + n)`
- [ ] 星期串拼接已 i18n 化
- [ ] 字体切换提示已 i18n 化
- [ ] monthNames 数组运行时改为 `i18n.t('clock_month_' + n)`
- [ ] `年 月` 拼接已 i18n 化
- [ ] 日历事件提示已 i18n 化
- [ ] `工作时间`/`休息时间` 已 i18n 化
- [ ] `已完成 N 个番茄` 已 i18n 化，使用 `{n}` 占位符
- [ ] `开始`/`暂停`/`重置` 按钮已 i18n 化
- [ ] 注释未改动

## chart.js
- [ ] 图例 `输入序列`/`融合预测`/`各方法预测` 已 i18n 化
- [ ] 图例 `拟合曲线` 已 i18n 化
- [ ] tooltip `索引:`/`融合预测:`/`预测:`/`训练预测:`/`值:` 已 i18n 化
- [ ] `方法权重 (降序)` 标题已 i18n 化
- [ ] 注释未改动

## nn-visualizer.js
- [ ] 图层标签 `输入层 (N)`/`输出层 (N)`/`隐藏层 K (N)` 已 i18n 化，使用 `{n}`/`{idx}`
- [ ] `轮次:`/`损失:` 标签已 i18n 化
- [ ] `损失曲线 (Loss Curve)` 标题已 i18n 化
- [ ] `等待训练开始...` 已 i18n 化
- [ ] 网络结构错误（至少 2 层 / 神经元 >= 1）已 i18n 化
- [ ] 注释未改动

## nn.js
- [ ] Lines 516-518 三个图层标签已 i18n 化，使用 `{n}`
- [ ] Line 532 `误差:` 标签已 i18n 化
- [ ] 训练阶段消息（`训练完成`/`步骤 X/Y` 等）保持原样不动
- [ ] 注释未改动

## offsetfit.js + app.js
- [ ] 11 个 functionName 字段均新增对应 functionNameKey 字段
- [ ] Line 1013 复制逻辑同步复制 functionNameKey
- [ ] `js/app.js` 第 1353 行使用 `i18n.t(best.functionNameKey)` 替代 `best.functionName`，保留 fallback
- [ ] 注释未改动

## overfit.js
- [ ] 新增 `METHOD_NAME_KEYS` 数组（3 个键）
- [ ] Line 441 显示处使用 `i18n.t(METHOD_NAME_KEYS[m])` + 中文 fallback
- [ ] 注释未改动

## physics-sandbox.js
- [ ] 新增 `ELEMENT_NAME_KEYS` 数组（3 个键）
- [ ] Line 442 显示处使用 `i18n.t(ELEMENT_NAME_KEYS[id])` + 中文 fallback
- [ ] 注释未改动

## image-pixelizer.js
- [ ] Line 269 `pixelizer_error_not_init` 已接入
- [ ] Line 273 `pixelizer_error_no_file` 已接入
- [ ] Line 294 `pixelizer_error_load_failed` 已接入
- [ ] 采用防御性 i18n 模式
- [ ] 注释未改动

## 语法与一致性验证
- [ ] `node -c js/expression-parser.js` 通过
- [ ] `node -c js/function-3d.js` 通过
- [ ] `node -c js/pixel-clock.js` 通过
- [ ] `node -c js/chart.js` 通过
- [ ] `node -c js/nn-visualizer.js` 通过
- [ ] `node -c js/nn.js` 通过
- [ ] `node -c js/offsetfit.js` 通过
- [ ] `node -c js/overfit.js` 通过
- [ ] `node -c js/physics-sandbox.js` 通过
- [ ] `node -c js/image-pixelizer.js` 通过
- [ ] `node -c js/app.js` 通过
- [ ] `node -c js/i18n.js` 通过
- [ ] i18n.js 中本次新增的所有键 zh/en 一一对应
- [ ] 所有 i18n.t 调用的键名在 i18n.js 中均已注册
- [ ] 功能保持不变（仅替换文本，不改逻辑）
