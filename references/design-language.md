# 设计语言规范（Design Language）

克兰因蓝蓝白极简体系。以下 token 可直接写进 CSS `:root`。黄金样板见 `assets/StyleWork-proposal-example.html`。

## 1. 色彩 Token

```css
:root{
  --blue:#002FA7;        /* 克兰因蓝：主色、强调、图表主色 */
  --blue-deep:#00237d;    /* hover/按压 */
  --blue-ink:#001a5e;     /* 深蓝文字 */
  --blue-soft:#eef1fc;    /* 浅蓝底：图标底、选中态 */
  --blue-softer:#f5f7fe;  /* 更浅蓝：统计卡、引用块 */
  --blue-line:#d6ddf7;    /* 蓝描边 */
  --ink:#14181f;          /* 主文字 */
  --ink-2:#454c5a;        /* 次文字 */
  --ink-3:#8a91a0;        /* 弱文字/说明 */
  --line:#e7e9ef;         /* 描边 */
  --line-2:#eef0f5;       /* 更浅描边/分隔 */
  --bg:#ffffff;
  --bg-soft:#f6f7f9;      /* 分区底、表头 */
  /* 状态色（克制使用） */
  --green:#0f7b5c;  --green-soft:#e7f4ef;
  --amber:#9a6a00;  --amber-soft:#faf1dc;
  --red:#b3322c;    --red-soft:#fbeae9;
}
```

**配色原则**
- 单一强调色（蓝），状态色仅用于标签/警示，不进主视觉。
- 背景以白为主，用 `--bg-soft` / `--blue-softer` 做分区与呼吸，不靠深色块。
- 图表配色（同色系派生，不用 ECharts 默认色轮）：
  `#002FA7, #3d5fd0, #7a93e0, #b8c6f0`；漏斗可用 `#002FA7→#a9b9ec` 渐变。
- 扩色用 oklch 沿同一 hue 调 lightness/chroma，不凭空发明 hex。

## 2. 字体

```css
--font:-apple-system,BlinkMacSystemFont,"SF Pro Text","PingFang SC","Helvetica Neue","Segoe UI",sans-serif;
```

- 一个字体家族贯穿全篇，不堆字体、不用 Inter/Roboto（被用滥）。
- 中文正文 ~15.5px，行高 1.7；英文小标用 `letter-spacing:.18–.26em` 拉开。
- 层级靠字号/字重/行长建立：Hero 48–52px（800），章节 32px（700），子标题 19px（700），卡片标题 15.5px（700）。
- 数字用 `font-variant-numeric:tabular-nums` 对齐。
- 行长控制在 ~40 个全角字内。

## 3. 间距 / 圆角 / 阴影

```css
--radius:12px;  --radius-sm:9px;
--shadow-1:0 1px 2px rgba(20,24,31,.05);
--shadow-2:0 1px 2px rgba(20,24,31,.04),0 10px 28px rgba(0,47,167,.07);
--shadow-hover:0 2px 4px rgba(20,24,31,.05),0 16px 40px rgba(0,47,167,.12);
```

- 章节纵向留白 80–88px；卡片内边距 24px；卡片间距 18px。
- 圆角统一 10–12px（卡片/输入），小元素 8–9px。
- 阴影极淡、偏蓝灰；hover 才加深，不做厚重投影。

## 4. 组件规范

| 组件 | 规范 |
|---|---|
| **侧边导航** | 固定 248px，浅底 #fbfcfe，右 1px 描边；logo 30px 圆角方块；链接 8px 圆角，选中为实心蓝 |
| **章节头** | 英文 kicker（蓝字、字距 .18em、带 26px 短横线）+ 大标题 + 超大章节编号（描边/淡蓝 54px） |
| **卡片** | 白底、1px 描边、12px 圆角、淡阴影；图标 38px 浅蓝底方块；hover 上浮 3px、描边变蓝 |
| **统计卡** | 白→浅蓝渐变底，数值 32px 蓝、标签 12.5px |
| **表格** | 卡片化包裹（圆角+描边）；表头浅灰底 12px；行 hover 浅蓝；`table-layout:fixed` + 显式 colgroup |
| **状态标签** | 胶囊 chip（20px 圆角），前置 6px 圆点：dead 红 / half 琥珀 / empty 绿 / ours 蓝 |
| **引用/Insight** | 浅蓝底 + 蓝描边，左侧 30px 实心蓝标记（!），不做圆角+左边框的老套容器 |
| **按钮** | 主按钮实心蓝（带淡蓝投影）；次按钮白底描边；hover 有位移；圆角 10px |
| **报价卡** | 16px 圆角；推荐档蓝描边 + 顶部悬浮徽章；价格 40px 蓝；清单前置勾形 Line 图标 |

## 5. 图标语言

- 统一内联 SVG，`viewBox="0 0 24 24"`，`stroke-width="1.8"`，`stroke-linecap/linejoin="round"`，`fill="none"`。
- 选中/勾选可用实心勾；状态用 6px 圆点。
- 禁止 Emoji；图标尺寸：导航 26px、卡片 20px、行内 16–17px。

## 6. Hero / Signature

- 满屏（min-height 100vh），极淡网格底纹 + 角落径向浅蓝（用 mask 收敛，不抢内容）。
- 结构：客户/范围/周期/版本 meta → 英文 kicker → 大标题（关键词可着蓝）→ 中英 slogan → **流程动效线** → CTA。
- 流程动效线：5–6 个圆角小卡（图标+标题+说明），卡片间箭头；加载时依次 fade-up（stagger .15s）。
- 底部 SCROLL 鼠标提示（移动端隐藏）。

## 7. 质感与红线

- 极淡网格/径向仅用于 Hero 与分隔，装饰层独立绝对定位且 `aria-hidden`。
- 红线：滥用渐变、毛玻璃、glow 发光边框、圆角+左边框、3D 图表、饼图 >5 片、双 Y 轴、词云、彩虹色板、data-aos 全站铺、密集粒子。
