# SVGArena — 大模型 SVG 信息图能力评测

> 通过统一的提示词让各大模型生成 SVG 信息图，自动核验指令遵循度与渲染质量，以静态画廊形式呈现各模型的 SVG 能力差异。

**在线画廊**：[https://earlzhang.github.io/SVGArena](https://earlzhang.github.io/SVGArena)

## 评测方法

每个测试项包含一套提示词和若干份内容素材。模型按提示词生成 SVG 信息图，每个模型生成 5 次（temperature 0.8），然后自动核验两层指标：

### A 指令遵循度（最多 4 项各 1 分）

| 指标 | 判定条件 |
|---|---|
| 纯净输出 | 响应恰好以 `<svg` 开头、`</svg>` 结尾，无围栏/前言/后记 |
| 比例合规 | viewBox 高宽比在 2.10~2.20 范围内（iPhone 全面屏） |
| 字体合规 | font-family 不含非 macOS 内置中文字体 |
| 浅色底 | PNG 边框区域亮度中位数 > 0.75 |

### B 渲染质量（4 项各 1 分）

| 指标 | 判定条件 |
|---|---|
| 无溢出 | 所有文字元素包围盒不超出 viewBox 边界 |
| 无重叠 | 文字元素两两包围盒重叠面积不超过较小元素的 15% |
| 无遮挡 | 文字未被图形元素遮挡（像素级差分检测） |
| 非空白 | 墨迹率（非背景像素占比）在 2%~80% 之间 |

**总分 = 启用的 A 层数 + 4**。前提门槛：SVG 合法且渲染成功，否则 0 分。

## 提示词

当前测试项的提示词见 [`tasks/iphone_infographic/prompt.md`](tasks/iphone_infographic/prompt.md)。

约束配置见 [`tasks/iphone_infographic/task.yaml`](tasks/iphone_infographic/task.yaml)。

## 测试内容

| 内容 | 来源 |
|---|---|
| [英伟达服务器涨价](tasks/iphone_infographic/contents/nvidia_server_price_hike.md) | [财联社](https://www.cls.cn/detail/2461501) |

## 渲染环境

- 浏览器：Playwright Chromium（与网页展示同引擎）
- 字体：macOS 内置中文字体（不注入自定义字体）
- 截图：deviceScaleFactor=2

## 免责声明

- 评测结果仅反映模型在特定提示词和特定内容下的 SVG 生成能力，不代表模型综合实力
- 模型会静默发版，不同日期的评测结果可能对应不同版本
- 评测时间见画廊页面各模型的标注
