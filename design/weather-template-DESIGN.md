# Weather App UI Design — 像素级蒸馏规范

> Source: Figma Community file `1100826294536456295` · 作者 Aksonvady · 2.8k likes · 101k users
> Tags: dark mode / glassmorphism / neumorphism / ios / weather app
> 参考图: `design/weather-ref/cover.png` (3840x1920)、`carousel-1.jpg`（同尺寸）、`carousel-2.jpg`
> 用途: 雅思工作台重皮的唯一视觉事实源。所有视觉决策以本文档为准，冲突时以参考图为准。

---

## 1. Visual Theme

深夜紫罗兰「玻璃拟态 × 黏土拟物」混合风格：

- 深蓝紫夜空做底，缀细星点，营造"夜晚氛围层"
- 内容装在磨砂玻璃浮层里，像隔着起雾的窗看灯光
- 3D 黏土质感插画做情绪点缀（软、圆润、有光泽）
- 主操作按钮用轻新拟物（凸起），次级导航做内凹，凹凸成对出现
- 全局唯一光源：**左上方 45°**。一切高光在左上，投影在右下，无一例外

一句话气质：安静、柔软、贵气，像深夜里发光的玻璃温室。

## 2. Color Palette

| 角色 | 值 | 说明 |
|---|---|---|
| 夜空背景 | `#1E2235 → #2B2F4A` 垂直渐变 | 最深只到这，禁用纯黑 #000 |
| 星点噪声 | 白 6%~12% 透明度细点 | CSS radial-gradient 平铺 |
| 环境光渐变 | `#C9A0EE → #8F6BE0`（135°），局部粉调 `#E3B8E8` | 页面级氛围光斑，低频大面积 |
| 玻璃卡片基色 | `rgba(108,91,212,0.55)` ~ `0.65` | 主容器卡 |
| 玻璃小卡 | 同色相，透明度降到 `0.35~0.45` | 小时胶囊/chip，比容器更透 |
| 玻璃高光边 | 左上 `rgba(255,255,255,0.35~0.50)`，底部压到 `≤0.10` | 1~1.5px 内描边，模拟玻璃厚度 |
| 主文字 | `#FFFFFF` | 大数字、标题 |
| 次文字 | 白 `80~90%` | 说明性信息 |
| 语义点缀 | 太阳黄 `#F5C842` · 雨滴青 `#3EC6E8` · 樱粉 `#E8A2BC` · 暖窗 `#F0C060` | 只用于状态/插画，禁止当装饰色撒 |
| 深符号色 | `#3D3470` | 凸起按钮上的图标/文字 |

纪律：紫色系是绝对主调（≥60% 面积），白色文字，语义点缀合计 ≤10%。第六种颜色即失控。

## 3. Typography

- 字体族：SF Pro Display / -apple-system / Inter 回退链
- 英雄数字（温度位）：**Thin~Light (200–300)**，72–88pt，字距 `-1% ~ -2%`，纯白
- 度数符号：数字高度的 ~55%，上标位置
- 辅助信息：Regular 13–14pt，白 85%
- 层级策略：**字号对比拉到极限**（88 vs 13），不靠颜色数量做层级

## 4. Component Stylings

### 玻璃容器卡（复盘卡/计划卡）
```css
background: rgba(108,91,212,0.55);
backdrop-filter: blur(28px);
border-radius: 30px;
border: 1px solid rgba(255,255,255,0.12);
box-shadow:
  inset 1px 1px 0 rgba(255,255,255,0.35),   /* 左上玻璃厚度 */
  0 12px 32px rgba(20,16,60,0.35);          /* 环境投影 */
padding: 16px 18px;
```

### 胶囊 chip（材料直达/小时预报格）
全圆角 22–24px，填充透明度 0.35–0.45，blur 24px，同款内高光。

### 凸起主按钮（新拟物 FAB）
直径 56–64px，圆形：
```css
background: linear-gradient(180deg,#8A7AE0,#6C5BD4);
box-shadow:
  inset 1px 1px 2px rgba(255,255,255,0.5),   /* 左上内高光 */
  -3px -3px 8px rgba(255,255,255,0.15),      /* 左上外反光 */
  0 6px 12px rgba(30,25,70,0.35);            /* 右下外投影 */
```
按钮上符号用深紫 `#3D3470`。按下时投影收缩 + scale(0.96)，模拟物理按压。

### 内凹态（次级导航图标）
与凸起相反：`inset 2px 2px 4px rgba(20,16,60,0.4), inset -1px -1px 2px rgba(255,255,255,0.25)`。
激活态 = 凹陷；未激活 = 平。凸（FAB）与凹（tab）必须成组存在才有拟物逻辑。

### 图标
全部 Lucide 内联 SVG，24×24，stroke-width 2，round cap/join。**零 emoji**。

## 5. Layout

- 基准宽 375pt（我们 430px 列），左右安全边距 16–20px
- 8pt 基线网格；卡片间距 14–16px；组件尺寸取 8 的倍数
- 上部 ~40% 高度给"英雄锚点"（大数字倒计时），下部 ~45% 是玻璃浮层堆叠
- 玻璃层允许与背景插画重叠 ~15%；个别小元素故意溢出卡片边界 20–40px 制造景深
- 正负空间 6:4：信息紧凑，氛围留白宽松

## 6. Depth & Elevation

三层预算封顶：
1. z0 氛围层：夜空 + 星点 + 紫色光斑（不可交互）
2. z1 玻璃容器：blur 浮层
3. z2 内容元素：胶囊、按钮、溢出的小图形

阴影全站最多两层规格（环境投影 + 内高光），多一层即打回。

## 7. Do's & Don'ts

**Do**
- 光源方向全局一致（左上 45°）
- 大数字敢给字号，Thin 字重配大尺寸才显贵
- 玻璃叠玻璃时，下层降透明度区分层级
- 激活态即时切换 + 入场交错动画（40–60ms 步进）

**Don't**
- 不用纯黑、不用纯灰平面底色
- 功能色不做装饰
- emoji 一律替换为 Lucide SVG
- 动效不抢戏：单次入场 ≤400ms，弹簧过冲 ≤1.05
- 不给玻璃卡加彩色描边或彩虹渐变边（廉价感来源）

## 8. Responsive

- 手机优先单列 430px；桌面居中同列，两侧留给氛围层呼吸
- 桌面端氛围光斑可放大偏移，内容列宽度不变
- 触控目标 ≥44px

## 9. Agent Prompt Guide（生成提示词）

> 按此 prompt 描述风格去生成/实现任何界面：

**EN**
> A dark-mode mobile app UI in violet glassmorphism style: deep indigo-night background (#1E2235→#2B2F4A) with faint stars and soft purple ambient glows (#C9A0EE→#8F6BE0, 135°). Content sits on frosted glass panels — fill rgba(108,91,212,0.55), backdrop-blur 28px, 30px rounded corners, 1px inner top-left highlight rgba(255,255,255,0.35). One hero metric rendered in ultra-thin (weight 200) pure-white type at 80pt+. Primary action is a neumorphic circular FAB (56px, gradient #8A7AE0→#6C5BD4, dual-light soft shadows, light source fixed top-left 45°); secondary nav icons are inset/recessed. Pill-shaped translucent chips (radius 22px, 40% opacity) for secondary items. All icons are 24px stroke-2 line icons (Lucide). 8pt spacing grid, generous negative space, clay-soft 3D accents used sparingly in yellow #F5C842 / cyan #3EC6E8 / pink #E8A2BC only for semantic highlights. Calm, premium, quiet-luxury mood; no pure black, no rainbow gradients, shadows capped at two layers.

**CN**
> 深夜紫罗兰玻璃拟态移动端界面：靛蓝夜空底（#1E2235→#2B2F4A）缀微星点与柔紫光斑；内容承载于磨砂玻璃卡（填充 rgba(108,91,212,0.55)、blur 28px、圆角 30px、左上 1px 白色内高光）；核心指标用 200 字重超大纯白数字（约 80pt）；主操作为 56px 新拟物圆形凸起按钮（渐变 #8A7AE0→#6C5BD4、双光源柔影）；次级导航为内凹态；辅助信息用全圆角半透明胶囊 chip。图标一律 24px 双描边线性 SVG。8pt 栅格、留白慷慨；黄/青/樱粉仅作语义点缀且合计 ≤10% 面积。气质：安静、柔软、贵气。禁纯黑、禁彩虹渐变、阴影至多两层、光源统一左上 45°。

## 10. 映射到雅思工作台

| 模板元素 | 工作台对应 |
|---|---|
| 19° 温度大数字 | 考试倒计时天数（Thin 80pt 白） |
| Hourly Forecast 玻璃面板 | 今日任务列表容器 |
| Weekly Forecast 面板 | 本周打卡七格子 + 近7天记录 |
| 城市搜索页玻璃卡 | 计划/统计页卡片 |
| 凸起 "+" FAB | 生成打卡文案主按钮 |
| 底部凹凸导航 | Today/计划/统计/设置 四 tab（激活=凹陷） |
| 3D 黏土小屋/云 | CSS 渐变光斑 + 柔和立体感图形替代，不用位图 |
