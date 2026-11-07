# FVG量均线（FVG + Volume Profile + Moving Averages）

中文 | [English](#english)

一个 TradingView 三合一指标（Pine Script v6）：**成交量分布 + 双均线系统 + 多周期 FVG（公允价值缺口）**，外加一个右上角「背景提示」面板。设计理念：**只画地图，不喊买卖**——把结构（哪里成交最密、哪里是真空、趋势背景如何）一眼呈现，开仓信号留给使用者自己的体系。

**双版本**：仓库里有两份代码，逻辑完全一致，只有界面语言不同——`FVG量均线.pine`（中文界面）与 `FVG_Volume_MA_EN.pine`（英文界面），按习惯选一份即可。

<!-- 在这里放一张指标截图会更好：把图表截图命名为 screenshot.png 传到仓库，然后把下面一行开头的注释符号删掉即可
![指标效果](screenshot.png)
-->

## 功能

### 1. 成交量分布（Volume Profile）
- 按价位分行统计成交量，涨 K / 跌 K 分色绘制，固定画在最后一根 K 线右侧的空白区，不遮挡 K 线
- 标出 **POC**（最大成交价位，粗橙线向左横贯回看区间）、**VAH / VAL**（价值区上下沿，默认覆盖 70% 成交量）
- 统计窗口 = 从最后一根 K 往前数的固定根数（默认 200 根），不是随缩放变化的可见范围
- **真空带（LVN）自动标灰**：某行成交量低于 POC 行的 20%（可调）算稀疏行，柱子自动变灰；连续 ≥3 行（可调）成段即真空带，背景面板会报出最近的上方 / 下方真空带价位段

### 2. 双均线系统
- EMA（指数移动平均）与 MA（简单移动平均）各 20 / 60 / 120 / 200，共 8 条，默认全开，可按组关闭
- 柔和护眼配色，同周期同色、EMA 粗实 / MA 细淡

### 3. 多周期 FVG（公允价值缺口）
- 1 小时 / 4 小时 / 日线三个周期常开，当前图表周期另有开关（默认关）
- 三根 K 判定，只用已完成 K 线、**不重绘**；向上缺口 = 下方支撑带（绿框），向下缺口 = 上方压力带（红框）
- 缺口被价格完全回补后自动删除；每个周期保留数量、向右延长长度可调；日线带只画虚线框、不填充

### 4. 背景提示面板（右上角）
机械统计三项打分，给出当前结构背景，**不是买卖信号**：
- 位置：现价在价值区上方 / 区内 / 下方（±1）
- 长期：现价 vs EMA200（±1）
- 短期：EMA20 vs EMA60 排列（±1）
- 合计 ≥ +2 = 偏多背景，≤ −2 = 偏空背景，其余为中性震荡 / 过渡未定型；并列出 POC 相对现价的位置、最近的 FVG 压力 / 支撑带

## 安装（30 秒）

1. 打开 [TradingView](https://www.tradingview.com/) 图表，底部打开 **Pine 编辑器**
2. 新建一个空白脚本，全选删掉默认内容
3. 把所选版本 `.pine` 文件的全部代码粘贴进去
4. 点「添加到图表」即可

> TradingView 免费版现在限制云端保存脚本的数量；代码保存在本地文件或本仓库里，需要时粘贴使用即可，不依赖云端保存。

## 主要参数

| 参数 | 默认 | 说明 |
|---|---|---|
| 回看K线数 | 200 | 成交量分布的统计窗口 |
| 分布行数 | 30 | 价格分行数 |
| 价值区占比 | 70% | VAH / VAL 覆盖的成交量比例 |
| 成交量分布距最后一根K线 | 2 根 | 分布图与 K 线的间隔 |
| 真空行判定 | 低于 POC 行的 20% | 低于此比例的行算稀疏行并标灰 |
| 成段真空最少连续行数 | 3 | 连续稀疏行成段才报为真空带 |
| 每个周期最多保留 FVG 数 | 5 | 1H / 4H / 日线 / 当前周期各自计数 |
| FVG 向右延长 | 10 根 | 缺口带向右延伸长度 |

## 口径与局限（重要）

- **成交量分布是估算值**：Pine 拿不到逐笔成交，本指标把每根 K 的成交量在其高低区间内均分，与 TradingView 付费版 VPVR 的逐笔口径有出入。形状与密集区位置可信，单行数值仅作相对参考。
- 品种没有成交量数据时，用每根 K 记 1 代替（仅看相对形状）。
- FVG 用已完成 K 线判定，不重绘；高周期缺口在小周期图上是「区域提示」，不是精确入场位。
- 背景提示是**机械统计的环境描述**，不预测涨跌、不构成任何交易建议。

## 参与改进

欢迎帮忙修这个指标，两种方式：

- **提 Issue（问题反馈）**：发现 bug（错误）或有改进建议，在仓库的 Issues（问题）页新建一条，最好附上品种、周期和截图，方便复现。
- **提 Pull Request（合并请求）**：Fork（复刻）本仓库到你自己的账号，在你的复刻里改好 `FVG量均线.pine`，再向本仓库发起 Pull Request，简单说明改了什么、为什么。作者会逐个审核，合适就合并进主版本。

提交贡献即表示同意以本仓库的 MIT 许可证授权你的贡献。

## 免责声明

本指标仅供学习与技术分析参考，不构成投资建议。交易有风险，盈亏自负。

## 许可证

[MIT License](LICENSE) © QINGFANG

---

## English

A 3-in-1 TradingView indicator (Pine Script v6): **Volume Profile + dual moving averages + multi-timeframe FVG (Fair Value Gaps)**, plus a top-right market-background panel. Design principle: **it draws the map, it does not call trades** — structure (where volume is concentrated, where the vacuum zones are, what the trend background looks like) at a glance; entry signals stay with your own system.

**Two versions**: the repo ships the same code twice with identical logic — `FVG_Volume_MA_EN.pine` (English UI) and `FVG量均线.pine` (Chinese UI). Pick the one you prefer.

### Features

**1. Volume Profile**
- Volume-by-price rows, split by up/down bars, drawn in the empty space to the right of the last bar — never covering your candles
- **POC** (Point of Control, thick orange line extending left across the lookback window) and **VAH / VAL** (value-area high/low, 70% of volume by default)
- Fixed lookback window counted back from the last bar (default 200 bars), not the visible chart range
- **Vacuum bands (LVN) auto-grayed**: a row below 20% of the POC row's volume (adjustable) counts as sparse and turns gray; runs of ≥3 sparse rows (adjustable) form a vacuum band, and the background panel reports the nearest vacuum band above / below price

**2. Dual moving-average system**
- EMA and SMA at 20 / 60 / 120 / 200 (8 lines), all on by default, can be toggled per group
- Muted, eye-friendly colors; same color per period, EMA bold / SMA faint

**3. Multi-timeframe FVG**
- 1H / 4H / Daily always on; the chart timeframe has its own switch (default off)
- Classic 3-candle gaps, judged on **closed candles only — no repainting**; bullish gap = support zone below (green), bearish gap = resistance zone above (red)
- Zones are removed once fully filled; per-timeframe count and right-extension are adjustable; daily zones are dashed outlines with no fill

**4. Background panel (top right)**
A mechanical 3-factor score of the current structural background — **context only, not a trading signal**:
- Position: price above / inside / below the value area (±1)
- Long term: price vs EMA 200 (±1)
- Short term: EMA 20 vs EMA 60 alignment (±1)
- Total ≥ +2 = bullish background, ≤ −2 = bearish, otherwise neutral range / transition; also shows the POC's distance from price and the nearest FVG resistance / support zones

### Install (30 seconds)

1. Open a [TradingView](https://www.tradingview.com/) chart and open the **Pine Editor** at the bottom
2. Create a new blank script and delete the default content
3. Paste the full content of the `.pine` file for your chosen version
4. Click **Add to chart**

> TradingView's free plan now limits how many scripts you can save in the cloud. Keep the code as a local file or in this repo and paste it when needed.

### Main parameters

| Parameter | Default | Notes |
|---|---|---|
| Lookback bars | 200 | Volume Profile window |
| Profile rows | 30 | Price rows |
| Value area % | 70% | Volume covered by VAH / VAL |
| Profile gap from last bar | 2 bars | Space between candles and the profile |
| Vacuum row threshold | 20% of the POC row | Rows below this share are sparse and grayed |
| Min rows for a vacuum band | 3 | Consecutive sparse rows needed to form a band |
| Max FVG zones per timeframe | 5 | Counted separately for 1H / 4H / Daily / chart TF |
| FVG right extension | 10 bars | How far zones extend to the right |

### Methodology & limitations (important)

- **The volume profile is an estimate**: Pine has no tick data, so each bar's volume is spread evenly across its high–low range. This differs from TradingView's paid tick-based VPVR. The shape and the congestion zones are reliable; individual row values are relative approximations.
- Symbols without volume data use 1 per bar (relative shape only).
- FVG zones are judged on closed candles and never repaint; a higher-timeframe zone on a small-timeframe chart is an area hint, not a precise entry level.
- The background panel is a **mechanical description of context**. It does not predict direction and is not investment advice.

### Contributing

Issues and Pull Requests are welcome:
- **Issues**: bug reports and suggestions — please include the symbol, timeframe, and a screenshot so the problem can be reproduced.
- **Pull Requests**: fork the repo, edit the `.pine` files in your fork (please keep the Chinese and English versions in sync), and open a PR explaining what you changed and why. The author reviews each one and merges what fits.

By contributing, you agree to license your contribution under this repo's MIT License.

### Disclaimer

For study and technical-analysis reference only. Not investment advice. Trading involves risk; all gains and losses are your own.

### License

[MIT License](LICENSE) © QINGFANG
