# Djian6.github.io


# AI 图像生成 API 文档

## 概述

AI 图像生成服务提供文生图功能，基于 FLUX.1-Dev 模型，支持多种风格、色调、光照和构图选项。

## 基础信息

| 项目 | 值 |
|------|-----|
| API 地址 | `https://api.aizdzj.com/draw/text2image.php` |
| 请求方法 | POST |
| 请求类型 | `application/x-www-form-urlencoded; charset=UTF-8` |
| 响应类型 | `application/json` |

---

## 接口列表

### 1. 文生图

提交图像生成请求，返回任务 ID。

**请求地址**

```
POST https://api.aizdzj.com/draw/text2image.php
```

**请求参数**

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
|--------|------|------|--------|------|
| `prompt` | string | 是 | - | 图像描述文本，英文效果最佳 |
| `size` | string | 否 | `1024*1024` | 图像尺寸，格式为 `宽*高` |
| `model` | string | 否 | `flux-dev` | 模型名称 |
| `influence` | number | 否 | `100` | 影响力，范围 0-100 |
| `image_name` | string | 否 | - | 参考图名称（可选） |

**支持的尺寸选项**

| 尺寸 | 说明 |
|------|------|
| `1024*1024` | 正方形 |
| `1024*768` | 横屏 4:3 |
| `768*1024` | 竖屏 3:4 |
| `1024*576` | 宽屏 16:9 |
| `576*1024` | 竖屏 9:16 |
| `512*512` | 小正方形 |

**支持的风格选项（拼接到 prompt 末尾）**

| 风格 | 英文值 |
|------|--------|
| 电影 | Cinematic |
| 霓虹朋克 | Neon Punk |
| 动漫 | Anime |
| 摄影 | Photography |
| 奇幻艺术 | Fantasy Art |
| 低多边形 | Low Polygon |
| 模拟胶片 | Analog Film |
| 3D模型 | 3D Model |
| 漫画书 | Comic Book |
| 手工黏土 | Handmade Clay |

**支持的色调选项**

| 色调 | 英文值 |
|------|--------|
| 冷色调 | Cool Color Tone |
| 鲜艳色彩 | Vibrant Colors |
| 柔和色彩 | Soft Colors |
| 暖色调 | Warm Tone |
| 黑白 | Black and White |

**支持的光照选项**

| 光照 | 英文值 |
|------|--------|
| 戏剧性 | Dramatic Lighting |
| 体积光 | Volumetric Light |
| 黄金时光 | Golden Hour |
| 影棚 | Studio Lighting |
| 轮廓光 | Rim Light |
| 阳光 | Sunlight |
| 低光 | Low Light |
| 背光 | Backlight |
| 丁达尔光 | Tyndall Light |

**支持的构图选项**

| 构图 | 英文值 |
|------|--------|
| 广角 | Wide Angle |
| 仰视角度 | Low Angle |
| 特写 | Close-up |
| 浅景深 | Shallow Depth of Field |
| 俯视角度 | Top-down View |
| 背景模糊 | Bokeh |

**请求示例**

```bash
curl "https://api.aizdzj.com/draw/text2image.php" \
  -H "accept: */*" \
  -H "content-type: application/x-www-form-urlencoded; charset=UTF-8" \
  -H "origin: https://draw.freeforai.com" \
  -H "referer: https://draw.freeforai.com/" \
  -H "user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36" \
  --data-raw "prompt=Steampunk+airship+flying+above+giant+mechanical+gears&size=1024*1024&model=flux-dev&influence=100"
```

**成功响应**

```json
{
  "task_id": "gitee_6a1b02b644417_6013"
}
```

**错误响应**

```json
{
  "error": "错误信息描述"
}
```

---

### 2. 查询任务状态

根据任务 ID 查询图像生成结果。

**请求地址**

```
POST https://api.aizdzj.com/draw/text2image.php
```

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `task_id` | string | 是 | 文生图接口返回的任务 ID |

**请求示例**

```bash
curl "https://api.aizdzj.com/draw/text2image.php" \
  -H "accept: */*" \
  -H "content-type: application/x-www-form-urlencoded; charset=UTF-8" \
  -H "origin: https://draw.freeforai.com" \
  -H "referer: https://draw.freeforai.com/" \
  -H "user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36" \
  --data-raw "task_id=gitee_6a1b02b644417_6013"
```

**成功响应（生成完成）**

```json
{
  "task_status": "SUCCEEDED",
  "url": "https://gitee-ai.su.bcebos.com/v1/public/temp/26053100/ZAASABCBBETPUEAE.png",
  "result": {
    "data": [
      {
        "url": "https://gitee-ai.su.bcebos.com/v1/public/temp/26053100/ZAASABCBBETPUEAE.png"
      }
    ],
    "created": 1780158019159
  }
}
```

**处理中响应**

```json
{
  "task_status": "PENDING"
}
```

**失败响应**

```json
{
  "task_status": "FAILED",
  "error": "网络请求错误: Operation timed out after 10000 milliseconds with 0 out of 0 bytes received",
  "error_details": []
}
```

---

## 任务状态说明

| task_status 值 | 说明 |
|---------------|------|
| `SUCCEEDED` | 生成成功，可获取图片URL |
| `COMPLETED` | 生成完成（同 SUCCEEDED） |
| `PENDING` | 处理中 |
| `FAILED` | 生成失败 |

---

## 完整流程示例

1. **提交生成请求**

```javascript
const response = await fetch('https://api.aizdzj.com/draw/text2image.php', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'Origin': 'https://draw.freeforai.com',
    'Referer': 'https://draw.freeforai.com/',
  },
  body: 'prompt=Steampunk airship&size=1024*1024&model=flux-dev&influence=100'
});
const data = await response.json();
console.log(data.task_id); // gitee_xxxxx_xxxx
```

2. **轮询查询结果**

```javascript
async function pollTask(taskId, maxRetries = 30, interval = 5000) {
  for (let i = 0; i < maxRetries; i++) {
    const res = await fetch('https://api.aizdzj.com/draw/text2image.php', {
      method: 'POST',
      headers: { 'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8' },
      body: `task_id=${taskId}`
    });
    const data = await res.json();

    if (data.task_status === 'SUCCEEDED' || data.task_status === 'COMPLETED') {
      return data.url || data.result?.data?.[0]?.url;
    }

    if (data.task_status === 'FAILED') {
      throw new Error(data.error || '生成失败');
    }

    await new Promise(r => setTimeout(r, interval));
  }
  throw new Error('超时');
}
```

---

## 注意事项

1. **跨域限制**：浏览器直接请求会受 CORS 限制，建议通过代理服务器转发
2. **建议轮询间隔**：3-5 秒，最多轮询 30-60 次
3. **图片 URL 有效期**：生成后的图片链接可能有有效期，请及时下载保存
4. **提示词**：使用英文描述可获得更好的生成效果
