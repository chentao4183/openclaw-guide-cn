# 移动端 Nodes

连接 iOS/Android 设备，扩展 AI 能力到移动端。

---

## 什么是 Nodes？

Nodes 是运行在移动设备上的 OpenClaw 客户端，让 AI 能够：
- 📷 拍照和录像
- 📍 获取位置信息
- 📱 读取屏幕内容
- 🔔 发送通知

---

## 支持平台

| 平台 | 状态 | 功能 |
|------|------|------|
| iOS | ✅ | 相机、位置、通知、屏幕 |
| Android | ✅ | 相机、位置、通知、屏幕 |
| macOS | ✅ | 相机、位置、通知、屏幕 |

---

## 安装 Nodes

### iOS

1. 从 App Store 下载 OpenClaw Nodes
2. 打开应用
3. 扫描配对二维码

### Android

1. 从 Google Play 下载 OpenClaw Nodes
2. 打开应用
3. 扫描配对二维码

---

## 配对流程

### 1. 启动配对

在设备上打开 OpenClaw Nodes 应用，会显示一个配对码。

### 2. 批准配对

```bash
# 查看待配对设备
openclaw devices pending

# 批准配对
openclaw devices approve <requestId>
```

### 3. 验证连接

```bash
# 查看已配对设备
openclaw nodes status
```

---

## 使用 Nodes

### 拍照

```bash
# 拍照
openclaw nodes camera snap --node <node-id>

# 使用前置摄像头
openclaw nodes camera snap --node <node-id> --facing front

# 录制视频
openclaw nodes camera clip --node <node-id> --duration 10
```

### 获取位置

```bash
# 获取当前位置
openclaw nodes location get --node <node-id>
```

### 发送通知

```bash
# 发送通知
openclaw nodes notify --node <node-id> --title "提醒" --body "别忘了开会"
```

### 屏幕录制

```bash
# 录制屏幕
openclaw nodes screen record --node <node-id> --duration 30
```

---

## 在对话中使用

直接告诉 AI：

```
"用我的手机拍张照片"
"获取我的当前位置"
"给我的手机发个通知"
```

AI 会自动调用对应的 Node 功能。

---

## Node 配置

### 权限设置

在设备上的 OpenClaw Nodes 应用中配置：

| 权限 | 说明 |
|------|------|
| 相机 | 拍照和录像 |
| 位置 | 获取 GPS 位置 |
| 通知 | 发送推送通知 |
| 屏幕录制 | 录制屏幕内容 |

### 设备命名

```bash
# 设置设备名称
openclaw nodes rename --node <node-id> --name "我的手机"
```

---

## 安全考虑

### 权限最小化

只授予必要的权限：
- 不需要位置？禁用位置权限
- 不需要屏幕录制？禁用屏幕权限

### 审计日志

所有 Node 操作都会记录在 Gateway 日志中：

```bash
openclaw logs --follow | grep node
```

---

## 多设备管理

```bash
# 列出所有设备
openclaw nodes status

# 查看设备详情
openclaw nodes describe --node <node-id>

# 移除设备
openclaw nodes remove --node <node-id>
```

---

## 使用场景

### 场景 1：家庭监控

```
"每隔 1 小时用客厅的 iPad 拍张照片发给我"
```

### 场景 2：位置追踪

```
"获取我孩子的位置"
```

### 场景 3：远程通知

```
"当我离开办公室时，给我的手机发通知"
```

---

## 故障排查

### Q: 设备显示离线

**A:**
1. 检查设备网络连接
2. 确认应用在前台运行（iOS 限制）
3. 重启应用

### Q: 拍照失败

**A:**
1. 确认相机权限已授予
2. 检查设备存储空间
3. 重启应用

### Q: 位置获取失败

**A:**
1. 确认位置权限已授予
2. 检查设备 GPS 是否开启
3. 尝试在室外获取位置

---

## 下一步

- [定时任务](./cron.md)
- [Sub-Agents](./sub-agents.md)
