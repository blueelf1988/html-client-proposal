# 交互特效规范（Interaction Spec）

原生 JS 实现，不用框架、不用构建。以下为可直接套用的模式；完整实现见 `assets/StyleWork-proposal-example.html`。

## 1. 固定侧边导航 + Scrollspy

- 桌面 `position:fixed` 宽 248px；移动端改抽屉。
- scrollspy：监听 scroll，遍历 section，取最后一个 `offsetTop - 140 <= scrollY` 的 id，给对应链接加 `.active`。
- 移动端：汉堡按钮 + 抽屉（`transform:translateX`）+ 遮罩；点链接或遮罩关闭。
- **关闭时必须 `visibility:hidden`**（配合 transition 延迟），否则抽屉在视口外仍计入 `scrollWidth` 造成水平溢出。

## 2. 阅读进度条 / 回到顶部

- 顶部 3px 蓝条：`width = scrollY / (scrollHeight - innerHeight) * 100%`。
- 回到顶部：滚动 >600px 淡入，点击 `scrollTo({top:0,behavior:'smooth'})`。

## 3. 滚动入场（IntersectionObserver）

```js
const io = new IntersectionObserver((es)=>es.forEach(e=>{
  if(e.isIntersecting){ e.target.classList.add('in'); io.unobserve(e.target); }
}),{threshold:.12, rootMargin:'0px 0px -40px 0px'});
document.querySelectorAll('.rv').forEach(el=>io.observe(el));
```

- `.rv{opacity:0;transform:translateY(22px)}`，`.in` 过渡到常态（.7s cubic-bezier(.22,.8,.3,1)）。
- **单次触发、幅度小**；非每个元素都做，避免 AI 感。
- 必须有 `prefers-reduced-motion` 降级（直接显现）。

## 4. Tab（通用）

- 容器 `.tabs` 内 `.tab-btn[data-tab=key]`；同级 `.tab-panel[data-panel=key]`。
- 点按钮：切换 active，面板按 key 显隐；面板进入用小 fade-up。
- 适用：三类决策人、周/月/季 SOP、报价/方案对比。

## 5. 手风琴 Accordion（通用）

- `.acc-item` 内 `.acc-head` + `.acc-body`；用 `max-height = scrollHeight` 过渡（不用固定高度）。
- 同一手风琴内默认互斥（开一个关其余）。
- 头部：编号/占比小方块 + 标题 + 旋转箭头。
- 适用：栏目详情、合同条款、FAQ、待确认事项。

## 6. 折叠表格 / 横向滚动

- 「展开全部」：表格卡片加 `.open`，额外行 `.extra-row` 显隐；按钮箭头旋转。
- 移动端宽表格：`.t-scroll{overflow-x:auto}` + `table{min-width:560–600px}`，让它横向滚动而非压扁。
- 所有表格 `table-layout:fixed` + `<colgroup>` 显式列宽合计 100%。

## 7. 筛选 / 切换

- 增值表：行带 `data-cat`，筛选按钮按类别切换 `display`；按钮 active 态实心蓝。
- 报价档位：可做双卡片静态对比，或切换器联动（切换时改产出与价格）。

## 8. ECharts 图表

- 环形图：`radius:['52%','74%']`，`borderRadius:4`、白色 3px 间隔；标签 `{d}%`，图例 circle 置底。
- 漏斗图：`minSize:'30%'`，`gap:3`，标签 inside 白字；无真实数据时用递减值并标注"结构示意"。
- 条形（FTE/产能）：进入视口再动画（IntersectionObserver 触发 width 或 setOption）。
- 每个图表 `window.resize` 时 `chart.resize()`；tooltip 加 `confine:true`。
- 移动端图表容器必须随父 grid 堆叠为 1fr（避免内联 `grid-template-columns` 覆盖媒体查询）。

## 9. 微交互

- 卡片 hover：`translateY(-3px)` + 阴影加深 + 描边变蓝。
- 按钮 hover：主按钮上移 1px、加深；次按钮描边变蓝。
- **禁止僵尸按钮**：每个 `<button>`/`<a>` 必须有真实 handler、跳转或 toast 反馈；无目标的统一给 toast。
- 提供 `toast(msg)` 工具（底部居中、自动消失）。

## 10. 无障碍 / 性能

- `prefers-reduced-motion`：关闭动画与平滑滚动。
- 键盘焦点可见；图标按钮带 `aria-label`；装饰层 `aria-hidden`。
- 正文 ≥14px（输入 ≥16px）；viewport 不锁缩放。
- 图表/图片懒触发，避免一次性大量动画；CDN 统一 jsDelivr（备用 ccdnjs）。

## 11. 自检（强制）

用 html skill 的截图脚本：

```bash
python3 <html-skill>/scripts/shot.py <file.html>          # 桌面+移动
python3 <html-skill>/scripts/shot.py <file.html> --only mobile
```

重点核对：
- 移动端 `horizontalOverflow`（侧边栏/内联 grid 是高发根因）；
- `responsiveChartIssues`（图表是否堆叠成全宽）；
- `squeezedTableColumns`（列宽/ min-width）；
- 桌面逐区块确认图表非空白、无文字墙。
