# Playback Event HTTP API 文档

用于获取 **Unplay 播放器状态** 以及 **通过 HTTP 指令触发播放** 的接口文档。  
适用于投屏控制、播放调度、自动播放、二次开发等场景。

---

**目录**
1. 播放器状态查询（GET）
2. 播放控制（POST）

---

## 1. 播放器状态查询（GET）

**接口地址**  
`http://{UNPLAY_IP}:9030/PlaybackEvent`

**参数说明**

| 参数名 | 说明 |
| --- | --- |
| `UNPLAY_IP` | Unplay 软件运行设备的 IP 地址 |

**IP 获取方式**  
在 **Unplay 软件设置界面** 中查看「HTTP 投屏 IP」。

**示例**  
`http://192.168.0.111:9030/PlaybackEvent`

**返回数据示例**

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

**字段说明**

| 字段名 | 类型 | 说明 |
| --- | --- | --- |
| `total_duration` | Number | 当前视频的总时长（秒） |
| `current_time` | Number | 当前播放时间（秒） |
| `playback_status` | String | 播放器状态 |
| `title` | String | 当前媒体标题 |
| `current_index` | Number | 当前播放在列表中的序号 |
| `total_videos` | Number | 播放列表中的总视频数量 |

**播放状态值**

| 状态值 | 含义 |
| --- | --- |
| `PLAYING` | 正在播放 |
| `PAUSED_PLAYBACK` | 播放暂停 |
| `STOPPED` | 播放已停止 |

---

## 2. 播放控制（POST）

通过 **HTTP POST 请求**，向播放器发送 **播放视频指令**。

**接口地址**  
`http://{UNPLAY_IP}:9030/`

**请求方式**

| 项 | 值 |
| --- | --- |
| Method | `POST` |
| Content-Type | `application/x-www-form-urlencoded` |

**POST 参数说明**

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| `urls` | 是 | 播放资源列表（URL 编码） |
| `option` | 是 | 播放器类型 |
| `mode` | 是 | 播放模式 |

### 2.1 `urls` 参数（重点）

**规则**
1. `urls` 必须进行 URL 编码  
2. 内容为每行一条播放资源  
3. 每一行格式：`标题$播放地址`

**示例（编码前）**
```
第20集$http://192.168.0.1/test.mp4
第20集,http://192.168.0.1/test.mp4
```

**示例（URL 编码后）**
```
%E7%AC%AC20%E9%9B%86%24http%3A%2F%2F192.168.0.1%2Ftest.mp4
```

**多条视频时**
1. 使用换行符分隔多条资源  
2. 对整体内容进行 URL 编码

---

### 2.2 `option` 参数（播放器类型）

| 值 | 说明 |
| --- | --- |
| `native` | 使用原生播放器（默认推荐） |
| `ffmpeg` | 使用 FFmpeg 播放器 |

### 2.3 `mode` 参数（播放模式）

| 值 | 说明 |
| --- | --- |
| `none` | 只播放一集 |
| `playList` | 播放列表模式 |

---

### 2.4 完整请求示例

```http
POST http://192.168.0.111:9030/
Content-Type: application/x-www-form-urlencoded

urls=%E7%AC%AC20%E9%9B%86%24http%3A%2F%2F192.168.0.1%2Ftest.mp4&option=native&mode=playList
```
