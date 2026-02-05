# Playback Event HTTP API Documentation

用于获取 **Unplay 播放器当前播放状态** 的 HTTP 接口文档。  
适合用于投屏联动、远程控制、播放状态同步、二次开发等场景。

---

## ✨ 功能概述

该接口用于 **实时获取播放器当前播放信息**，包括：

- 播放状态（播放 / 暂停 / 停止）
- 当前播放时间
- 当前视频总时长
- 当前播放的媒体标题
- 播放列表位置与总数量

接口返回 **JSON 格式数据**，便于前端、服务端或自动化程序解析。

---

## 🌐 接口地址

http://{UNPLAY_IP}:9030/PlaybackEvent

### 参数说明

| 参数名 | 说明 |
|------|------|
| `UNPLAY_IP` | Unplay 软件运行设备的 IP 地址 |

📌 **IP 获取方式**  
在 **Unplay 软件设置界面** 中查看「HTTP 投屏 IP」。

示例：
http://192.168.0.111:9030/PlaybackEvent


---

## 📥 返回数据示例

```json
{
  "total_duration": 936.233361,
  "current_time": 5.188000000000001,
  "playback_status": "PLAYING",
  "title": "第19集",
  "current_index": 1,
  "total_videos": 3
}

```

| 字段名               | 类型     | 说明               |
| ----------------- | ------ | ---------------- |
| `total_duration`  | Number | 当前视频的 **总时长（秒）** |
| `current_time`    | Number | 当前播放时间（秒）        |
| `playback_status` | String | 播放器状态            |
| `title`           | String | 当前媒体标题           |
| `current_index`   | Number | 当前播放在列表中的序号      |
| `total_videos`    | Number | 播放列表中的总视频数量      |

| 状态值               | 含义    |
| ----------------- | ----- |
| `PLAYING`         | 正在播放  |
| `PAUSED_PLAYBACK` | 播放暂停  |
| `STOPPED`         | 播放已停止 |


🧠 使用场景示例
📺 投屏状态同步
🎮 远程控制播放器
📱 手机 / 平板显示播放进度
🧩 智能家居或自动化系统联动
🧪 播放器状态监控与调试
