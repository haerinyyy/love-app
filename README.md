# 💝 给宝贝 — 使用文档（维护者用）

## 项目结构

```
love-app/
├── index.html         # 主页面（所有代码都在这里）
├── manifest.json       # PWA 配置（添加到桌面用）
├── README.md           # 本文件
├── my melody/          # 美乐蒂图片（24张）
└── zhangyuge/          # 章鱼哥图片（31张）
```

## 日常维护

### 修改情话

打开 `index.html`，搜索 `LOVE_LETTERS`，找到数组：

```javascript
const LOVE_LETTERS = [
  "今天天气很好，但不如你笑起来的样子好看。",
  // ... 想改哪条改哪条，想加就加
];
```

- 加：直接在数组末尾加一行，英文逗号结尾
- 删：删掉对应的那行
- 改：直接改引号里的文案
- 数量不限，加多少都行

### 修改重要日期

搜索 `CONFIG`，找到：

```javascript
const CONFIG = {
  togetherDate: new Date(2025, 11, 31, 13, 0, 0), // 在一起的时间
  herBirthday: { month: 12, day: 28 },             // 她的生日
  city: 'Beijing',                                  // 天气城市
};
```

注意：JavaScript 的月份从 0 开始（1月=0，12月=11）。

### 添加新图片

1. 把图片放进对应的文件夹（`my melody/` 或 `zhangyuge/`）
2. 打开 `index.html`，搜索 `buildImagePaths`
3. 修改图片编号范围：

```javascript
function buildImagePaths() {
  const melody = [];
  for (let i = 6527; i <= 6550; i++) {  // ← 改这里的数字范围
    melody.push(`my%20melody/IMG_${i}.JPG`);
  }
  const squid = [];
  for (let i = 6551; i <= 6581; i++) {  // ← 改这里的数字范围
    squid.push(`zhangyuge/IMG_${i}.JPG`);
  }
  return { melody, squid, all: [...melody, ...squid] };
}
```

### 修改天气提醒规则

搜索 `getWeatherRemark`，找到温度判断逻辑，直接改文案或温度阈值即可。

### 重置「想你」计数

在页面上**长按「想你」按钮 15 秒**，会弹出「已归零 ✅」提示。

如果想手动改，打开浏览器开发者工具（F12）→ Console，输入：
```javascript
localStorage.setItem('loveApp_missCount', '0')
```

---

## 部署更新流程

改完代码后，执行以下命令同步到两边：

```bash
cd C:\Users\Haerin_y\love-app

git add -A
git commit -m "改了什么写在这里"

# 推送到 GitHub（需要 VPN）
git push origin main

# 推送到 Gitee（国内直连）
git push gitee main
```

**重要：** 腾讯云静态网站托管不会自动同步，每次更新代码后需要手动上传新的 `index.html`：

1. 打开 https://console.cloud.tencent.com/tcb
2. 进入环境 → 左侧「静态网站托管」
3. 找到 `index.html` → 上传替换

---

## 功能介绍

| 功能 | 说明 |
|------|------|
| 问候语 | 根据时间段自动切换（早上好/下午好/晚上好）|
| 在一起天数 | 从 2025.12.31 13:00 算起 |
| 你的生日倒计时 | 12月28日 |
| 周年倒计时 | 12月31日 |
| 里程碑 | 显示下一个整百天 |
| 今日天气 | 北京实时温度 + 贴心提醒 |
| 每日角色图 | 每天随机一张美乐蒂或章鱼哥 |
| 每日情话 | 60条随机循环，每天0点更新 |
| 留言板 | 她可以留言，你可以回复 |
| 想你按钮 | 她点击 → 你微信收到推送（Server酱）|

## 注意事项

- Server酱 SendKey 已写在代码里，如果有人查看源码会看到。目前页面只有你俩用，没问题
- 留言板数据存在她手机的 localStorage 里，换手机会丢失
- 想你计数器也存在她的手机里
