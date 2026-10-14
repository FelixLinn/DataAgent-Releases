# DataAgent 快速安装与部署

把下面的安装指令整体复制给能操作你电脑终端或浏览器的 Agent（例如 Codex），让它完成环境检查、下载安装和启动验收。无需把 DataAgent 源代码交给 Agent，也不需要提前安装 Python、Java、ZooKeeper 或 DeepSeek Harness 应用。

你需要一个有权访问 [DataAgent 私有发行仓库](https://github.com/FelixLinn/DataAgent-Releases) 的 GitHub 账号，并已接受仓库邀请。登录、模型密钥和更新凭据由你在本机填写，不要粘贴到聊天中。

截至 2026 年 10 月 14 日，[v0.2.0 下载页](https://github.com/FelixLinn/DataAgent-Releases/releases/tag/v0.2.0) 提供 **macOS Apple Silicon（M 系列）0.2.0** 和 **Windows x64 0.2.0-preview.1** 的完整安装包，大小分别约 1.52 GB 和 1.30 GB。Windows 包已通过 Windows CI 与 Windows 10 实机安装、托管服务启动和重复启动验证。Linux 与 Intel Mac 暂无已验收的可下载包。

## 复制给 Agent 的安装指令

完整复制以下文本。默认使用本地自动安装模式；如果你有现成的 IGinX，可在末尾补充“连接已有 IGinX”，以及不含密码的主机地址和端口。

```text
请在这台电脑上安装并启动 DataAgent，使用官方完整发行包，完成实际检查后告诉我如何再次打开。

发行仓库：https://github.com/FelixLinn/DataAgent-Releases
版本列表：https://github.com/FelixLinn/DataAgent-Releases/releases
快速说明：https://github.com/FelixLinn/DataAgent-Releases/blob/main/QUICKSTART.md

我的默认选择：安装到当前用户目录；使用默认用户数据目录；自动安装本地 IGinX；保留已有项目、会话和配置。仅进行安装与只读验收，不发起治理任务或付费模型请求。

请按以下步骤执行，不要只提供建议：

1. 检查操作系统、CPU 架构、可用磁盘及已有 DataAgent 安装。预留至少 20 GB 空间用于下载、解压、安装和运行时展开。已有安装时先识别其版本和数据目录；如果同版本已正常运行，直接验证并复用。不要覆盖同名但不属于 DataAgent 的目录，不自动迁移源码工作区数据。

2. 查询私有仓库实际发布的版本、说明和附件，选择已发布下载页中支持本机平台且已验收的安装包，不用草稿附件。v0.2.0 下载页包含 DataAgent-0.2.0-macos-arm64.zip 和 DataAgent-0.2.0-preview.1-windows-x64.zip；Windows 包内版本为 0.2.0-preview.1，不要重命名为 0.2.0。其他平台是否可安装，以当前真实附件和发行说明为准。找不到匹配包时明确说明并停止安装，不猜测下载地址，不改用其他架构包，也不擅自转为源码编译。

3. 优先复用已授权的 GitHub CLI 下载；未登录时引导我在浏览器完成 gh auth login。没有 CLI 时可使用浏览器授权下载；若你的工具无法操作浏览器，说明我应下载哪两个文件、放到哪里，随后继续。没有仓库权限时让我接受邀请或联系维护者，不把 404 当成软件包损坏。不要索取聊天中的访问令牌，不回显、共享或把凭据写进脚本。

4. 将同一下载页的目标 ZIP 和对应校验文件下载到单独目录（macOS 为 SHA256SUMS，Windows 为 SHA256SUMS-windows-x64），核验对应文件的 SHA256，再完整解压到该下载目录。核验不通过就停止，不继续运行。不要把下载目录放在最终安装目录里。macOS 使用 shasum -a 256，Linux 使用 sha256sum，Windows 使用 Get-FileHash -Algorithm SHA256。不要执行未经查看的远程 curl | sh 脚本。

5. 使用解压目录随包提供的安装脚本：macOS 为 sh DataAgent/install.command；Windows 为 DataAgent\install.cmd；Linux 为 sh DataAgent/install.sh。只在平台匹配时运行。安装脚本使用包内 Python，不需要自行安装系统 Python、Java、Node、Git、ZooKeeper 或 DSH 桌面应用。正常安装使用默认路径，不把验收专用的 --data-dir 参数传给安装器。如果已有治理任务在运行、排队或等待审阅，保留现场并等任务结束，不强制停止或删除锁文件。

6. 打开首次设置页。除非我明确提供已有服务，否则选择“自动安装本地 IGinX”，让程序管理随包依赖、ZooKeeper、IGinX 和存储。若我选择已有服务，则核实地址、端口、账号及存储是否可用，不启动替代引擎，不改写远端配置；需要密码时由我在本机表单中输入。端口被占用时使用程序报告的实际端口，不终止其他进程，不把工作台地址固定为 8765。只部署本机访问，不新增公网监听。

7. 需要模型凭据时，让我在首次设置页或受支持的本机隐藏输入入口自行填写。不读取其他应用的密钥，不把密钥放到命令参数或报告。如果我暂不配置，仍完成可执行的安装、数据服务与工作台验收，并明确标为“模型尚未配置”，不要报告 Agent 已可正常调用模型。

8. 验证安装版本、实际工作台地址、页面可打开、后端确实连接 IGinX，并在界面中查看数据项目页。新安装没有项目是正常现象。再次打开启动入口，确认复用已有服务而不是重复启动。若只能完成部分检查，逐项说明已通过、未通过和需要我操作的步骤；没有运行完整 /govern 就不要声称治理全链已验收。不要为验收修改业务数据。

9. 告诉我后续可在“帮助 → 应用更新”中授权自己的 GitHub 凭据，检查、下载并重启安装兼容更新；GitHub CLI 登录不会自动授权应用更新。当前没有更新或尚未授权不代表安装失败。运行中任务会阻止重启安装，下载可以先完成。不要把维护者凭据内置进程序。

10. 最后给出：已安装版本和平台、程序目录、用户数据目录、实际访问地址、下次启动方式、验收结果和剩余步骤。出错时给出具体阶段、脱敏错误及日志位置，保留用户数据，不通过清空目录重试，不关闭系统全局安全保护。macOS 若因尚未公证而阻止打开，交由我按系统提示或组织流程处理。

请现在开始检查。只有在缺少账号授权、需要我输入凭据、没有匹配安装包或必须由我作出选择时才暂停相应步骤，其余可独立完成的检查请继续。
```

## 需要你亲自完成的操作

Agent 可以完成下载和安装操作，但 GitHub 登录、仓库邀请、密钥输入和系统安全确认可能需要你在本机完成。如果 Agent 没有浏览器操作能力，让它打开首次设置地址，你填写后告诉它继续验收即可。

模型凭据可以稍后配置。安装成功与模型可用是两个验收结果；“IGinX 已连接”也不表示已经完成一次数据治理。

## 安装后的目录和入口

以下是默认路径；已有安装或显式选择自定义路径时，以 Agent 实际核验的结果为准。Windows 和 macOS 已有下载包；Linux 行用于对应发行包可用后的安装。

| 系统 | 程序目录 | 用户数据目录 | 再次启动 |
| --- | --- | --- | --- |
| macOS | `~/Applications/DataAgent/` | `~/Library/Application Support/DataAgent/` | 用户“应用程序”中的 `DataAgent.app` |
| Windows | `%LOCALAPPDATA%\Programs\DataAgent\` | `%LOCALAPPDATA%\DataAgent\` | 开始菜单中的 DataAgent |
| Linux | `~/.local/opt/dataagent/` | `${XDG_DATA_HOME:-~/.local/share}/dataagent/` | 应用菜单，或 `~/.local/bin/dataagent` |

Linux 未设置 `XDG_DATA_HOME` 时，默认使用 `~/.local/share/dataagent/`。用户数据下的 `projects/` 保存项目和 Git 历史，`web/` 保存会话与工作台状态，`imports/` 保存导入文件，`services/` 保存本地引擎数据和配置。连接外部 IGinX 时，远端引擎数据仍由原服务管理。

程序更新保留独立用户数据目录。源码版原有的 `data-projects/` 和 `.dataagent-runtime/` 不会自动搬到安装版；需要迁移时应单独安排。

## 手动下载命令

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

### macOS Apple Silicon

以下示例适用于 **macOS Apple Silicon 的 0.2.0**。已安装 GitHub CLI 且接受仓库邀请后，在终端逐步执行：

```sh
gh auth login
mkdir -p DataAgent-download-v0.2.0
cd DataAgent-download-v0.2.0
gh release download v0.2.0 --repo FelixLinn/DataAgent-Releases --pattern 'DataAgent-0.2.0-macos-arm64.zip' --pattern SHA256SUMS
shasum -a 256 -c SHA256SUMS
```

确认校验结果为 `OK` 后再执行：

```sh
unzip DataAgent-0.2.0-macos-arm64.zip
sh DataAgent/install.command
```

私有附件不能匿名 `curl` 下载。没有 GitHub CLI 时，可以登录 [v0.2.0 下载页](https://github.com/FelixLinn/DataAgent-Releases/releases/tag/v0.2.0)，下载 ZIP 与校验文件后继续安装。

详细配置和排障见[部署说明](https://oxlh5mrwi0.feishu.cn/docx/Bp5udrz0gooNWnxNYmucfNI9nPg)，使用流程见[用户手册](https://oxlh5mrwi0.feishu.cn/docx/T8ZTd0bptoNyzJxlFRHcpZkNnQZ)，授权更新见[应用更新](README.md#后续更新)。
