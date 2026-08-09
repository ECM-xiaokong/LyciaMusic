# 🛠️ 修复日志 — Codex 改进 (模型: DeepSeek v4 Flash)

## 修复内容

### 1. BitLocker 锁定驱动器刷新残留歌曲修复
- **问题**: 锁定 BitLocker 驱动器后刷新本地音乐，锁定驱动器内的歌曲仍然保留在列表中
- **根因**: library.rs 中两处 DELETE FROM songs 的 SQL LIKE 语句使用了错误的路径转义
- **修复**: 使用现有的 escape_like() 工具函数替代手动转义
- **涉及文件**: src-tauri/src/music/library.rs

### 2. 中文标签乱码修复
- **问题**: 某些歌曲 ID3v2 标签编码被错误标记为 Latin-1，导致中文/日文显示为乱码
- **修复**: 增加 decode_garbled_latin1 函数，支持 UTF-8/GBK/Shift-JIS 回退解码
- **涉及文件**: src-tauri/src/music/tags.rs

### 3. 无法播放歌曲自动跳过
- **问题**: 锁定驱动器中的歌曲无法播放时播放器卡住
- **修复**: 播放失败后 1 秒自动跳转到下一首，对所有播放模式有效
- **涉及文件**: src/composables/playerPlayback.ts

### 4. load_cached_songs 路径匹配修复
- **修复**: 将 format!(\"{locked_path}\\\"\") 改为 format!(\"{locked_path}\\\\\")，修复反斜杠被替换为双引号的 bug

## 关联 Issues: #83, #84, #85
