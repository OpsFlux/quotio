# Quotio - 代码库摘要

> **最后更新**: 2025年1月2日
> **Swift 版本**: 6.0
> **最低 macOS**: 15.0 (Sequoia)

---

## 目录

1. [技术栈](#technology-stack)
2. [依赖项](#dependencies)
3. [高级模块概述](#high-level-module-overview)
4. [关键文件及其用途](#key-files-and-their-purposes)
5. [数据流概述](#data-flow-overview)
6. [构建和配置文件](#build-and-configuration-files)

---

## 技术栈

| 类别 | 技术 |
|----------|------------|
| **平台** | macOS 15.0+ (Sequoia) |
| **语言** | Swift 6 带严格并发 |
| **UI 框架** | SwiftUI |
| **应用框架** | AppKit (用于 NSStatusBar, NSPasteboard) |
| **并发** | Swift Concurrency (async/await, actors) |
| **状态管理** | Observable 宏模式 |
| **包管理器** | Swift Package Manager |
| **自动更新** | Sparkle Framework |

### 使用的关键 Swift 6 特性

- **`@Observable`** 宏用于响应式状态
- **`@MainActor`** 用于 UI 绑定类
- **`actor`** 用于线程安全服务
- **`Sendable`** 一致性用于跨 actor 数据
- **`async/await`** 用于所有异步操作

---

## 依赖项

### 第三方依赖项

| 依赖 | 用途 | 集成方式 |
|------------|---------|-------------|
| **Sparkle** | 自动更新框架 | Swift Package Manager |

### 系统框架

| 框架 | 用途 |
|-----------|---------|
| **SwiftUI** | 用户界面 |
| **AppKit** | 菜单栏、剪贴板、工作区 |
| **Foundation** | 核心工具、网络 |
| **ServiceManagement** | 启动服务 |

### 外部二进制文件

| 二进制文件 | 来源 | 用途 |
|--------|--------|---------|
| **CLIProxyAPI** | GitHub (自动下载) | 本地代理服务器 |

---

## 高级模块概述

### 应用层

```
Quotio/
├── QuotioApp.swift          # 应用入口、生命周期管理
└── Info.plist               # 应用元数据和权限
```

### 模型层

```
Quotio/Models/
├── Models.swift             # 核心数据类型 (AIProvider, AuthFile 等)
├── AgentModels.swift        # CLI 代理配置类型
├── AntigravityActiveAccount.swift # Antigravity 账户模型和切换状态
├── AppMode.swift            # 应用模式管理 (Full/Quota-Only)
└── MenuBarSettings.swift    # 菜单栏配置和持久化
```

### 服务层

```
Quotio/Services/
├── CLIProxyManager.swift        # 代理进程生命周期
├── ManagementAPIClient.swift    # 代理 API 的 HTTP 客户端
├── StatusBarManager.swift       # NSStatusBar 管理
├── StatusBarMenuBuilder.swift   # 原生 NSMenu 构建器 (菜单栏内容)
├── NotificationManager.swift    # 用户通知处理
├── UpdaterService.swift         # Sparkle 集成
├── AgentDetectionService.swift  # CLI 代理检测
├── AgentConfigurationService.swift # 代理配置生成
├── ShellProfileManager.swift    # Shell 配置更新
├── DirectAuthFileService.swift  # 直接认证文件扫描
├── CLIExecutor.swift            # CLI 命令执行
├── LanguageManager.swift        # 本地化管理
├── AntigravityAccountSwitcher.swift  # 账户切换协调器
├── AntigravityDatabaseService.swift  # SQLite 数据库操作
├── AntigravityProcessManager.swift   # IDE 进程生命周期管理
├── AntigravityProtobufHandler.swift  # Protobuf 编码/解码
└── *QuotaFetcher.swift          # 提供商特定的配额获取器 (7 个文件)
```

### ViewModels 层

```
Quotio/ViewModels/
├── QuotaViewModel.swift         # 主应用状态容器
└── AgentSetupViewModel.swift    # 代理配置状态
```

### 视图层

```
Quotio/Views/
├── Components/
│   ├── AccountRow.swift         # 带切换按钮的账户行
│   ├── AgentCard.swift          # 代理显示卡片
│   ├── AgentConfigSheet.swift   # 代理配置表单
│   ├── ProviderIcon.swift       # 提供商标题组件
│   ├── QuotaCard.swift          # 配额显示卡片
│   ├── QuotaProgressBar.swift   # 进度条组件
│   ├── SidebarView.swift        # 导航侧边栏
│   └── SwitchAccountSheet.swift # 账户切换确认对话框
└── Screens/
    ├── DashboardScreen.swift    # 主仪表板
    ├── QuotaScreen.swift        # 配额监控
    ├── ProvidersScreen.swift    # 提供商管理
    ├── AgentSetupScreen.swift   # 代理配置
    ├── APIKeysScreen.swift      # API 密钥管理
    ├── LogsScreen.swift         # 日志查看器
    └── SettingsScreen.swift     # 应用设置
```

### 资源

```
Quotio/Assets.xcassets/
├── AppIcon.appiconset/          # 应用图标 (生产环境)
├── AppIconDev.appiconset/       # 应用图标 (开发环境)
├── MenuBarIcons/                # 菜单栏提供商图标
├── ProviderIcons/               # 提供商标志
└── AccentColor.colorset/        # 强调色定义
```

---

## 关键文件及其用途

### 入口点

| 文件 | 用途 |
|------|---------|
| **QuotioApp.swift** | 应用入口、场景定义、AppDelegate、ContentView、菜单栏协调 |

### 核心数据类型

| 文件 | 关键类型 | 用途 |
|------|-----------|---------|
| **Models.swift** | `AIProvider`, `ProxyStatus`, `AuthFile`, `UsageStats`, `AppConfig`, `NavigationPage` | 核心域模型 |
| **AgentModels.swift** | `CLIAgent`, `AgentConfigType`, `ModelSlot`, `AgentStatus`, `AgentConfiguration` | CLI 代理类型 |
| **AppMode.swift** | `AppMode`, `AppModeManager` | Full/Quota-Only 模式管理 |
| **MenuBarSettings.swift** | `MenuBarQuotaItem`, `MenuBarColorMode`, `QuotaDisplayMode`, `MenuBarSettingsManager`, `AppearanceManager` | 菜单栏配置 |

### 服务

| 文件 | 关键类/Actor | 用途 |
|------|-----------------|---------|
| **CLIProxyManager.swift** | `CLIProxyManager`, `ProxyError`, `AuthCommand` | 代理二进制生命周期、下载、CLI 认证命令 |
| **ManagementAPIClient.swift** | `ManagementAPIClient`, `APIError` | 向代理管理 API 发起 HTTP 请求 |
| **StatusBarManager.swift** | `StatusBarManager` | NSStatusItem 管理、弹出框处理 |
| **NotificationManager.swift** | `NotificationManager` | 用户通知交付和管理 |
| **AgentDetectionService.swift** | `AgentDetectionService` | 查找已安装的 CLI 代理 |
| **AgentConfigurationService.swift** | `AgentConfigurationService` | 生成代理配置 |
| **ShellProfileManager.swift** | `ShellProfileManager` | 更新 shell 配置 (zsh/bash/fish) |

### 配额获取器

| 文件 | 提供商 | 方法 |
|------|-------------|--------|
| **AntigravityQuotaFetcher.swift** | Antigravity | 使用认证文件的 API 调用 |
| **OpenAIQuotaFetcher.swift** | Codex (OpenAI) | 使用认证文件的 API 调用 |
| **CopilotQuotaFetcher.swift** | GitHub Copilot | 使用认证文件的 API 调用 |
| **ClaudeCodeQuotaFetcher.swift** | Claude | CLI 命令 (`claude usage`) |
| **CursorQuotaFetcher.swift** | Cursor | 浏览器会话/数据库 |
| **CodexCLIQuotaFetcher.swift** | Codex | CLI 认证文件 (`~/.codex/auth.json`) |
| **GeminiCLIQuotaFetcher.swift** | Gemini | CLI 认证文件 (`~/.gemini/oauth_creds.json`) |

### ViewModels

| 文件 | 关键类 | 职责 |
|------|-----------|------------------|
| **QuotaViewModel.swift** | `QuotaViewModel`, `OAuthState` | 中央应用状态、代理控制、OAuth 流程、配额管理、菜单栏项 |
| **AgentSetupViewModel.swift** | `AgentSetupViewModel` | 代理检测、配置、测试 |

---

## 数据流概述

### 应用启动流程

```
1. QuotioApp.init()
   │
   ├─▶ @State viewModel = QuotaViewModel()
   │   └─▶ CLIProxyManager.shared 初始化
   │
   ├─▶ 检查引导状态
   │   └─▶ 如果未完成则显示 ModePickerView
   │
   └─▶ initializeApp()
       ├─▶ 应用外观设置
       ├─▶ 基于模式的初始化
       │   ├─▶ 完整模式：如果启用了 autoStart 则启动代理
       │   └─▶ 仅配额：加载直接认证文件、获取配额
       │
       └─▶ 更新状态栏
```

### 完整模式数据流

```
┌──────────────────┐
│   用户操作    │
│ (启动代理)    │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ QuotaViewModel   │
│  .startProxy()   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐       ┌────────────────────┐
│ CLIProxyManager  │──────▶│   CLIProxyAPI      │
│    .start()      │       │   (二进制)         │
└────────┬─────────┘       └────────────────────┘
         │
         ▼
┌──────────────────┐
│ ManagementAPI    │
│    Client        │ ◀─── HTTP 请求到 localhost:8317
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  自动刷新    │ ──── 每 15 秒
│    任务          │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ UI 通过   │
│  @Observable     │
└──────────────────┘
```

### 配额获取流程

```
┌──────────────────────────────────────────────────────┐
│                  refreshAllQuotas()                   │
└────────────────────────┬─────────────────────────────┘
                         │
    ┌────────────────────┼────────────────────┐
    │                    │                    │
    ▼                    ▼                    ▼
┌─────────┐        ┌─────────┐         ┌─────────┐
│Antigrav │        │ OpenAI  │         │ Copilot │
│ Fetcher │        │ Fetcher │         │ Fetcher │
└────┬────┘        └────┬────┘         └────┬────┘
    │                   │                    │
    ▼                   ▼                    ▼
┌─────────────────────────────────────────────────┐
│           providerQuotas: [AIProvider:         │
│                 [String: ProviderQuotaData]]   │
└─────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────┐
│           StatusBarManager.updateStatusBar()    │
└─────────────────────────────────────────────────┘
```

### OAuth 认证流程

```
┌──────────────┐     ┌───────────────┐     ┌─────────────────┐
│    用户      │────▶│ QuotaViewModel │────▶│ Management API  │
│ 点击认证  │     │  .startOAuth() │     │ Client          │
└──────────────┘     └───────┬───────┘     └────────┬────────┘
                             │                      │
                             │     GET /xxx-auth-url
                             │◀─────────────────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  打开浏览器   │
                    │   (OAuth URL)   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  轮询状态    │
                    │  (每 2 秒)     │
                    └────────┬────────┘
                             │
                    成功? ─┴─ 继续轮询
                             │
                             ▼
                    ┌─────────────────┐
                    │  刷新数据     │
                    └─────────────────┘
```

### 代理配置流程

```
┌─────────────────┐     ┌──────────────────────┐
│  AgentSetup     │────▶│ AgentSetupViewModel   │
│   Screen        │     │  .applyConfiguration()│
└─────────────────┘     └──────────┬───────────┘
                                   │
                                   ▼
                       ┌───────────────────────┐
                       │ AgentConfiguration    │
                       │      Service          │
                       └──────────┬────────────┘
                                  │
         ┌────────────────────────┼────────────────────────┐
         │                        │                        │
         ▼                        ▼                        ▼
┌─────────────────┐    ┌─────────────────┐     ┌─────────────────┐
│  写入配置   │    │  更新 Shell  │     │   复制到       │
│   JSON/TOML     │    │    配置      │     │   剪贴板     │
└─────────────────┘    └─────────────────┘     └─────────────────┘
```

---

## 构建和配置文件

### Xcode 项目

| 文件/目录 | 用途 |
|----------------|---------|
| **Quotio.xcodeproj/** | Xcode 项目容器 |
| **project.pbxproj** | 项目设置、目标、构建阶段 |
| **xcschemes/Quotio.xcscheme** | 构建方案配置 |
| **Package.resolved** | Swift Package Manager 依赖锁定 |

### 构建配置

| 文件 | 用途 |
|------|---------|
| **Config/Debug.xcconfig** | Debug 构建设置 |
| **Config/Release.xcconfig** | Release 构建设置 |
| **Config/Local.xcconfig.example** | 本地覆盖模板 |

### 构建脚本

| 脚本 | 用途 |
|--------|---------|
| **scripts/build.sh** | 构建发布归档 |
| **scripts/release.sh** | 完整发布工作流 |
| **scripts/bump-version.sh** | 版本管理 |
| **scripts/notarize.sh** | Apple 公证 |
| **scripts/package.sh** | DMG 打包 |
| **scripts/generate-appcast.sh** | Sparkle appcast 生成 |
| **scripts/config.sh** | 共享配置 |
| **scripts/ExportOptions.plist** | 归档导出选项 |

### 应用配置

| 文件 | 用途 |
|------|---------|
| **Info.plist** | 应用元数据、权限、URL 方案 |
| **Quotio.entitlements** | 沙盒和功能权限 |

---

## 运行时文件位置

### 应用支持

```
~/Library/Application Support/Quotio/
├── CLIProxyAPI          # 下载的代理二进制文件
└── config.yaml          # 代理配置
```

### 认证文件目录

```
~/.cli-proxy-api/
├── gemini-cli-*.json    # Gemini 认证文件
├── claude-*.json        # Claude 认证文件
├── codex-*.json         # Codex 认证文件
├── github-copilot-*.json # Copilot 认证文件
└── ...                  # 其他提供商认证文件
```

### 用户默认键

| 键 | 类型 | 用途 |
|-----|------|---------|
| `proxyPort` | Int | 代理服务器端口 |
| `managementKey` | String | 管理 API 密钥 |
| `autoStartProxy` | Bool | 启动时自动启动代理 |
| `appMode` | String | 当前应用模式 |
| `hasCompletedOnboarding` | Bool | 引导完成状态 |
| `menuBarSelectedQuotaItems` | Data | 选定的菜单栏项 |
| `menuBarColorMode` | String | 菜单栏颜色模式 |
| `showMenuBarIcon` | Bool | 显示菜单栏图标 |
| `menuBarShowQuota` | Bool | 在菜单栏显示配额 |
| `quotaDisplayMode` | String | 配额显示模式 |
| `loggingToFile` | Bool | 启用文件日志 |
| `appearanceMode` | String | 浅色/深色/系统模式 |
| `quotaAlertThreshold` | Double | 低配额通知阈值 |

---

## 本地化结构

```
Quotio/
└── Resources/
    ├── en.lproj/
    │   └── Localizable.strings
    └── vi.lproj/
        └── Localizable.strings
```

### 本地化键模式

| 模式 | 示例 | 用途 |
|---------|---------|-------|
| `nav.*` | `nav.dashboard` | 导航标签 |
| `action.*` | `action.startProxy` | 按钮操作 |
| `status.*` | `status.running` | 状态指示器 |
| `settings.*` | `settings.port` | 设置标签 |
| `error.*` | `error.invalidURL` | 错误消息 |
