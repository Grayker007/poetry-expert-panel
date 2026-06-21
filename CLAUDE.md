# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# 每周AI家庭分享

家庭AI分享机制，达哥、静静、可心三人轮值，每月3场，周末进行。

## 项目结构与产物

这是一个**内容项目**（家庭AI分享 + 古诗文亲子共读），不是代码库——没有构建/测试/lint 命令，产物是 markdown 记录和 HTML 页面。工作流的每一步都对应根目录下按分享人区分的文件，挑文件先认准当期分享人那一份：

- `tracker.md` — **进度真相源**：当月状态表 + 往期记录。每完成一场分享必须更新这里。
- `ideas/{达哥,静静,可心}-选题池.md` — 三人各自的选题候选池（对应工作流第1步「选题准备」）。
- `templates/{达哥,静静,可心}-模板.md` — 三人各自的会后记录模板（对应工作流第3步「会后记录」）。
- `sessions/YYYY-MM/` — 每场家庭分享的记录与素材（如第1期《如梦令》AI实验）。
- `可心初中古诗文学习项目/` — **独立子项目**（独立 git 仓库，已在 `.gitignore` 忽略），承载122篇古诗文专家团，详见其自带 `CLAUDE.md`。

## 家庭成员

| 成员 | AI水平 | 常用工具 | 角色定位 |
|------|--------|----------|----------|
| 达哥 | 高级 | Claude Code、OpenClaw、GPT、Gemini | 未来引路人 |
| 静静 | 中级 | Gemini（辅助可心作业） | 生活实践者 |
| 可心 | 入门（会简单对话） | Gemini、GPT | 小小探索者 |

可心 2016-08-23 出生，四年级，正在备战小升初。兴趣：航模、足球、哈利波特。

## 轮值规则

- 第1个周末 → 达哥
- 第2个周末 → 静静
- 第3个周末 → 可心
- 第4个周末 → 自由周（安全阀）
- 不强制固定日期，当月完成3场即可
- 每场15-20分钟含讨论

## 工作流程（3步）

### 1. 选题准备

根据当期分享人，走对应选题路径：

**达哥选题**：
1. 从工作版AI分享中提取"家庭可迁移"话题
2. AI HOT 教育/生活/创意类筛选（`category=ai-products` + `q=教育/孩子/学习`）
3. Exa 补充搜索儿童AI教育、适龄工具

**静静选题**：
1. 她用 Gemini 的真实经验
2. 从可心笔记中发现"AI能帮上忙"的场景
3. 生活中的AI使用体验

**可心选题**：
1. 她与AI的互动体验（对话、画画、写故事）
2. 达哥从可心笔记中提取学习亮点，转化为"AI也能帮你做这个"
3. 同学/学校里关于AI的见闻

### 2. 辅助准备

Claude Code 按需灵活切换：
- 辅助达哥（轻量）：选题筛选、桥接转译
- 辅助静静（教练）：选题启发、内容梳理
- 辅助可心（伙伴）：挖亮点、引导整理想法

**核心原则**：永远是帮手不是代替者。不能让AI替可心做分享内容，只能帮她整理自己的想法。

### 3. 会后记录

分享完成后，用对应模板在 `sessions/YYYY-MM/` 下创建记录文件。

## 数据源

### Get笔记-可心账号（原始素材）

可心有独立的 Get笔记账号，API key 配置在 OpenClaw：
`~/.openclaw/openclaw.json` → `skills.entries.getnote_kexin`

Claude Code 中访问：
```bash
KEXIN_KEY=$(python3 -c "import json; d=json.load(open('$HOME/.openclaw/openclaw.json')); print(d['skills']['entries']['getnote_kexin']['apiKey'])")
KEXIN_CLIENT=$(python3 -c "import json; d=json.load(open('$HOME/.openclaw/openclaw.json')); print(d['skills']['entries']['getnote_kexin']['env']['GETNOTE_CLIENT_ID'])")

# 笔记列表
curl -s "https://openapi.biji.com/open/api/v1/resource/note/list?since_id=0" \
  -H "Authorization: $KEXIN_KEY" -H "X-Client-ID: $KEXIN_CLIENT"

# 笔记详情（含 audio.original 录音转文字）
curl -s "https://openapi.biji.com/open/api/v1/resource/note/detail?id={note_id}" \
  -H "Authorization: $KEXIN_KEY" -H "X-Client-ID: $KEXIN_CLIENT"
```

内容类型：recorder_audio（课堂录音）、class_audio（课外直播）、plain_text（整理笔记）、link（收藏链接）。

### Obsidian 可心笔记（叙事视角）

路径：`/Users/grayker/Library/Mobile Documents/iCloud~md~obsidian/Documents/Obsidian Vault/日常沉淀/智理台/可心笔记/2026/`

从 Get笔记录音生成的每日观察和周回顾。选题时先看这里获得叙事概览，再去 Get笔记查原始录音细节。

### AI HOT

用 aihot skill，重点关注教育/创意/生活类。达哥选题时使用。

### Exa

补充搜索儿童AI教育、适龄创意工具。后置使用。

## 内容方法论（贯穿所有讨论）

每次讨论分享选题时，自然嵌入以下框架——不是额外步骤，是讨论本身的逻辑：

### 三步法
1. **获取信息**：多元化输入。这个话题除了本领域，还能从哪些跨领域素材获得弹药？（美第奇效应：创新在交叉点）
2. **找角度（权重60%）**：反差=情理之中，预料之外。第一直觉想到的不能用。先想所有人会怎么切，然后不讲那个。做**陌生化**检查——不是给新信息，是让人重新看见熟悉的东西。
3. **创作（节奏）**：情绪曲线有波峰波谷。三个模型——故事弧线（Vonnegut）、升番（喜剧三番：小→大→意外）、英雄之旅（千面英雄）。

### 六条内核
1. 故事是人脑的协作协议，不是修辞技巧
2. 价值=是否经过了你这个主体的加工（搬运≠创造）
3. 新意发生在交叉点，不在领域内部
4. 最高级的价值不是新信息，是重新看见（陌生化）
5. 你设计的不是内容，是一段注意力与情绪的旅程
6. 长期主义里，不做什么比做什么更决定走多远

### 品味即护城河
- AI吞工序，选择能力成为真护城河
- 品味=被压缩的判断力=庞大参照库+一个能站住的观点
- **传品味>有品味**：给引擎，别给结论
- 对可心：环境+示范+不评判的提问（坑：强加→逆反→没主见）
- 家庭AI分享本身就是在实践"传品味"

### 讨论时的四个自然检查点
1. 获取信息：除了AI领域，还有什么跨领域弹药？
2. 找角度：第一直觉会怎么切？我们不讲那个，讲什么？
3. 节奏：情绪曲线怎么走？高峰在哪？
4. 复盘：这次的品味判断是什么？哪里是选择、哪里是工序？

完整知识体系在 Obsidian `/知识沉淀/知识库/方法/` 下（内容创造的内核、节奏设计、品味即护城河）。

## 与工作版的区别

- 不做HTML全屏演示，只用markdown简单记录
- 不走6步标准化流程，3步轻量即可
- 不限于国内工具，可用 Gemini、GPT、Claude Code、OpenClaw
- 重点在讨论不在讲课
- 保持"发现"和"好玩"的基调，不是考试或作业

## 古诗文专家团（已拆分为独立子项目）

古诗文专家团（122篇逐篇多角度亲子共读，七人工作流 + `poetry-expert-panel` skill）原是本项目衍生出来的内容，现已**独立成子项目**：

➡️ **`./可心初中古诗文学习项目/`**（独立 git 仓库，被本仓库 `.gitignore` 忽略）

要做古诗文相关工作，进那个目录、看它自己的 `CLAUDE.md` 与 `README.md`。本项目这边只保留《如梦令》家庭分享第1期的素材（`sessions/2026-06/260601_55_如梦令/` 的 `01-*` 文件 + `历史文档/` 的 AI 实验视频）——它们是家庭分享产物，不随专家团迁走。

## 避坑清单

- 不要跳过讨论环节——讨论比演示更重要
- 不要给可心推荐复杂工具，从对话和创意入手
- 不要月月满负荷——第4周是安全阀
- 不要把工作版的正式感带进来，家庭版要轻松
- Claude Code 读可心笔记要用她的独立 key，不是达哥的 getnote CLI
