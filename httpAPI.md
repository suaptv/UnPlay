# Playback Event HTTP API Documentation

用于获取 **Unplay 播放器状态** 以及 **通过 HTTP 指令触发播放** 的接口文档。  
适用于投屏控制、播放调度、自动播放、二次开发等场景。

---

## ✨ 功能概述

当前提供两类 HTTP 接口能力：

1. **播放器状态查询**
   - 获取播放状态、进度、标题等信息
2. **播放器控制**
   - 通过 POST 请求直接触发视频播放
   - 支持单集播放与播放列表模式

---

## 🌐 播放器状态查询接口（GET）

### 接口地址


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


## ▶️ 播放控制接口（POST）

通过 **HTTP POST 请求**，向播放器发送 **播放视频指令**。

---

### 🌐 接口地址
http://{UNPLAY_IP}:9030/

---

### 📤 请求方式

- **Method**：`POST`
- **Content-Type**：`application/x-www-form-urlencoded`

---

### 📦 POST 参数说明

| 参数名 | 必填 | 说明 |
|------|------|------|
| `urls` | ✅ | 播放资源列表（URL 编码） |
| `option` | ❌ | 播放器类型 |
| `mode` | ❌ | 播放模式 |

---

### 🧾 urls 参数说明（重点）

- `urls` **必须进行 URL 编码**
- 内容为 **每行一条播放资源**
- 每一行格式如下：

标题$播放地址
#### 示例（编码前）
第20集$http://192.168.0.1/test.mp4
#### 示例（URL 编码后）
%E7%AC%AC20%E9%9B%86%24http%3A%2F%2F192.168.0.1%2Ftest.mp4


📌 **多条视频时**

- 使用 **换行符** 分隔多条资源
- 然后对 **整体内容进行 URL 编码**

---

### ⚙️ option 参数（播放器类型）

| 值 | 说明 |
|---|---|
| `native` | 使用 **原生播放器**（默认推荐） |
| `ffmpeg` | 使用 **FFmpeg 播放器** |

---

### 🎬 mode 参数（播放模式）

| 值 | 说明 |
|---|---|
| `none` | 只播放一集 |
| `playList` | 播放列表模式 |

---

### 📌 完整请求示例

```http
POST http://192.168.0.111:9030/
Content-Type: application/x-www-form-urlencoded

urls=%E7%AC%AC20%E9%9B%86%24https%3A%2F%2Fcdn.yzzy32-play.com%2F20260205%2F16484_4fffd1f6%2Findex.m3u8&option=native&mode=playList



