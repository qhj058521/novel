---
name: 背景提示词提取
description: 为仙侠/修真小说场景生成背景图像提示词。当用户想要基于小说分镜或场景描述为AI图像生成工具（Midjourney、Stable Diffusion等）创建场景描述时使用。输出高质量中文仙侠风格提示词，包含恰当的光照、氛围和构图细节。
---

# 仙侠小说背景提示词生成器

此技能为中文仙侠/修真小说生成高质量的背景图像提示词。所有提示词都指定干净的背景，不包含人物、文本或UI元素，适合AI图像生成工具。

## 何时使用此技能

**触发条件：**
- 用户提到为小说/漫画创建场景背景
- 用户想要用于Midjourney、Stable Diffusion或其他AI图像生成器的提示词
- 用户需要修真/仙侠场景的视觉描述
- 用户正在制作分镜或场景规划

**此技能的功能：**
- 分析场景信息（位置、时间、氛围）
- 生成详细的中文仙侠风格图像提示词
- 确保提示词指定：无人物、无文本、无UI元素
- 创建主场景和衍生变体（时间变化、天气变化、细节特写）
- 保持一致的修真/仙侠美学

## 输入格式

接受任何格式的场景信息。提取以下关键元素：

**必需：**
- `location`：场景位置（如"青云宗外门弟子房"、"主殿"、"练功场"）
- `time_of_day`：时间（如"清晨"、"黄昏"、"夜晚"、"正午"）

**可选但有帮助：**
- `visual_effects.environment`：环境细节（雾、山、天气）
- `visual_effects.lighting`：具体光照描述
- `visual_effects.color_palette`：色彩基调或氛围

输入示例：
```json
{
  "shot_id": "EP01-S001",
  "location": "青云宗外门弟子房",
  "time_of_day": "清晨",
  "visual_effects": {
    "environment": "窗外云雾缭绕,远处青山如黛",
    "lighting": "自然光从窗户斜射,柔和昏暗",
    "color_palette": "暖色调,晨光金"
  }
}
```

## 输出格式

按照以下结构生成提示词：

```
Chinese xianxia style, [场景描述], [光照], [色彩], [修真氛围], [质量修饰符], 无人物、无文本、无UI元素
```

### 提示词组成部分

**1. 风格前缀**（始终包含）
- `Chinese xianxia style` - 确立修真美学

**2. 场景描述**
- 室内：建筑细节、家具、房间特征
- 室外：自然环境、建筑、地形
- 特殊空间：神秘元素、阵法、屏障

**3. 光照**
根据一天中的时间匹配相应的光照关键词：

| 时间 | 光照关键词 | 色调 |
|------|-------------------|------------|
| 清晨 | morning sunlight, soft dawn light, golden sunrise | warm golden, soft |
| 上午 | natural morning light, bright daylight | bright, fresh |
| 正午 | bright noon sunlight, strong shadows, direct sunlight | high contrast |
| 下午 | afternoon light, natural illumination | warm |
| 黄昏 | sunset golden hour, warm orange glow, evening light | warm orange, golden |
| 夜晚 | moonlight, blue moonlight, starlight, lantern light | cool blue, mysterious |
| 雨天 | rain, grey sky, gloomy lighting, wet surface | grey, moody |
| 雷雨 | dark storm clouds, lightning, dramatic lighting | dramatic, dark |

**4. 修真关键词**（每个提示词使用2-3个）

*建筑：*
- cultivation sect, pavilion, altar, mountain gate, secret realm
- ancient temple, meditation hall, training ground, scripture pavilion

*自然：*
- sea of clouds, spiritual energy, ancient trees, mysterious mist
- mountain peaks, floating islands, spiritual spring, mystical forest

*效果：*
- magical formations, glowing runes, spiritual herbs, treasure light
- barrier, ethereal glow, mystical aura, cultivation atmosphere

*氛围：*
- ethereal, ancient, mysterious, solemn, peaceful, dangerous
- transcendent, otherworldly, mystical

**5. 质量修饰符**（始终包含）
- `high quality, detailed, 8k resolution`

**6. 排除项**（始终包含）
- `no people, no text, no UI elements, no characters, no writing, no symbols`

## 场景类型检测

根据位置关键词确定场景类型：

**室内**
关键词：房、殿、堂、阁、室、屋

描述示例：
- 简陋木屋：`humble wooden room in cultivation sect, simple wooden furniture, wooden bed, wooden table, paper window`
- 宏伟大殿：`grand main hall of cultivation sect, ornate pillars, stone floor, ceremonial altar, spiritual symbols`
- 修炼室：`cultivation training room, meditation cushions, wooden floor, incense burner, tranquil atmosphere`

**室外**
非室内、非特殊空间的默认类型

描述示例：
- 山门：`majestic mountain gate of cultivation sect, stone stairs, ancient architecture, mountain peaks in background`
- 庭院：`open courtyard plaza in cultivation sect, stone pavement, traditional buildings surrounding, open sky`
- 山区：`secluded mountain area behind sect, ancient trees, stone path, spiritual energy gathering spots`

**特殊空间**
关键词：秘境、异空间、结界、虚空

描述示例：
- `mystical space, [位置], glowing runes, magical barrier, spiritual energy, ethereal atmosphere`

## 生成主场景

1. **识别唯一组合**：每个（位置 + 时间）组合应生成一个唯一的主场景
2. **确定场景类型**：根据位置关键词归类为室内/室外/特殊
3. **构建描述**：选择适当的场景描述模板
4. **添加光照和色彩**：匹配时间
5. **包含修真元素**：添加2-3个相关的修真关键词
6. **应用质量和排除项**：始终附加质量修饰符和排除术语

## 生成子场景（衍生场景）

为每个主场景生成2-3个衍生变体：

**1. 时间变化**
在保持相同位置的同时改变一天中的时间：
- 夜晚版本（月光、蓝色调）
- 黄昏版本（日落、金色调）
- 雨天版本（灰色、忧郁氛围）

**2. 天气变化**
- 雨：添加雨水、湿滑表面、灰暗天空
- 雾：添加浓雾、能见度降低
- 雪：添加雪花、寒冷氛围、白色调

**3. 角度/细节变化**
- 特写：聚焦具体细节（木纹、石质纹理、建筑特征）
- 不同视角：广角与亲密视图

**衍生示例：**
*主场景：*"青云宗外门弟子房-清晨"
*子场景：*
- "青云宗外门弟子房-夜晚"（夜晚版本）
- "青云宗外门弟子房-细节特写"（家具细节特写）

## 提示词示例

### 室内场景 - 清晨
```
Chinese xianxia style, humble wooden room in cultivation sect, simple wooden furniture, wooden bed, wooden table, morning sunlight through paper window, misty mountains visible outside window, warm golden lighting, peaceful cultivation atmosphere, high quality, detailed, 8k resolution, no people, no text, no UI elements
```

### 室外场景 - 黄昏
```
Chinese xianxia style, majestic mountain gate of cultivation sect, stone stairs, ancient architecture, mountain peaks in background, sunset golden hour, warm orange glow, dramatic lighting, ethereal cultivation atmosphere, high quality, detailed, 8k resolution, no people, no text, no UI elements
```

### 特殊空间 - 夜晚
```
Chinese xianxia style, mystical secret realm, glowing magical formations, blue runes on stone floor, ethereal barrier, moonlight illuminating the space, spiritual energy gathering spots, mysterious cultivation atmosphere, high quality, detailed, 8k resolution, no people, no text, no UI elements
```

## 质量检查清单

生成提示词后，验证：

- [ ] 所有提示词包含"Chinese xianxia style"
- [ ] 所有提示词包含"no people, no text, no UI elements"
- [ ] 所有提示词包含质量修饰符："high quality, detailed, 8k resolution"
- [ ] 主场景是唯一的（没有重复的位置 + 时间组合）
- [ ] 每个主场景有2-3个子场景（衍生场景）
- [ ] 光照和色彩与指定的时间匹配
- [ ] 修真关键词使用得当
- [ ] 场景类型（室内/室外/特殊）正确识别
- [ ] 描述具体详细，不泛泛而谈

## 使用技巧

**用于小说插图：**
- 为常用位置生成主场景
- 为不同情绪创建时间/天气变体
- 使用特写强调重要细节

**用于AI动画/漫画：**
- 首先批量生成所有主场景
- 为不同情感节拍创建变体
- 在相似位置之间保持一致性

**用于场景规划：**
- 使用提示词在写作前可视化场景
- 生成多个变体以探索美学选项
- 为同一位置测试不同氛围

## 重要提示

1. **一致性**：对相关位置使用相似的术语和关键词
2. **无现代元素**：严格保持仙侠/修真美学（除非另有说明）
3. **质量优于数量**：最好有较少、精心制作的提示词，而不是许多通用的提示词
4. **具体细节**：包含具体的视觉元素（不仅仅是"美丽的风景"）
5. **强制排除**：始终明确排除人物、文本和UI元素

## 示例工作流程

**用户请求：**"为每一章的分镜生成背景提示词"

**响应：**
1. 读取分镜文件以提取场景
2. 识别所有唯一的（位置 + 时间）组合
3. 对于每个组合：
   - 确定场景类型（室内/室外/特殊）
   - 生成主场景提示词
   - 生成2-3个子场景变体
4. 输出格式为JSON或markdown，包含：
   - 场景ID
   - 场景名称
   - 位置类型
   - 一天中的时间
   - 主提示词
   - 子场景
   - 标签
   - 描述
5. 验证所有质量检查清单项通过

---

**记住：**目标是生成干净、有氛围的背景图像，捕捉中国仙侠修真世界的精髓。每个提示词都应将观看者带入一个神秘的、古代的武术和精神修炼领域。
