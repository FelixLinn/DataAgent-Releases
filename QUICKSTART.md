# DataAgent 快速安装与部署

把下面的安装指令整体复制给能执行本机终端命令的 Agent（例如 Codex），让它完成环境检查、下载安装和启动验收。安装默认通过命令行完成，不需要 computer use，也不依赖任何浏览器的登录状态。无需把 DataAgent 源代码交给 Agent，也不需要提前安装 Python、Java、ZooKeeper 或 DeepSeek Harness 应用。

你需要一个有权访问 [DataAgent 私有发行仓库](https://github.com/FelixLinn/DataAgent-Releases) 的 GitHub 账号，并已接受仓库邀请。登录、模型密钥和更新凭据由你在本机填写，不要粘贴到聊天中。

旧 macOS 0.2.0 和 Windows 0.2.0-preview.1 包存在托管 IGinX 监听所有网卡的问题，请选择修复后的 **0.2.1** 完整包。下载前确认 [v0.2.1 下载页](https://github.com/FelixLinn/DataAgent-Releases/releases/tag/v0.2.1) 已提供本机平台的附件及验收说明；没有匹配附件时停止安装。各平台分批完成构建，不能把一个平台的包用于另一个平台。Linux 与 Intel Mac 的可用性同样以实际发布并验收的附件为准。

## 复制给 Agent 的安装指令

完整复制以下文本。默认使用本地自动安装模式；如果你有现成的 IGinX，可在末尾补充“连接已有 IGinX”，以及不含密码的主机地址和端口。

```text
请在这台电脑上安装并启动 DataAgent，使用官方完整发行包，完成实际检查后告诉我如何再次打开。

发行仓库：https://github.com/FelixLinn/DataAgent-Releases
版本列表：https://github.com/FelixLinn/DataAgent-Releases/releases
快速说明：https://github.com/FelixLinn/DataAgent-Releases/blob/main/QUICKSTART.md

我的默认选择：安装到当前用户目录；使用默认用户数据目录；自动安装本地 IGinX；保留已有项目、会话和配置。仅进行安装与只读验收，不发起治理任务或付费模型请求。

请按以下步骤执行，不要只提供建议。默认只使用终端和 HTTP 接口，不调用 computer use，不自动打开浏览器或客户端窗口。浏览器用于用户自行登录或最后查看界面；没有浏览器操作能力不应阻塞命令行安装和验收。先读取快速说明中的最新安装约定，若发行说明要求避开旧版本，应遵循该要求。

1. 检查操作系统、CPU 架构、可用磁盘及已有 DataAgent 安装。预留至少 20 GB 空间用于下载、解压、安装和运行时展开。已有安装时先识别其版本和数据目录；如果同版本已正常运行，直接验证并复用。不要覆盖同名但不属于 DataAgent 的目录，不自动迁移源码工作区数据。

2. 先检查 gh --version 和 gh auth status --hostname github.com 的退出状态，再通过 gh api repos/FelixLinn/DataAgent-Releases --jq .full_name 核实当前 CLI 账号的仓库权限。已登录且有权访问时直接复用，不重新登录、不打开浏览器。浏览器、GitHub 连接器和 CLI 的登录状态互相独立；不要因为一个浏览器已登录就认定另一个浏览器或 CLI 已授权。认证失败、仓库无权访问、代理/VPN/网络失败要分别诊断，404 也可能表示没有私有仓库权限。

   没有 gh 时，优先通过已有包管理器或 GitHub CLI 官方安装包/便携包安装到本机，核实官方来源及校验信息；官方来源为 https://github.com/cli/cli；Windows 可用 winget install --id GitHub.cli --exact --source winget，安装后重新定位 gh，不要假设当前终端 PATH 已刷新。不要使用维护者令牌或去读浏览器/其他应用的凭据。缺少包管理器或无权安装时，才让用户自行获取官方 CLI 或下载 DataAgent ZIP 与校验文件。

   只有 CLI 确实未登录或凭据失效，才请我在本机终端执行 gh auth login --hostname github.com --git-protocol https，由我完成设备码/网页登录或本机凭据输入，随后重新检查 CLI 和仓库权限。设备授权可以由我在自己选择的任意浏览器完成，完成后以 CLI 检查为准；不要操作不确定的浏览器，不索取聊天中的账号密码、验证码或访问令牌，不执行 gh auth token，不回显、共享或把凭据写进脚本。没有仓库权限时让我接受邀请或联系维护者。

3. 查询私有仓库实际发布的版本、说明和附件，选择支持本机平台、通过验收且没有已知安装阻塞问题的最新正式发行包，不用草稿或 Acceptance/Failure 验收专用附件。包内版本与下载页标签可能不同，以安装清单为准，不自行重命名版本。不要默认选择旧的 v0.2.0：其中 macOS 0.2.0 和 Windows 0.2.0-preview.1 的托管 IGinX 存在监听所有网卡的问题，应选择发行说明明确已修复的新包。找不到匹配包时明确说明并停止安装，不猜测下载地址，不改用其他架构包，也不擅自转为源码编译。

4. 将同一下载页的目标 ZIP 和对应校验文件下载到单独目录（macOS 为 SHA256SUMS，Windows 为 SHA256SUMS-windows-x64），核验对应文件的 SHA256，再完整解压到该下载目录。核验不通过就停止，不继续运行。不要把下载目录放在最终安装目录里。macOS 使用 shasum -a 256，Linux 使用 sha256sum，Windows 使用 Get-FileHash -Algorithm SHA256。不要执行未经查看的远程 curl | sh 脚本。

5. 使用解压目录随包提供的安装脚本：macOS 为 sh DataAgent/install.command --no-launch；Windows 为 DataAgent\install.cmd --no-launch；Linux 为 sh DataAgent/install.sh --no-launch。--no-launch 避免安装后自动打开窗口；随后单独启动和配置。只在平台匹配时运行。安装脚本使用包内 Python，不需要自行安装系统 Python、Java、Node、Git、ZooKeeper 或 DSH 桌面应用。正常安装使用默认路径，不把验收专用的 --data-dir 参数传给安装器。如果已有治理任务在运行、排队或等待审阅，保留现场并等任务结束，不强制停止或删除锁文件。

6. 从已安装目录运行官方启动入口，传入 --no-open --progress-json，在后台保存并读取进度，不等待首次设置进程退出后才配置。各平台具体入口见快速说明的“无浏览器首次配置”。已有配置时启动器直接复用，读取 workbench/ready 事件中的实际 URL。全新安装时读取 setup/ready 事件的本机 URL，先 GET 该 URL 下的 state，确认是当前首次设置实例，再 POST configure，Content-Type 为 application/json；默认请求体仅为 {"mode":"managed"}。这是首次设置页使用的同一接口，不手写 services.json，不调用内部 Python 函数绕过设置。随后轮询 state，checking 时继续等待并报告真实进展；failed 时记录脱敏错误并停止验收；ready 时获取实际工作台 URL。启动器退出后不继续轮询已关闭的设置服务。设置 URL 含临时授权路径，仅在本机使用，不贴入聊天或最终报告。

   除非我明确提供已有服务，否则使用 managed，让程序管理随包依赖、ZooKeeper、IGinX 和存储。external 使用该接口的 host/port/username/password 字段，并经原有只读连接检查；密码由我在本机隐藏输入后在内存中构造请求，不放入命令行、请求文件、日志或聊天。无法提供可靠隐藏输入时才让我手动打开本机设置页填写，其他独立检查继续。不要读取其他应用密钥，不启动替代引擎，不改写远端配置。端口被占用时使用启动器报告的实际端口，不终止其他进程，不固定为 8765。只部署本机访问，不新增公网监听。

7. 需要模型凭据时，优先让我在本机终端运行启动入口 --no-open --configure-model kimi-coding 或 --no-open --configure-model deepseek-official，通过官方隐藏输入自行填写；也可由我选择使用首次设置页。不读取其他应用的密钥，不把密钥放到命令参数或报告。如果我暂不配置，仍完成可执行的安装、数据服务与工作台验收，并明确标为“模型尚未配置”，不要报告 Agent 已可正常调用模型。

8. 通过实际工作台 URL 的 GET /healthz 验证 service 为 dataagent-workbench、版本、PID 和实例身份，并与本次安装/数据目录的启动记录核对；不要仅凭端口有响应认定启动成功。GET /api/status 核实 IGinX 连接状态；GET /api/projects 检查项目列表是否能读取；GET / 验证前端资源可访问。新安装没有项目是正常现象。检查托管 ZooKeeper、IGinX 和存储进程的实际监听地址，必须是环回地址，不能仅凭配置 IP 或本机连接判定通过；若检查不通过，保留证据并停止继续启动，不自行修改官方包或系统安全保护。

   再用同一数据目录的官方启动入口 --no-open 启动一次，确认工作台和引擎 PID 被复用而非重复创建。区分首次配置与已有配置复用。命令行和接口验收不依赖浏览器；没有实际查看界面就标为“界面人工检查待完成”，不要报告截图或 GUI 验收通过。结束时给我实际工作台地址，由我决定在哪个浏览器打开；只有我明确要求才代为打开。若只能完成部分检查，逐项说明已通过、未通过和剩余步骤；没有运行完整 /govern 就不要声称治理全链已验收。不要为验收修改业务数据。

9. 告诉我后续可在“帮助 → 应用更新”中授权自己的 GitHub 凭据，检查、下载并重启安装兼容更新；GitHub CLI 登录不会自动授权应用更新。当前没有更新或尚未授权不代表安装失败。运行中任务会阻止重启安装，下载可以先完成。不要把维护者凭据内置进程序。

10. 最后给出：已安装版本和平台、程序目录、用户数据目录、实际访问地址、下次启动方式、验收结果和剩余步骤。出错时给出具体阶段、脱敏错误及日志位置，保留用户数据，不通过清空目录重试，不关闭系统全局安全保护。macOS 若因尚未公证而阻止打开，交由我按系统提示或组织流程处理。

请现在开始检查。只有在缺少账号授权、需要我输入凭据、没有匹配安装包或必须由我作出选择时才暂停相应步骤，其余可独立完成的检查请继续。
```

## 需要你亲自完成的操作

Agent 可以通过终端完成下载、安装、本地 IGinX 首次配置和接口验收。GitHub 未登录时，你在自己的终端完成 CLI 登录；设备码可在你选择的浏览器中授权，Agent 无需控制该浏览器。CLI 已登录时跳过此步骤。仓库邀请、密钥输入和系统安全确认仍可能需要你亲自完成。

有多个浏览器时，无需在它们之间寻找“已登录窗口”：下载使用已核验的 CLI 账号；首次配置使用启动器给出的本机接口；最后你自行打开实际工作台地址。若你偏好图形设置，可以选择手动打开首次设置页，这不是默认安装的前提。

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

先确认 v0.2.1 下载页已包含 Windows x64 附件。已接受仓库邀请并安装 GitHub CLI 的用户，在 PowerShell 中逐步执行。只有登录检查失败时，才先自行执行 `gh auth login --hostname github.com --git-protocol https`；已登录则直接下载：

```powershell
gh auth status --hostname github.com
New-Item -ItemType Directory -Path DataAgent-download-windows
Set-Location DataAgent-download-windows
gh release download v0.2.1 --repo FelixLinn/DataAgent-Releases --pattern 'DataAgent-0.2.1-windows-x64.zip' --pattern SHA256SUMS-windows-x64
Get-FileHash .\DataAgent-0.2.1-windows-x64.zip -Algorithm SHA256
Get-Content .\SHA256SUMS-windows-x64
```

逐字核对两处 SHA256，相同后完整解压，再执行安装：

```powershell
Expand-Archive .\DataAgent-0.2.1-windows-x64.zip -DestinationPath .\unpacked
.\unpacked\DataAgent\install.cmd --no-launch
```

下次可从开始菜单打开 DataAgent，也可在 PowerShell 执行：

```powershell
& "$env:LOCALAPPDATA\Programs\DataAgent\DataAgent.cmd"
```

首次启动会展开随包依赖，可能需要数分钟。后续启动会校验并复用已有服务。Windows 本机部署不需要为 Java 开放外部网络访问；防火墙提示可选择取消。

### macOS Apple Silicon

以下示例适用于 **macOS Apple Silicon 的 0.2.1**。已安装 GitHub CLI 且接受仓库邀请后，在终端逐步执行。只有登录检查失败时，才先自行执行 `gh auth login --hostname github.com --git-protocol https`；已登录则直接下载：

```sh
gh auth status --hostname github.com
mkdir -p DataAgent-download-v0.2.1
cd DataAgent-download-v0.2.1
gh release download v0.2.1 --repo FelixLinn/DataAgent-Releases --pattern 'DataAgent-0.2.1-macos-arm64.zip' --pattern SHA256SUMS
shasum -a 256 -c SHA256SUMS
```

确认校验结果为 `OK` 后再执行：

```sh
unzip DataAgent-0.2.1-macos-arm64.zip
sh DataAgent/install.command --no-launch
```

安装命令使用了 `--no-launch`，请继续执行下方无浏览器首次配置，或自行打开已安装的 DataAgent 入口进行图形设置。

私有附件不能匿名 `curl` 下载。没有 GitHub CLI 时，可以登录 [v0.2.1 下载页](https://github.com/FelixLinn/DataAgent-Releases/releases/tag/v0.2.1)，下载 ZIP 与校验文件后继续安装。

## 无浏览器首次配置（0.2.1 已支持）

先用 `--no-launch` 安装，再运行下列本机启动命令。它不打开浏览器；首次使用时进程会等待配置，**保持运行**，在另一个终端提交配置。Agent 可在后台启动并捕获进度。已有配置时会直接启动或复用工作台，无需再次 POST。

Windows PowerShell：

```powershell
& "$env:LOCALAPPDATA\Programs\DataAgent\DataAgent.cmd" --no-open --progress-json
```

macOS：

```sh
"$HOME/Applications/DataAgent/DataAgent.command" --no-open --progress-json
```

Linux（仅在本机平台有对应发行包时）：

```sh
"$HOME/.local/bin/dataagent" --no-open --progress-json
```

在进度中找到 `stage=setup`、`state=ready` 事件的 `url`。它形如 `http://127.0.0.1:实际端口/临时授权路径/`，不是固定 8765，也不是最终工作台地址。不要把这个临时 URL 发到聊天里；仅在自己的第二个终端输入。请求体 `{"mode":"managed"}` 不含密钥，使用与首次设置页相同的受控接口。

Windows PowerShell 第二个终端：

```powershell
$setupUrl = Read-Host '输入本机 setup/ready 事件的完整 URL（末尾保留 /）'
$state = Invoke-RestMethod -Uri ($setupUrl + 'state')
if ($state.state -ne 'setup') { throw '当前不是等待首次配置的实例，请检查启动进度' }
Invoke-RestMethod -Uri ($setupUrl + 'configure') -Method Post -ContentType 'application/json' -Body '{"mode":"managed"}'
Invoke-RestMethod -Uri ($setupUrl + 'state')
```

macOS/Linux 第二个终端：

```sh
read -r setup_url
curl --fail --silent --show-error "${setup_url}state"
# 确认 state 为 setup 后才执行下一条
curl --fail --silent --show-error -H 'Content-Type: application/json' \
  --data '{"mode":"managed"}' "${setup_url}configure"
curl --fail --silent --show-error "${setup_url}state"
```

`202 accepted` 仅表示配置已接受，不表示服务已就绪。在启动器仍运行时每隔 1～2 秒读取 `state`，并每隔约 30 秒报告阶段与耗时；不要将“没有新输出”误判为成功或无限等待而不说明。`state`：`checking` 表示依赖和服务启动中，`failed` 表示失败；`ready` 的 `url` 才是工作台地址。启动器也会输出 `workbench/ready` 事件，随即退出并关闭临时设置接口。若设置接口已关闭，读取启动进度中的最终事件，而不是重新提交配置。

随后对**实际工作台地址**（替换下面示例地址）只读验收。PowerShell：

```powershell
$workbenchUrl = Read-Host '输入 workbench/ready 事件的工作台 URL'
Invoke-RestMethod -Uri ($workbenchUrl.TrimEnd('/') + '/healthz')
Invoke-RestMethod -Uri ($workbenchUrl.TrimEnd('/') + '/api/status')
Invoke-RestMethod -Uri ($workbenchUrl.TrimEnd('/') + '/api/projects')
```

macOS/Linux：

```sh
read -r workbench_url
curl --fail --silent --show-error "${workbench_url%/}/healthz"
curl --fail --silent --show-error "${workbench_url%/}/api/status"
curl --fail --silent --show-error "${workbench_url%/}/api/projects"
```

检查 `healthz.service` 为 `dataagent-workbench`，版本为本次安装版本，PID/实例与启动记录一致；`status.backend` 为 `iginx`（表示会话认证与通信正常）；项目接口返回有效列表。还需按安装指令检查实际监听地址及重复启动复用。这些是接口检查，界面可由用户之后自行打开浏览器确认。受保护的已有工作台若返回 401，使用它已配置的官方登录方式，不关闭认证。

连接已有 IGinX 时，配置接口接受 `mode=external` 和 `host`、`port`、`username`、`password`。密码只能由用户本机隐藏输入后在内存中提交；上面的命令示例不应添加明文密码。模型可暂不配置，之后用启动入口 `--no-open --configure-model kimi-coding` 或 `--no-open --configure-model deepseek-official` 在本机隐藏输入。

详细配置和排障见[部署说明](https://oxlh5mrwi0.feishu.cn/docx/Bp5udrz0gooNWnxNYmucfNI9nPg)，使用流程见[用户手册](https://oxlh5mrwi0.feishu.cn/docx/T8ZTd0bptoNyzJxlFRHcpZkNnQZ)，授权更新见[应用更新](README.md#后续更新)。
