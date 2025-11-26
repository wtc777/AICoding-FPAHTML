

------

你是一个资深前端工程师，请帮我实现一个 **网页交互式摆牌界面**，用于性格卡牌选择与排序。

## 一、技术要求

1. 使用 **原生 HTML + CSS + JavaScript**，不依赖构建工具（不用 React/Vue 等）。
2. 单文件 `index.html` 即可运行（直接用浏览器打开就能看到效果）。
3. 代码结构清晰，尽量模块化：
   - HTML：语义化结构
   - CSS：写在 `` 标签中
   - JS：写在 `` 标签中，使用立即执行函数或模块化结构避免全局变量污染

------

## 二、卡牌数据结构

页面中使用固定的 12 张卡牌数据，数据结构如下（请直接在 JS 中内联一份常量，用这个结构）：

```js
const cards = [
	{
		id: "card_01",
		front: {
			title: "悲观",
			english: "Frustrated",
			value: 3,
			color: "blue"
		},
		back: {
			title: "乐观",
			english: "Optimistic",
			value: 1,
			color: "red"
		}
	},
	{
		id: "card_02",
		front: {
			title: "他人认可最重要",
			english: "Humans are the most important",
			value: 2,
			color: "red"
		},
		back: {
			title: "事情结果最重要",
			english": "Result is the most important",
			value: 2,
			color: "yellow"
		}
	},
	{
		id: "card_03",
		front: {
			title: "主动帮助他人",
			english": "Always trying to help others",
			value: 2,
			color: "red"
		},
		back: {
			title: "静待问题过去",
			english": "Waiting for things to go away",
			value: 2,
			color: "green"
		}
	},
	{
		id: "card_04",
		front: {
			title: "条理",
			english": "Organized",
			value: 1,
			color: "blue"
		},
		back: {
			title: "随意",
			english": "Random",
			value: 3,
			color: "red"
		}
	},
	{
		id: "card_05",
		front: {
			title: "以他人为中心",
			english": "Others-centered",
			value: 1,
			color: "green"
		},
		back: {
			title: "以自我为中心",
			english": "Self-centered",
			value: 3,
			color: "yellow"
		}
	},
	{
		id: "card_06",
		front: {
			title: "越挫越勇",
			english": "What doesn't kill one makes one stronger",
			value: 1,
			color: "yellow"
		},
		back: {
			title: "逆来顺受",
			english": "Conservative and hold back",
			value: 3,
			color: "green"
		}
	},
	{
		id: "card_07",
		front: {
			title: "目标坚定",
			english": "Determined",
			value: 1,
			color: "yellow"
		},
		back: {
			title: "缺乏主见",
			english": "Hold back",
			value: 3,
			color: "green"
		}
	},
	{
		id: "card_08",
		front: {
			title: "批判性强",
			english": "Critical",
			value: 3,
			color: "yellow"
		},
		back: {
			title: "平和宽容",
			english": "Peaceful and tolerant",
			value: 1,
			color: "green"
		}
	},
	{
		id: "card_09",
		front: {
			title: "发现问题先研究",
			english": "Study first when there is a problem",
			value: 2,
			color: "blue"
		},
		back: {
			title: "发现问题先解决",
			english": "Act immediately once there is a problem",
			value: 2,
			color: "yellow"
		}
	},
	{
		id: "card_10",
		front: {
			title: "情绪化",
			english": "Emotional",
			value: 3,
			color: "red"
		},
		back: {
			title: "自律",
			english": "Self-discipline",
			value: 1,
			color: "blue"
		}
	},
	{
		id: "card_11",
		front: {
			title": "内心保守",
			english": "Conservative",
			value: 3,
			color": "blue"
		},
		back: {
			title": "乐于分享",
			english": "Enjoy sharing",
			value: 1,
			color": "red"
		}
	},
	{
		id": "card_12",
		front: {
			title": "相安无事最重要",
			english": "Waiting for things to go away",
			value: 2,
			color": "green"
		},
		back: {
			title": "坚持原则最重要",
			english": "Sticking to the principles is the most important",
			value": 2,
			color": "blue"
		}
	}
]
```

> 注意：如果上面 JSON 有引号的小错误，请你先自动修正为合法 JS，再使用。

------

## 三、页面布局需求

页面主要分为三个区域：

1. **顶部：摆牌区域（目标布局区）**
   - 一个 **4 x 3 的网格**，共 12 个“空位格子”。
   - 每个格子要有边框（例如虚线灰框），在没有牌时显示 “空位” 或简单的占位提示。
   - 当有卡牌放入某个格子时，展示卡牌内容（选定的那一面：正面或反面）。
2. **中部：操作提示区（简单文字说明即可）**
   - 一段说明文字，如：“步骤：1）从下方卡组选择卡牌 2）选择正面/反面 3）点击一个空位放置 4）拖拽上方的牌进行排序”。
3. **底部：卡组区域（原始牌堆）**
   - 展示所有 12 张卡牌的一个“缩略列表”（不区分正反面，展示一个简化版本，例如只显示 `id` + `front.title`）。
   - 每张卡牌显示为一个小卡片，可点击（不是从这里拖拽，只是选择要使用的牌）。

布局风格要求：

- 使用 `flex` 或 `grid` 布局实现自适应。
- 桌面端浏览器观看体验良好即可（不用过度适配移动端）。

------

## 四、交互逻辑要求

### 1. 选牌 & 选正反面

交互流程如下：

1. 用户在 **底部卡组** 区域点击一张卡牌（例如 `card_03`）。
2. 弹出一个 **对话框 / 悬浮面板（Modal）**，内容包括：
   - 标题：选择牌面
   - 两个选项：
     - 选项 A：显示 `front` 的内容（标题 + 英文 + 颜色 + value）
     - 选项 B：显示 `back` 的内容（标题 + 英文 + 颜色 + value）
   - 用户点击其中一个选项确认。
3. 选择完正反面之后，**这个“已配置的具体牌面”进入“待放置状态”**：
   - 在界面上可以有一个小提示，比如顶部显示：“当前待放置卡牌：XXX（正面/反面）”。

### 2. 放牌到上方空位

1. 在有“待放置卡牌”时，用户点击 **上方 4x3 网格中的某个空格子**：
   - 如果格子为空，则将当前待放置的卡牌放进去。
   - 放进去后，该格子显示这张卡牌的内容（选定面）。
   - 放置后，“待放置卡牌”状态被清空。
2. 若用户点击的是一个 **已有卡牌的格子**：
   - 此次点击不做处理（或者提示“此位置已有卡牌，可拖拽调整顺序”）。

> 注意：同一张卡牌 `id` 理论上只允许使用一次即可，可以在放置后将该卡从“底部卡组”标记为已使用（例如变灰或标记 “已放置”）。

### 3. 牌面显示样式

在上方 4x3 网格中，每一张已放置的牌需要显示以下信息：

- 中文标题：`title`
- 英文标题：`english`
- 数值：`value`
- 颜色：根据 `color` 决定卡片背景色或边框色：
  - red
  - blue
  - yellow
  - green

简单配色即可，比如不同颜色浅色背景 + 深色文字。

### 4. 拖拽排序（重要）

上方 4x3 区域的牌需要支持 **拖拽排序**：

1. 用户可以用鼠标拖拽某个有卡牌的格子，将它拖到另一个格子：
   - 若目标格子已有卡牌，则两张牌交换位置。
   - 若目标格子为空，则牌移动到该空格，原位置变为空。
2. 建议使用原生 HTML5 DnD 或 `pointerdown + pointermove` 自己实现都可以：
   - 不依赖外部库。
3. 拖拽时有简单的视觉反馈：
   - 被拖拽卡牌有轻微阴影或透明度变化。
   - 目标格子在拖拽悬停时高亮边框。

### 5. 重置与导出（简单按钮）

在页面某处（例如顶部右侧）添加两个按钮：

1. **重置按钮**
   - 功能：清空上方 4x3 网格中所有卡牌，恢复为空位状态。
   - 同时将所有底部卡牌恢复为“未使用”状态。
2. **导出当前布局按钮**
   - 点击后，在控制台 `console.log` 输出当前布局的 JSON：
     - 一个长度为 12 的数组，对应 4x3 网格中的位置（按行优先从左到右），每个元素要么是 `null`，要么是一个对象：
       - 包含：`cardId`, `side`（"front" or "back"）, `title`, `english`, `value`, `color`, `slotIndex`
   - 不需要做下载文件，只要 `console.log` 出来即可。

------

## 五、代码结构建议（请你帮我自行合理拆分）

1. 初始化：

   - 渲染底部卡组列表。

   - 初始化 12 个空位格子的数据结构，比如：

     ```js
     const slots = Array(12).fill(null)
     ```

2. 状态管理：

   - `currentSelectedCard`：当前在底部选中的卡（card 对象）。
   - `currentPlacedCard`：当前已选正/反面且待放置的卡片（含 side, cardId, title 等）。
   - `slots`：表示上方 12 个位置当前的卡牌状态。

3. DOM 更新：

   - 封装函数 `renderSlots()` 与 `renderDeck()`，每次状态改变时刷新视图。

4. 拖拽：

   - 每个 slot DOM 元素设置拖拽属性和事件处理函数：
     - `dragstart`
     - `dragover`
     - `drop`
   - 在 JS 中写清楚 slot 之间互换逻辑。

------

## 六、UI 风格要求

1. 整体风格简洁干净，适合之后接入正式 UI 设计。
2. 卡牌建议用统一尺寸，例如宽 160px、高 100px 左右，圆角 + 阴影。
3. 颜色采用简单背景色即可，比如：
   - red: `#fde2e2`
   - blue: `#e0f0ff`
   - yellow: `#fff9db`
   - green: `#e5f9e7`
4. 字体使用系统默认字体即可。

------

请根据上述完整需求，编写一个可直接运行的 `index.html` 文件示例，包含完整的 HTML、CSS、JS 代码，并保证：

- 打开浏览器即可看到 4x3 摆牌区域 + 底部卡组。
- 可以按“选卡牌 → 选正/反面 → 点击上方空位放置 → 拖拽排序”的流程完整操作。
- 点击“导出当前布局”按钮时在控制台输出当前状态 JSON。

------

你可以直接把我刚才这段提示交给 Codex，用它生成完整前端代码。