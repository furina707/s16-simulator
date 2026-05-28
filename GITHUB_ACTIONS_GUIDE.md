# GitHub Actions 自动构建 APK 指南

## 概述

本项目已配置 GitHub Actions，可以自动构建 Android APK 并发布到 GitHub Releases。

## 触发方式

GitHub Actions 会在以下情况自动运行：

1. **推送到 main/master 分支** - 自动构建并发布
2. **创建 Pull Request** - 构建但不发布
3. **手动触发** - 在 Actions 页面点击 "Run workflow"

## 使用步骤

### 1. 上传到 GitHub 仓库

```bash
# 初始化 Git 仓库
git init
git add .
git commit -m "Initial commit"

# 推送到 GitHub
gh repo create s16-simulator --public --source=. --push
# 或
# git remote add origin https://github.com/YOUR_USERNAME/s16-simulator.git
# git push -u origin main
```

### 2. 查看构建状态

- 打开 GitHub 仓库页面
- 点击 "Actions" 标签
- 查看最新的构建状态

### 3. 下载 APK

构建完成后，APK 可以通过两种方式获取：

#### 方式一：Artifacts（每次构建）
- 在 Actions 页面点击完成的 workflow
- 滚动到底部找到 "Artifacts" 部分
- 下载 `s16-simulator-apk`

#### 方式二：Releases（main分支推送）
- 打开仓库的 "Releases" 页面
- 找到最新的 `latest` 标签
- 下载 `app-debug.apk`

## 工作流配置说明

`.github/workflows/build-apk.yml` 包含以下步骤：

| 步骤 | 说明 |
|------|------|
| Checkout | 拉取代码 |
| Setup Node.js | 安装 Node.js 20 |
| Setup Java | 安装 JDK 17 |
| Install deps | 安装 npm 依赖 |
| Build web | 构建 Web 资源 |
| Sync Android | 同步到 Capacitor Android |
| Build APK | 使用 Gradle 构建 |
| Upload | 上传 APK 作为 artifact |
| Release | 发布到 GitHub Releases |

## 自定义配置

### 修改触发条件

编辑 `.github/workflows/build-apk.yml`：

```yaml
on:
  push:
    branches: [main, master, develop]  # 添加更多分支
  schedule:
    - cron: '0 0 * * 0'  # 每周日自动构建
```

### 添加签名（发布到应用商店）

如需发布到 Google Play，需要添加签名配置：

1. 生成签名密钥：
```bash
keytool -genkey -v -keystore s16.keystore -alias s16 -keyalg RSA -keysize 2048 -validity 10000
```

2. 在 GitHub 仓库设置中添加 Secrets：
   - `KEYSTORE_BASE64`: base64 编码的 keystore 文件
   - `KEYSTORE_PASSWORD`: 密钥库密码
   - `KEY_ALIAS`: 密钥别名
   - `KEY_PASSWORD`: 密钥密码

3. 修改工作流添加签名步骤

## 常见问题

### Q: 构建失败怎么办？
A: 查看 Actions 页面的日志，通常是依赖安装或 Gradle 配置问题。

### Q: 如何加快构建速度？
A: 工作流已配置 npm 缓存，Gradle 也会自动缓存依赖。

### Q: 可以构建 Release 版本吗？
A: 可以，将 `./gradlew assembleDebug` 改为 `./gradlew assembleRelease`，但需要配置签名。

## 相关文件

- `.github/workflows/build-apk.yml` - GitHub Actions 工作流
- `BUILD_GUIDE.md` - 本地构建指南
- `capacitor.config.ts` - Capacitor 配置
