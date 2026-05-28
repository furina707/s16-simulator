# S16 电路模拟器 Android APK 构建指南

## 方式一：在本地电脑构建（推荐）

### 前置要求
1. 安装 Node.js (v16+)
2. 安装 Android Studio 或命令行 Android SDK

### 构建步骤

```bash
# 1. 打开终端，进入项目目录
cd s16-apk-builder

# 2. 安装依赖
npm install

# 3. 添加 Android 平台（如果还没有）
npx cap add android

# 4. 同步 Web 内容到 Android
npx cap sync android

# 5. 构建 APK
cd android
./gradlew assembleDebug

# 6. APK 文件位置
# android/app/build/outputs/apk/debug/app-debug.apk
```

## 方式二：使用 Android Studio

1. 用 Android Studio 打开 `s16-apk-builder/android` 文件夹
2. 等待 Gradle 同步完成
3. 点击 Run > Build APK
4. APK 将在 `app/build/outputs/apk/debug/` 目录生成

## 安装 APK

构建完成后，将 `app-debug.apk` 复制到手机上安装。

首次安装可能需要在设置中开启"允许安装未知来源应用"。

## 应用信息

- **应用名称**: S16模拟器
- **包名**: com.s16.simulator
- **版本**: 1.0.0
- **最小 Android 版本**: 22 (Android 5.1)

## 功能说明

- 运算放大器仿真（反相/同相放大、积分、微分）
- RC元件阵列
- 信号合成（加法器）
- 实时波形显示
- 触摸连线交互

---
构建时间: 2026-05-28
