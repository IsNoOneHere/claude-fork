# Claude Code 桌面端 — 安装与构建指南

> 从下载到运行，覆盖 macOS / Windows / Linux 三大平台。

<p align="center">
<a href="#一下载安装包">下载</a> · <a href="#二macos-安装">macOS</a> · <a href="#三windows-安装">Windows</a> · <a href="#四linux-安装">Linux</a> · <a href="#五从源码构建">源码构建</a> · <a href="#六自动更新">自动更新</a> · <a href="#七发布流程开发者">发布流程</a>
</p>

---

## 一、下载安装包

前往 [GitHub Releases](https://github.com/NanmiCoder/cc-haha/releases) 页面，根据你的系统下载对应安装包：

| 平台 | 架构 | 文件 |
|------|------|------|
| macOS | Apple Silicon (M1/M2/M3/M4) | `Claude.Code.Haha_x.x.x_aarch64.dmg` |
| macOS | Intel | `Claude.Code.Haha_x.x.x_x64.dmg` |
| Windows | x64 | `Claude.Code.Haha_x.x.x_x64-setup.exe` |
| Linux | x64 | `.deb` 或 `.AppImage` |
| Linux | ARM64 | `.deb` 或 `.AppImage` |

> **不确定 Mac 的架构？** 点击左上角  → 关于本机 → 查看"芯片"信息。Apple M 开头就选 ARM64，Intel 就选 x64。

---

## 二、macOS 安装

### 正常安装

1. 双击下载的 `.dmg` 文件
2. 将 `Claude Code Haha.app` 拖入 `Applications` 文件夹
3. 在启动台或 `Applications` 中打开应用

### 处理"未签名应用"提示

由于应用暂未进行 Apple 公证签名，macOS 会阻止首次运行。有以下几种方式处理：

#### 方法一：右键打开（最简单）

1. 在 **Finder** 中找到 `Claude Code Haha.app`（在应用程序文件夹中）
2. **右键点击**（或 Control + 点击）该应用
3. 选择 **「打开」**
4. 在弹出的对话框中点击 **「打开」**

> 只需操作一次，后续可正常双击启动。

#### 方法二：命令行移除隔离标记（推荐）

打开终端，执行：

```bash
xattr -cr /Applications/Claude\ Code\ Haha.app
```

此命令移除 macOS 的 Gatekeeper 隔离属性，之后可正常双击启动。

#### 方法三：系统设置中允许

1. 打开 **系统设置** → **隐私与安全性**
2. 向下滚动，在安全性区域找到关于 Claude Code Haha 被阻止的提示
3. 点击 **「仍然打开」**
4. 输入密码确认

> macOS Sequoia (15.x) 及以上版本可能需要先尝试打开一次应用，才会在系统设置中出现该选项。

### macOS 系统要求

- macOS 11.0 (Big Sur) 或更高版本
- Apple Silicon 或 Intel 处理器

---

## 三、Windows 安装

### 正常安装

1. 双击下载的 `.exe` 安装程序
2. 按照 NSIS 安装向导完成安装
3. 安装完成后从开始菜单或桌面快捷方式启动

### 处理 SmartScreen 拦截

由于应用暂未进行 Windows 代码签名，首次运行时 SmartScreen 会弹出警告。

#### 绕过方式

1. 在 SmartScreen 蓝色窗口中点击 **「更多信息」**（More info）
2. 点击 **「仍要运行」**（Run anyway）

> 只需第一次运行时操作，后续不再提示。

#### 如果没有"更多信息"选项

部分企业版 Windows 可能隐藏了该选项。可以通过以下方式临时关闭 SmartScreen：

1. 打开 **Windows 安全中心** → **应用和浏览器控制**
2. 点击 **「基于信誉的保护设置」**
3. 临时关闭 **「检查应用和文件」**
4. 安装并运行应用后，建议重新开启此选项

### WebView2 运行时

Claude Code Haha 依赖 Microsoft Edge WebView2 运行时。大多数 Windows 10/11 系统已预装。如果提示缺少 WebView2：

- 从 [Microsoft 官方](https://developer.microsoft.com/en-us/microsoft-edge/webview2/) 下载 Evergreen Bootstrapper 并安装

### Windows 系统要求

- Windows 10 (1803+) 或 Windows 11
- x64 处理器
- WebView2 运行时

---

## 四、Linux 安装

### Debian / Ubuntu（.deb）

```bash
sudo dpkg -i claude-code-haha_x.x.x_amd64.deb
# 如果提示缺少依赖：
sudo apt-get install -f
```

### AppImage（通用）

```bash
chmod +x Claude.Code.Haha_x.x.x_amd64.AppImage
./Claude.Code.Haha_x.x.x_amd64.AppImage
```

> AppImage 无需安装，赋予执行权限后直接运行。

### 系统依赖

Linux 版本需要以下系统库（.deb 安装时会自动处理依赖）：

```bash
# Ubuntu / Debian
sudo apt-get install -y \
  libwebkit2gtk-4.1-0 \
  libgtk-3-0 \
  libayatana-appindicator3-1 \
  librsvg2-2
```

### Linux 系统要求

- Ubuntu 22.04+ 或同等 glibc 版本的发行版
- x64 或 ARM64 处理器
- WebKitGTK 4.1

---

## 五、从源码构建

如果你想自己编译桌面端，或想参与开发：

### 前置依赖

| 工具 | 最低版本 | 安装方式 |
|------|---------|----------|
| **Bun** | 1.0+ | `curl -fsSL https://bun.sh/install \| bash` |
| **Rust** | 1.70+ | `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \| sh` |
| **Node.js** | 20+ | [nodejs.org](https://nodejs.org/) |

**Linux 额外依赖：**

```bash
sudo apt-get install -y \
  libwebkit2gtk-4.1-dev \
  libappindicator3-dev \
  librsvg2-dev \
  patchelf
```

### 构建步骤

```bash
# 1. 克隆仓库
git clone https://github.com/NanmiCoder/cc-haha.git
cd cc-haha

# 2. 安装依赖
bun install
cd desktop && bun install

# 3. 开发模式运行
bun run tauri dev

# 4. 生产构建（生成安装包）
bun run tauri build
```

构建产物位于 `desktop/src-tauri/target/release/bundle/` 目录下。

### 跨平台构建

Sidecar 构建脚本会自动检测当前平台的 Rust target triple，也可以手动指定：

```bash
# 为 macOS Intel 构建 sidecar
TAURI_ENV_TARGET_TRIPLE=x86_64-apple-darwin bun run build:sidecars

# 为 Linux x64 构建 sidecar
TAURI_ENV_TARGET_TRIPLE=x86_64-unknown-linux-gnu bun run build:sidecars
```

> 注意：Tauri 应用本身不支持在 macOS 上交叉编译 Linux/Windows 版本。跨平台构建由 CI/CD 流水线在各自平台的构建环境中完成。

---

## 六、自动更新

桌面端内置了自动更新机制（基于 Tauri Updater 插件）：

1. **检查更新** — 应用启动 5 秒后自动检查 GitHub Releases
2. **提示更新** — 如有新版本，右上角弹出更新提示卡片
3. **下载安装** — 点击「Update now」自动下载并安装
4. **重启应用** — 安装完成后自动重启

> 自动更新需要网络连接到 GitHub。如果你在受限网络环境中，可以手动从 Releases 页面下载。

---

## 七、发布流程（开发者）

项目使用 **tag-based release** 工作流，推送版本标签后自动触发跨平台构建。

### 发布新版本

```bash
# 1. 确保在 main 分支且工作区干净
git checkout main && git pull

# 2. 使用 release 脚本更新版本号
bun run scripts/release.ts patch     # 0.1.0 → 0.1.1
bun run scripts/release.ts minor     # 0.1.0 → 0.2.0
bun run scripts/release.ts major     # 0.1.0 → 1.0.0
bun run scripts/release.ts 2.0.0     # 指定版本号

# 3. 预览变更（不会修改任何文件）
bun run scripts/release.ts patch --dry

# 4. 推送到远程，触发 CI 构建
git push origin main --tags
```

### CI 构建矩阵

推送 `v*.*.*` 标签后，GitHub Actions 自动在 5 个平台并行构建：

| 构建环境 | 目标 | 产物 |
|----------|------|------|
| `macos-latest` | `aarch64-apple-darwin` | `.dmg`, `.app.tar.gz` |
| `macos-latest` | `x86_64-apple-darwin` | `.dmg`, `.app.tar.gz` |
| `ubuntu-22.04` | `x86_64-unknown-linux-gnu` | `.deb`, `.AppImage` |
| `ubuntu-22.04-arm` | `aarch64-unknown-linux-gnu` | `.deb`, `.AppImage` |
| `windows-latest` | `x86_64-pc-windows-msvc` | `.exe`, `.msi` |

所有产物自动上传到 GitHub Draft Release。检查无误后手动点击发布。

### 配置代码签名（可选）

如需正式签名以消除安全警告，需在 GitHub 仓库 Settings → Secrets 中配置：

**macOS（需 Apple Developer Program，$99/年）：**

| Secret | 说明 |
|--------|------|
| `APPLE_CERTIFICATE` | Base64 编码的 .p12 证书 |
| `APPLE_CERTIFICATE_PASSWORD` | .p12 导出密码 |
| `APPLE_SIGNING_IDENTITY` | 如 `Developer ID Application: Your Name (TEAMID)` |
| `APPLE_TEAM_ID` | 10 位 Team ID |
| `APPLE_ID` | Apple 账号邮箱 |
| `APPLE_PASSWORD` | App 专用密码（非 Apple ID 密码） |

**Windows（需 OV 代码签名证书）：**

| Secret | 说明 |
|--------|------|
| `AZURE_CLIENT_ID` | Azure AD 应用客户端 ID |
| `AZURE_TENANT_ID` | Azure AD 租户 ID |
| `AZURE_CLIENT_SECRET` | Azure AD 客户端密钥 |

**自动更新签名：**

| Secret | 说明 |
|--------|------|
| `TAURI_SIGNING_PRIVATE_KEY` | Updater 签名私钥 |
| `TAURI_SIGNING_PRIVATE_KEY_PASSWORD` | 私钥密码 |

生成 Updater 签名密钥：

```bash
bunx tauri signer generate -w ~/.tauri/claude-code-haha.key
```

> 请妥善保管私钥！丢失后已安装用户将无法自动更新。

---

## 八、常见问题

### macOS："app is damaged and can't be opened"

执行以下命令后重试：

```bash
xattr -cr /Applications/Claude\ Code\ Haha.app
```

### macOS："unidentified developer"

参考上方 [方法一：右键打开](#方法一右键打开最简单) 或 [方法三：系统设置](#方法三系统设置中允许)。

### Windows：安装后无法启动

确认系统已安装 WebView2 运行时。从 [Microsoft 官方](https://developer.microsoft.com/en-us/microsoft-edge/webview2/) 下载安装。

### Linux：AppImage 双击无反应

确保已赋予执行权限：

```bash
chmod +x Claude.Code.Haha_*.AppImage
```

### Linux：提示缺少 libwebkit2gtk

```bash
sudo apt-get install libwebkit2gtk-4.1-0
```

### 构建时 sidecar 编译失败

确认 Bun 和 Rust 已正确安装：

```bash
bun --version   # 应 >= 1.0
rustc --version  # 应 >= 1.70
```

### 自动更新不工作

- 确认网络可访问 GitHub
- 检查是否配置了 `TAURI_SIGNING_PRIVATE_KEY`
- 查看应用日志中的 updater 错误信息
