# WPS Comate 版 ZCode

本发行版提供 **macOS Apple Silicon（arm64）** 和 **Windows x64** 的 ZCode Preview 桌面版与 `wps-zcode` 命令行版。两者使用个人网页登录取得的 `wps_sid` Cookie，直接向 WPS Comate 发起模型请求；使用时无需运行本地桥接服务或网页服务。

本仓库为私有二进制发行仓库。Git 仓库只保存本 README 和许可、第三方声明；安装包放在 [v0.1.0 Release](https://github.com/LiuYu757/wps-edu-zcode/releases/tag/v0.1.0)。仓库及安装包不包含个人 Cookie、个人配置或项目源码。请使用有权访问此私有仓库的 GitHub 账号，在浏览器中下载所需安装包。

| 系统 | 桌面版 | CLI npm 包 |
| --- | --- | --- |
| macOS arm64 | `ZCode-Preview-3.14.3-mac-arm64.dmg` | `wps-edu-zcode-cli-0.1.0-darwin-arm64.tgz` |
| Windows x64 | `ZCode-Preview-3.14.3-win-x64.exe` | `wps-edu-zcode-cli-0.1.0-win-x64.tgz` |

## 安装桌面版

### macOS arm64

1. 从 [v0.1.0 Release](https://github.com/LiuYu757/wps-edu-zcode/releases/tag/v0.1.0) 下载 `ZCode-Preview-3.14.3-mac-arm64.dmg`，双击打开，并把 **ZCode Preview.app** 拖入“应用程序”文件夹。
2. 启动 **ZCode Preview**。首次引导如要求选择 API Key，可以选择“暂时跳过”，然后按下文配置 WPS Comate。

macOS 应用使用临时（ad hoc）签名，**未经过 Apple 公证**。首次打开时，系统可能显示来源限制；请先核对 Release 下载文件及校验值，再按系统“隐私与安全性”页面的提示决定是否打开。此 Preview 使用独立应用身份，不会覆盖正式版 ZCode。

### Windows x64

1. 从同一 Release 下载 `ZCode-Preview-3.14.3-win-x64.exe` 和 `SHA256SUMS`，按下文核对 SHA-256 校验值。
2. 运行安装包，按安装向导完成安装，然后启动 **ZCode Preview**。如果 Windows SmartScreen 提示未知发布者，请先核对安装包来源和校验值，再决定是否运行。
3. 首次引导如要求选择 API Key，可以选择“暂时跳过”，然后按下文配置 WPS Comate。

Windows 安装包目前未做代码签名，因此 Windows SmartScreen 可能提示未知发布者。当前尚未在真实 Windows 机器上验证桌面端启动、模型连接或 Comate 请求。

## 获取 WPS 登录 Cookie 并配置模型

1. 在 Chrome 或 Edge 打开 [WPS Comate 网页版](https://comate.wps.cn/web?source_from=official_website)，登录自己的账号，并确认网页中的模型可用。
2. 打开开发者工具：macOS 按 `⌘⌥I`，Windows 按 `Ctrl+Shift+I`；依次进入 **应用（Application）→ 存储（Storage）→ Cookie → `https://comate.wps.cn`**。
3. 在 Cookie 表中筛选 `wps_sid`，找到名称为 `wps_sid`、域名为 `.wps.cn` 的记录，只复制其 **Value**。即使该 Cookie 标记为 `HttpOnly`，仍可在开发者工具中查看。不要复制整行、其他 Cookie、请求头或截图。
4. 在 **ZCode Preview → 设置 → 模型设置 → 添加供应商 → WPS Comate** 中，将该 Value 填入 **wps_sid Cookie**。若已经添加供应商，直接打开其设置。输入框也接受 `wps_sid=<值>`；不要附带其他键值或分号。离开输入框后设置会保存。
5. 在模型列表中，对 `deepseek-v4-flash` 点击 **测试模型**。连接成功后，回到工作区，在模型菜单选择 **WPS Comate → deepseek-v4-flash**，发送一条消息验证对话。

`wps_sid` 是登录凭据。ZCode 以明文将它保存在本机个人供应商配置中：macOS 为 `~/.zcode/v2/provider_config.json`，Windows 为 `%USERPROFILE%\.zcode\v2\provider_config.json`。macOS 文件权限 `0600` 已在本机验证；Windows 文件访问权限尚未实测，请确保该文件只允许自己的账号读取。配置不会随安装包发布。不要把凭据写入 README、终端命令、Git 提交、聊天消息或截图。Cookie 过期后，重新登录 Comate 网页并按上述步骤更新；本版本不会自动续期。

桌面版内置 Comate 模型短名称。**在新电脑上只填写 Cookie，还不足以调用这些短名称。** 请先安装并登录 WPS Comate 桌面端，让其生成本机模型目录：macOS 为 `~/.wpscomate/config.json`，Windows 预计为 `%USERPROFILE%\.wpscomate\config.json`。ZCode 会只读该目录，将短名称映射到当前账号的上游模型 ID。没有该目录时，也可在 ZCode 中手动添加账号可用的完整模型 ID。具体模型是否可用取决于 WPS 账号权限；这一步不需要运行本地桥接服务。Windows 的目录位置和读取行为尚待实机核实。

## 安装并配置 CLI

在已登录 GitHub 且有权访问本仓库的浏览器中，从 [v0.1.0 Release](https://github.com/LiuYu757/wps-edu-zcode/releases/tag/v0.1.0) 下载与系统匹配的 `.tgz`。私有仓库的下载需要 GitHub 认证，因此先下载到本机，再用 npm 安装本地包。安装前确认 `node --version` 和 `npm --version` 均可正常运行。

macOS 终端：

```sh
npm install -g ~/Downloads/wps-edu-zcode-cli-0.1.0-darwin-arm64.tgz
wps-zcode --version
```

Windows PowerShell：

```powershell
npm install -g "$HOME\Downloads\wps-edu-zcode-cli-0.1.0-win-x64.tgz"
wps-zcode --version
```

每个 npm 包仅含相应系统的可执行文件、包元数据和许可文档；不包含 JavaScript 源码、`node_modules` 或个人配置。安装后命令为 `wps-zcode`。发行包版本为 `0.1.0`；macOS CLI 实测 `wps-zcode --version` 显示上游 CLI 版本 `0.16.9`，Windows CLI 由同一源码构建但版本输出尚未在 Windows 实机验证。`wps-zcode` 在本机直接连接 Comate，桌面应用无需保持运行。可运行 `wps-zcode --licenses` 查看二进制内嵌的第三方许可声明。

CLI 默认读取桌面版创建的个人供应商配置，共用已经填写的 WPS Comate 供应商与 Cookie。要通过配置文件选择 WPS 模型，macOS 用文本编辑器打开 `~/.zcode/v2/provider_config.json`；Windows 可在 PowerShell 运行 `notepad "$HOME\.zcode\v2\provider_config.json"`。在现有的 `config` 对象中设置 `defaultModelSelection`：

```json
"defaultModelSelection": {
  "providerId": "wps-comate",
  "modelId": "deepseek-v4-flash",
  "options": { "reasoningLevel": "disabled" }
}
```

这只是需要修改的字段，**不是完整配置文件**。保留文件中的 `schemaVersion`、`providerConfigRules`、`modelConfigRules` 和已有凭据。如果你添加的供应商 ID 带数字后缀，请使用文件中对应的实际 ID。选择其他模型时，也要使用该模型支持的 `reasoningLevel`。文件含明文 Cookie，编辑后确保只有本人可读写，不要将它放入仓库或分享。更新 Cookie 可回到桌面版设置操作。

测试 CLI 的非交互对话，在 macOS 终端或 Windows PowerShell 中运行：

```text
wps-zcode --prompt "请只回复：OK" --output-format text
```

直接运行 `wps-zcode` 可进入交互式终端界面。非交互提示词模式可能允许 Agent 执行工具；测试时使用不涉及文件修改的简单提示词，正式任务前检查命令和权限设置。**Windows CLI 的启动与 Comate 请求尚未在真实 Windows 机器上验证。**

## 校验与排障

Release 同时提供 `SHA256SUMS`。将所需安装包和校验文件下载到同一目录。

macOS 终端；下载两个 macOS 安装包和校验文件后，运行以下命令检查这两个安装包：

```sh
cd ~/Downloads
grep -E '(mac-arm64\.dmg|darwin-arm64\.tgz)$' SHA256SUMS | shasum -a 256 -c -
```

Windows PowerShell 可分别查看已下载文件的 SHA-256，并与 `SHA256SUMS` 中的同名文件记录比对（十六进制字母忽略大小写）：

```powershell
Get-FileHash "$HOME\Downloads\ZCode-Preview-3.14.3-win-x64.exe" -Algorithm SHA256
Get-FileHash "$HOME\Downloads\wps-edu-zcode-cli-0.1.0-win-x64.tgz" -Algorithm SHA256
```

2026-09-26，macOS 桌面版已在本机完成 `deepseek-v4-flash` 模型连接、正常对话、只读 `pwd` 工具调用，以及退出重启后的再次连接测试；应用已通过 `codesign --verify --deep --strict`。macOS CLI 的最终二进制 npm 包已在隔离目录安装，安装后的命令完成真实 WPS 对话；模拟新版上游目录移除 WPS 模板后，仍可调用该模型。**Windows 桌面版和 CLI 尚未在真实 Windows 系统上完成启动、安装或 Comate 请求测试。** 其他模型以及 Cookie 过期后的重新登录尚未逐项验证。

- **401 / `not_login`**：网页登录状态可能失效。先确认 Comate 网页可用，再更新 `wps_sid`。
- **403 / 权限不足**：在 Comate 网页确认当前账号有权使用所选模型。
- **模型别名无法解析**：确认本机 Comate 模型目录可用，或在 ZCode 中填写完整上游模型 ID。
- **400、超时或其他请求错误**：先核对模型、账号权限和网络。Comate 使用非公开接口，上游行为变化也可能导致失败。

排障记录中只保留时间、模型名、状态码和去除凭据后的错误信息。

## 许可与来源

本发行版基于 [zai-org/ZCode](https://github.com/zai-org/ZCode) 3.14.3，其第一方代码使用 Apache-2.0，见 [LICENSE](LICENSE)、[NOTICE.md](NOTICE.md) 和 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。Comate 请求约定参考 [bakasbk/dsh-connect-comate](https://github.com/bakasbk/dsh-connect-comate)，其 MIT 许可文本见 [LICENSE.dsh-connect-comate](LICENSE.dsh-connect-comate)。

WPS Comate 的网页接口不是公开稳定 API，WPS 后续调整可能影响本发行版。此发行版不是 WPS 官方产品。
