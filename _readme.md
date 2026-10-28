# MidiShow downloader 

> 一个deepseek写的 Tampermonkey 脚本，帮助您一键下载 MidiShow 上的 MIDI 文件。  
> 支持自动跳转下载页、自动下载、进度条状态提示，并提供菜单手动触发。

---

## ✨ 功能特性

- **自动跳转**：当访问 `/midi/download` 下载页时，自动跳转到对应的详情页，并保留下载意图。
- **自动下载**：跳转后自动开始下载 MIDI 文件，无需额外操作。
- **进度与状态提示**：在右下角显示简约的悬浮面板，实时反馈下载进度（0% ~ 100%）和文字状态。
- **菜单手动触发**：通过 Tampermonkey 菜单（右键图标）可随时手动下载当前页面的 MIDI。
- **轻量精简**：无多余样式和动画，仅保留核心功能，兼容性强。

---

## 📥 安装方法

1. 确保浏览器已安装 **Tampermonkey** 扩展（[Chrome](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo) / [Firefox](https://addons.mozilla.org/firefox/addon/tampermonkey/)）。
2. 点击 Tampermonkey 图标 → “创建新脚本”。
3. 将本脚本的全部代码粘贴进去，保存（Ctrl+S）。
4. 脚本会自动生效（匹配 `*://www.midishow.com/*`）。

---

## 🚀 使用方法

### 自动模式（推荐）
- 直接访问任意 MIDI 详情页（如 `https://www.midishow.com/midi/123456.html`）：
  - 若页面是下载页（`/midi/download`），会**自动跳转**到详情页并触发下载。
  - 若已是详情页，脚本会检查是否有自动下载标记，有则立即开始下载。
- 下载过程中，右下角会显示进度条和状态文字，完成后自动消失。

### 手动模式（通过菜单）
- 在任何 MIDI 详情页，**右键** Tampermonkey 图标。
- 在菜单中选择 **“下载当前 MIDI”**。
- 脚本会立即开始下载，同样显示进度与状态。

---

## 🖥️ 界面预览

> 右下角悬浮面板（简约风格）：
> ```
> ┌───────────────────┐
> │  请求第一段数据...  │  ← 状态文字
> │  ████████░░░░ 50%  │  ← 进度条
> └───────────────────┘
> ```

---

## ⚙️ 脚本配置

| 配置项 | 说明 |
|--------|------|
| **自动跳转** | 默认开启，访问下载页时自动跳转并标记下载 |
| **自动下载** | 跳转后自动执行，无需干预 |
| **菜单命令** | 固定包含“下载当前 MIDI”和“显示当前 MIDI ID”（后者可快速查看 ID） |
| **通知** | 下载成功/失败会弹出系统通知（GM_notification） |

> 如需调整 UI 位置或颜色，可修改脚本中 `createUI` 函数内的 CSS 字符串。

---

## 🧩 依赖与权限

| 权限 | 用途 |
|------|------|
| `GM_xmlhttpRequest` | 跨域请求 MIDI 数据 |
| `GM_registerMenuCommand` | 注册 Tampermonkey 菜单 |
| `GM_notification` | 系统通知反馈 |
| `connect` | 允许连接 `www.midishow.com` 和 `s.midishow.net` |

---

## 🛠️ 技术实现

- 基于原 MidiShow 的 `etagDecode` 和 `de` 解码算法。
- 分两段请求加密数据，重组并验证 MIDI 头部（`MThd`）。
- 使用 `Blob` + `URL.createObjectURL` 触发浏览器下载。
- 利用 `sessionStorage` 标记自动下载状态，避免重复触发。

---

## ❓ 常见问题

**Q：为什么点击菜单后没有反应？**  
A：请确保当前页面是 MIDI 详情页（URL 包含 `/midi/数字.html`），脚本会提取 ID。若不是，会提示“未找到 MIDI ID”。

**Q：下载进度卡在某个百分比？**  
A：通常是网络问题或网站接口返回异常。可打开浏览器控制台查看详细日志（前缀 `[MidiDownload]`），或稍后重试。

**Q：自动下载没有触发？**  
A：检查是否从下载页跳转而来，或者浏览器是否禁用了 `sessionStorage`。可手动使用菜单触发。

**Q：下载的文件无法播放？**  
A：可能是解密环节出错，请检查控制台是否有报错信息，并确认 MIDI 文件头部为 `MThd`。

---

## 📄 许可

本项目仅供学习与个人使用，请遵守 MidiShow 网站的相关服务条款。  
代码基于 MIT 协议开源（如适用）。

---

## 📝 更新日志

### v2.0 (2026-07-04)
- 重构为自动+菜单双模式
- 添加简约进度条和状态面板
- 移除全部美化样式，仅保留核心功能
- 增加 `GM_notification` 反馈

### v1.0 (初始版本)
- 基础下载功能（菜单触发）

---

**Enjoy!** 🎵
