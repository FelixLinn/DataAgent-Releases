# DataAgent 安装与更新

这是供授权内部用户下载 DataAgent 的私有发行仓库，源代码在独立仓库维护。

## 让自己的 Agent 帮你安装

打开[快速安装与部署](QUICKSTART.md)，将完整指令复制给能操作本机的 Agent。它会检查平台、授权下载安装包、配置数据服务并验收；登录和密钥由你在本机完成。

## 下载与安装

[下载 DataAgent 0.2.0](https://github.com/FelixLinn/DataAgent-Releases/releases/tag/v0.2.0)

当前提供 **macOS Apple Silicon（M 系列芯片）0.2.0** 和 **Windows x64 0.2.0-preview.1** 完整安装包。Windows 包已通过 Windows CI 与 Windows 10 实机安装、服务启动和重复启动验收。Linux 与 Intel Mac 暂无已验收的下载包。

1. 接受仓库协作者邀请，使用自己的 GitHub 账号登录后打开上面的版本页。
2. 按平台下载 ZIP 与校验文件：macOS 为 `DataAgent-0.2.0-macos-arm64.zip`、`SHA256SUMS`；Windows 为 `DataAgent-0.2.0-preview.1-windows-x64.zip`、`SHA256SUMS-windows-x64`。校验后完整解压。
3. macOS 双击 `DataAgent/install.command` 或在终端执行 `sh DataAgent/install.command`；Windows 双击 `DataAgent\install.cmd` 或在 PowerShell 执行。安装到当前用户目录，不需要系统管理员权限。
4. 首次启动选择自动安装本地 IGinX，或填写已有 IGinX 服务的连接信息。运行依赖随包提供，无需自行安装 Python、Java、ZooKeeper 等。
5. 配置自己的模型 API 凭据后，选择数据项目，使用 `/govern` 发起治理。

macOS 包使用 ad-hoc 签名，尚未完成 Apple 公证；Windows 包尚未完成 Authenticode 签名。系统可能要求用户确认打开。安装目录与用户数据目录分开，首次安装不自动导入开发工作区的数据。

## 终端下载安装

### macOS Apple Silicon

已安装 GitHub CLI 的用户先运行 `gh auth login`，登录有权访问此仓库的账号，再逐步执行：

```sh
mkdir -p DataAgent-download
cd DataAgent-download
gh release download v0.2.0 --repo FelixLinn/DataAgent-Releases --pattern 'DataAgent-0.2.0-macos-arm64.zip' --pattern SHA256SUMS
shasum -a 256 -c SHA256SUMS
unzip DataAgent-0.2.0-macos-arm64.zip
sh DataAgent/install.command
```

校验失败时不要继续安装。私有附件不能匿名 `curl` 下载；没有 GitHub CLI 也可以通过浏览器登录下载。

### Windows x64

已接受仓库邀请并安装 GitHub CLI 的用户，在 PowerShell 中逐步执行：

```powershell
gh auth login
New-Item -ItemType Directory -Path DataAgent-download-windows
Set-Location DataAgent-download-windows
gh release download v0.2.0 --repo FelixLinn/DataAgent-Releases --pattern 'DataAgent-0.2.0-preview.1-windows-x64.zip' --pattern SHA256SUMS-windows-x64
Get-FileHash .\DataAgent-0.2.0-preview.1-windows-x64.zip -Algorithm SHA256
Get-Content .\SHA256SUMS-windows-x64
```

逐字核对两处 SHA256，相同后完整解压，再执行安装：

```powershell
Expand-Archive .\DataAgent-0.2.0-preview.1-windows-x64.zip -DestinationPath .\unpacked
.\unpacked\DataAgent\install.cmd
```

下次可从开始菜单打开 DataAgent，也可在 PowerShell 执行：

```powershell
& "$env:LOCALAPPDATA\Programs\DataAgent\DataAgent.cmd"
```

首次启动会展开随包依赖，可能需要数分钟。后续启动会校验并复用已有服务。Windows 本机部署不需要为 Java 开放外部网络访问；防火墙提示可选择取消。

## 后续更新

进入 **帮助 → 应用更新**，使用自己的 GitHub 访问令牌授权此仓库，然后检查新版本、下载并重启安装。应用不会读取 GitHub CLI 的凭据，也不包含维护者令牌。

发行方发布兼容的新版本和更新清单后，已安装应用即可获取更新。治理任务运行中不能重启安装；项目、会话和配置保存在独立用户目录，新版本启动失败时尝试恢复旧程序与更新前状态。运行依赖、内置算子或数据格式迁移需要专门升级流程。

应用帮助菜单包含[部署说明](https://oxlh5mrwi0.feishu.cn/docx/Bp5udrz0gooNWnxNYmucfNI9nPg)和[用户手册](https://oxlh5mrwi0.feishu.cn/docx/T8ZTd0bptoNyzJxlFRHcpZkNnQZ)。Windows 安装流程和最新版本状态见[快速安装与部署](QUICKSTART.md)；安装包内也附有本地文档。

Windows 的程序目录为 `%LOCALAPPDATA%\Programs\DataAgent\`，数据目录为 `%LOCALAPPDATA%\DataAgent\`；macOS 数据目录为 `~/Library/Application Support/DataAgent/`。首次展开依赖可能需要数分钟，后续启动会复用自己的服务。Windows 实机尚未配置模型，因此安装与运行验收不代表已完成模型驱动 `/govern` 验收。

## 访问权限

只有被授权且接受邀请的 GitHub 用户可以下载。个人私有仓库的协作者同时具有写权限，仅向可信内部人员开放。不要上传个人凭据、业务数据或用户配置。
