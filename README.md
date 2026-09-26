# WPS EDU ZCode 下载与登录

从 [Release v0.2.0](https://github.com/LiuYu757/wps-edu-zcode/releases/tag/v0.2.0) 下载对应文件：

| 系统 | 桌面版 | CLI |
| --- | --- | --- |
| macOS Apple 芯片 | `ZCode-Preview-3.14.3-mac-arm64.dmg` | `wps-edu-zcode-cli-0.2.0-darwin-arm64.tgz` |
| Windows x64 | `ZCode-Preview-3.14.3-win-x64.exe` | `wps-edu-zcode-cli-0.2.0-win-x64.tgz` |

桌面版安装后，进入 **设置 → 模型设置 → WPS Comate → 登录 WPS Comate**，在弹出的 WPS 官方网页完成登录。

CLI 下载后在文件所在目录安装：

```sh
# macOS
npm install -g ./wps-edu-zcode-cli-0.2.0-darwin-arm64.tgz
```

```powershell
# Windows PowerShell
npm install -g .\wps-edu-zcode-cli-0.2.0-win-x64.tgz
```

CLI 有两种登录方式，任选一种：

1. **网页登录：**运行 `wps-zcode login wps`，在弹出的 WPS 官方网页登录。CLI 可独立使用，不需要安装桌面版。
2. **手动配置：**在已登录的 WPS Comate 网页中，打开 Chrome / Edge 开发者工具的 **Application → Cookies → `https://comate.wps.cn`**，复制 `wps_sid` 的 **Value**。将该值写入下述个人配置文件中 WPS 供应商的 `config.access.apiKey`，只填值，不加 `wps_sid=`。

个人配置文件：macOS 为 `~/.zcode/v2/provider_config.json`；Windows 为 `%USERPROFILE%\.zcode\v2\provider_config.json`。首次手动配置可使用以下内容，将 `<wps_sid>` 换成自己的值：

```json
{
  "schemaVersion": 1,
  "config": {
    "providerConfigRules": {
      "providerRules": [{
        "providerId": "wps-comate",
        "templateId": "wps-comate",
        "providerName": "WPS Comate",
        "config": {
          "group": "standard-personal",
          "access": {"type": "api-key", "apiKey": "<wps_sid>"}
        }
      }]
    },
    "modelConfigRules": {"providerModelRules": [], "manualProviderModelRules": []},
    "defaultModelSelection": {"providerId": "wps-comate", "modelId": "deepseek-v4-flash"}
  }
}
```

已有配置文件时，只更新 WPS 供应商的 `apiKey` 和所需的 `defaultModelSelection`，保留其他字段。运行 `wps-zcode --prompt "你好"` 测试。会话值属于个人登录凭据，勿提交或分享配置文件。模型请求由 ZCode 直接发送到 Comate，不需要本地桥接服务。

若选择网页登录，登录后在同一配置文件的 `config` 中设置 `"defaultModelSelection": {"providerId": "wps-comate", "modelId": "deepseek-v4-flash"}`；若登录命令输出的供应商 ID 不同，请替换 `providerId`。


## 使用声明与限制

本仓库是基于 ZCode 的非官方技术交流发行版，仅提供二进制与说明；修改后的源码目前未公开。
维护者不提供 WPS Comate 账号、额度或 API 服务，也不鼓励将个人会话用于反向代理、共享或出售 API。
请只使用自己有权访问的账号与模型，遵守 WPS Comate 适用条款；妥善保管 `wps_sid`，不要提交或转发个人配置。
Comate 网页接口、模型名单、额度和登录状态可能变化，功能不保证持续可用。Windows 包尚未经过实机验证。
第一方代码采用 [Apache-2.0](LICENSE)，第三方版权和许可见 [NOTICE.md](NOTICE.md) 与 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。上述倡议不修改许可证授予的权利。
