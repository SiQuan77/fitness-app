# 健身记录 Web 应用 — SPEC.md

## 1. Concept & Vision

一款为健身爱好者设计的移动端优先记录工具，强调**快速记录**和**数据沉淀到个人知识库**。界面风格参考高端运动手表界面，深色主题，圆润的卡片设计，给人专业、专注的感觉。核心体验是"滑一下、点一下、记完了"。

## 2. Design Language

### Aesthetic Direction
高端运动手表风格 + 极简主义。深色背景，霓虹色点缀，圆润卡片，微妙的玻璃拟态效果。

### Color Palette
```
--bg-primary:     #0D0D0F      (主背景，黑偏蓝)
--bg-card:        #1A1A1E      (卡片背景)
--bg-card-hover:  #222226      (卡片悬停)
--accent-primary: #00D4AA      (主色调，青绿色)
--accent-warm:    #FF6B35      (警告/热量，橙红)
--accent-blue:    #4A9EFF      (信息/训练，蓝)
--accent-purple:  #A855F7      (补剂，紫色)
--text-primary:   #FFFFFF
--text-secondary: #8B8B94
--text-muted:     #4A4A52
--border:         #2A2A2E
--success:        #22C55E
```

### Typography
- 主字体：`"SF Pro Display", -apple-system, "PingFang SC", "Noto Sans SC", sans-serif`
- 数字字体：`"SF Mono", "JetBrains Mono", monospace`（用于重量、次数等）
- 字号：基础 16px，卡片标题 20px，数字显示 32px

### Spatial System
- 卡片圆角：16px
- 按钮圆角：12px
- 页面边距：16px
- 卡片间距：12px
- 底部安全区：env(safe-area-inset-bottom)

### Motion Philosophy
- 页面切换：slide-in 200ms ease-out
- 卡片点击：scale(0.98) + 轻微震动反馈
- 数字输入：数字滚动动画
- 成功反馈：绿色脉冲 + 轻微放大

## 3. Layout & Structure

### 页面结构
底部 Tab 导航（4个）：记录 / 计划 / 日历 / 我的

#### 3.1 记录页（默认首页）
- 顶部：今日日期 + 星期 + 天气图标
- 今日概览卡片（补剂计数 + 训练计数）
- 快速补剂按钮组（预设：肌酸、蛋白粉、复合维生素、鱼油、氮泵、锌镁片）
- 快速训练按钮组（预设动作分类：胸部、背部、腿部、肩部、手臂、核心）
- 底部浮动「+」按钮，点击展开完整记录表单

#### 3.2 计划页
- 分部位训练计划列表（胸、背、腿、肩、手臂、核心）
- 每周训练安排日历视图
- 当前计划的下一个动作提示

#### 3.3 日历页
- 月历视图，当前日期高亮
- 有记录的日子用彩色圆点标记（补剂=紫，训练=蓝）
- 点击日期查看当天详情

#### 3.4 我的页面
- GitHub 同步状态
- 知识库文件路径显示
- 统计数据（本周补剂次数、训练次数、最常记录的动作）
- 设置入口（GitHub Token、知识库路径）
- 关于

### 响应式策略
- 移动端优先（320px - 480px 基准）
- 平板（768px+）侧边栏导航代替底部 Tab
- 最大宽度 480px 居中显示

## 4. Features & Interactions

### 4.1 补剂记录
**快速记录**：点击预设按钮 → 直接添加一条记录
**自定义记录**：点击「+」→「补剂」tab → 输入名称/剂量 → 保存

记录格式：
```
- 时间 | 补剂名称 | 剂量
- 08:30 | 肌酸 | 5g
```

### 4.2 训练记录
**快速记录**：点击部位按钮 → 显示该部位常见动作 → 选择动作 → 填写重量/组数/次数
**自定义记录**：点击「+」→「训练」tab → 输入动作名/重量/组数/次数 → 保存

记录格式：
```
- 时间 | 动作名称 | 重量 | 组数 | 次数
- 19:00 | 深蹲 | 100kg | 5 | 5
```

### 4.3 训练计划
- 预设模板：5x5 力量计划、PPL 分化、简单三分化
- 用户可自定义每日训练内容
- 点击「开始训练」→ 逐步引导完成每个动作
- 每个动作完成后可标记「完成/跳过」

### 4.4 日历查看
- 月历浏览
- 点击有记录日期：弹出当天记录详情
- 支持补剂/训练分别筛选

### 4.5 知识库同步
- GitHub API 直连读写 `knowledge-base` 仓库
- 文件结构：
  ```
  健身/日志/补剂_YYYY-MM-DD.md
  健身/日志/训练_YYYY-MM-DD.md
  健身/训练计划/{部位}.md
  ```
- 首次使用需要配置 GitHub Personal Access Token
- 支持手动同步 + 自动同步（每次记录后）

### 4.6 离线支持
- localStorage 缓存所有记录
- 网络恢复后自动同步到 GitHub
- 离线时显示「离线模式」提示

## 5. Component Inventory

### 5.1 BottomNav（底部导航）
- 4个 Tab 项，图标 + 文字
- 当前页面高亮（accent-primary 颜色）
- 点击切换页面，slide 动画
- 状态：default, active, disabled

### 5.2 QuickAddButton（快速添加按钮）
- 圆形/药丸形按钮
- 点击：scale(0.95) + 轻微震动
- 按住：显示编辑选项
- 状态：default, pressed, disabled

### 5.3 RecordCard（记录卡片）
- 日期/时间 + 类型图标 + 内容摘要
- 背景：bg-card，hover 变 bg-card-hover
- 左滑显示删除按钮
- 状态：default, hover, swiped, deleting

### 5.4 NumberInput（数字输入）
- 超大数字显示（用于重量、次数）
- 左右箭头调整数字
- 长按快速增减
- 支持直接点击数字键盘输入
- 状态：default, focused, adjusting

### 5.5 PlanCard（计划卡片）
- 部位图标 + 名称 + 动作数量
- 当前计划高亮边框
- 点击展开详细动作列表
- 状态：default, active, completed

### 5.6 CalendarDay（日历日期）
- 圆形背景，日期数字居中
- 有记录时底部小圆点标记
- 今日：accent-primary 边框
- 选中：accent-primary 填充
- 状态：default, has_records, today, selected, other_month

### 5.7 SyncStatus（同步状态）
- 图标 + 文字
- 状态：synced（绿色勾），syncing（旋转），offline（灰色），error（红色感叹号）

### 5.8 Modal（弹窗）
- 半透明黑色遮罩
- 白色圆角卡片从底部滑入
- 点击遮罩或底部取消按钮关闭
- 动效：slide-up 250ms ease-out

### 5.9 Toast（轻提示）
- 底部居中弹出
- 成功：绿色背景 + 勾号
- 错误：红色背景 + 叉号
- 3秒自动消失，slide-up/out 动画

## 6. Technical Approach

### 架构
- **单文件 HTML**（便于部署，可直接用 file:// 打开或部署到任意静态托管）
- **原生 JavaScript**（无框架依赖，体积小，加载快）
- **GitHub API**（直接读写知识库 markdown 文件）
- **localStorage**（本地缓存 + 离线支持）

### 数据模型

```javascript
// 补剂记录
SupplementRecord {
  id: string,          // UUID
  name: string,        // 补剂名称
  dosage: string,      // 剂量（如 "5g", "2粒"）
  time: string,        // HH:mm 格式
  date: string,        // YYYY-MM-DD
  createdAt: number    // timestamp
}

// 训练记录
TrainingRecord {
  id: string,
  exercise: string,    // 动作名称
  weight: number,      // 重量（kg）
  sets: number,        // 组数
  reps: number,        // 每组次数
  rpe: number | null,  // RPE 1-10，可选
  date: string,
  time: string,
  createdAt: number
}

// 训练计划
Plan {
  id: string,
  name: string,        // 计划名称（如 "胸部 A"）
  muscleGroup: string, // 肌肉部位
  exercises: Exercise[],
  createdAt: number
}

Exercise {
  name: string,
  targetSets: number,
  targetReps: string,  // 如 "8-12" 或 "5"
  restSeconds: number
}
```

### GitHub API 集成

```javascript
// 写入文件
PUT /repos/{owner}/{repo}/contents/{path}

// 读取文件
GET /repos/{owner}/{repo}/contents/{path}

// 需要配置
const CONFIG = {
  owner: 'SiQuan77',
  repo: 'knowledge-base',
  token: '' // GitHub PAT
}
```

### 文件格式

**补剂_YYYY-MM-DD.md**:
```markdown
# 补剂记录 — YYYY-MM-DD

| 时间 | 补剂 | 剂量 |
|------|------|------|
| 08:30 | 肌酸 | 5g |
| 12:00 | 复合维生素 | 1粒 |

<!-- sync: 2024-01-15T10:30:00Z -->
```

**训练_YYYY-MM-DD.md**:
```markdown
# 训练记录 — YYYY-MM-DD

| 时间 | 动作 | 重量 | 组 | 次数 | RPE |
|------|------|------|-----|------|-----|
| 19:00 | 深蹲 | 100kg | 5 | 5 | 8 |

<!-- sync: 2024-01-15T10:30:00Z -->
```

### 关键实现细节

1. **触摸手势**：使用 touchstart/touchend 计算滑动距离，左滑 > 50px 显示删除
2. **数字输入**：监听 pointer 事件，支持触摸和鼠标，长按持续增减
3. **页面导航**：使用 hash routing（#record, #plan, #calendar, #profile）
4. **动画**：CSS transitions + requestAnimationFrame
5. **离线检测**：navigator.onLine + online/offline 事件
6. **GitHub API 限流**：缓存 last-sync 时间，分散请求

### 部署方式
1. 直接打开 HTML 文件（file://）
2. 部署到 GitHub Pages
3. 部署到 Cloudflare Pages / Vercel
4. 内网部署到本地服务器

### 浏览器兼容
- iOS Safari 14+
- Android Chrome 90+
- 微信内置浏览器
- 不支持 IE
