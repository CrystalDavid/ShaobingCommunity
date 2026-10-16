# 烧饼社区 · HarmonyOS

[LinuxSB](https://linux.sb/) 的非官方 HarmonyOS 原生客户端。使用 ArkTS、ArkUI 和 UI Design Kit，实现帖子浏览、板块、搜索、个人主页与发帖回复，配合蓝色主题、通透底栏和深浅色外观。

## 功能

- **社区浏览**：首页连续分页，按板块与排序方式查看帖子。
- **主题详情**：原生正文、回复和楼中楼排版，头像与图片按需加载。
- **搜索与个人主页**：搜索标题、正文和回帖，查看自己的主题与回复。
- **发布与互动**：登录后撰写主题和回复。
- **外观设置**：浅色、深色与跟随系统，两种首页帖子排版。
- **会话恢复**：接入官方登录与 Cloudflare 挑战流程，完成后返回原生界面。

## 从源码运行

需要 **DevEco Studio 6.1.1 / API 24 SDK**。工程的兼容下限是 **HarmonyOS API 23**。

```bash
git clone https://github.com/CrystalDavid/ShaobingCommunity.git
```

1. 用 DevEco Studio 打开仓库根目录，完成依赖同步。
2. 为本机设备或模拟器配置调试签名。
3. 选择 `entry` 模块与目标设备，点击 Run。
4. 进入首页浏览帖子；通过“我的”完成官方账号登录后使用搜索和发帖等功能。

也可以在 DevEco 配置好的终端中构建：

```powershell
hvigor assembleHap
```

构建环境和签名步骤见 [开发指南](docs/development.md)，安装与发布流程见 [发布指南](docs/release-testing.md)。当前源码版本为 **1.1.1**，发布进展见 [项目状态](docs/project-status.md)。

## 实现方式

```text
ArkUI 页面 → ViewModel / Repository → 同源网络会话
          → BBS1 HTML 解码（TaskPool）→ 领域模型 → 原生界面
```

业务界面由 ArkUI 绘制。网络层使用 ArkWeb 同源会话处理论坛登录与 Cloudflare，未就绪时由 RCP 回退；登录或挑战需要操作时显示官方网页。字体、颜色、通透材质与组件尺寸由共享设计系统管理。

| 路径 | 内容 |
| --- | --- |
| [entry](entry/) | 主应用模块、页面、组件、网络与数据层 |
| [AppScope](AppScope/) | 应用配置与资源 |
| [scripts](scripts/) | 原生页面范围和底栏视觉检查 |
| [docs](docs/index.md) | 架构、设计、开发与发布文档 |

## 开发检查

```powershell
powershell -ExecutionPolicy Bypass -File scripts/NoWebOutsideLogin.ps1
powershell -ExecutionPolicy Bypass -File scripts/CheckBottomTabVisualLock.ps1
```

- [架构](docs/architecture.md) · [UI 设计](docs/ui.md) · [性能](docs/performance.md)
- [测试说明](docs/testing.md) · [隐私政策](docs/privacy-policy.md)
