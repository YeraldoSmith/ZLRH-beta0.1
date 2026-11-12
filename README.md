# ZLRH-beta0.1

**ZLangRH（`zlrh`）私人 Beta 安装包** —— 本地大模型「蒸馏 + 融合」工具：把在线 API 或本地大模型的能力蒸馏进小模型、
按能力权重融合多个模型、在 MacBook 上轻量训练、和模型对话（含 LM Studio 的 GGUF 模型）、生图与生成视频、像应用商店一样搜索下载模型、
把本地模型接入 OpenClaw 等工具，以及用“斗兽场”让模型在纯内存的封闭环境里攻防评测。

Copyright © 2026 **Yeraldo Smith**（GitHub：YeraldoSmith · Hugging Face：Yeraldo）。保留所有权利。

> 本仓库只发布安装包，不含源代码。使用前请阅读 [`EULA.md`](EULA.md)（用户协议与版权声明），
> 程序首次运行时也会显示并要求你输入“同意”。未经作者许可，请勿转发或公开传播安装包。

## 最新版本：v0.7.3（beta0.3）

位于 [`releases/0.7.3-beta0.3/`](releases/0.7.3-beta0.3/)，更新内容见 [`releases/latest.json`](releases/latest.json) 的 notes：

- **交互向导重做**：菜单分组、Esc 返回上一级、输入当场校验并说明原因；每个操作结束后结果停留在屏幕上，按回车才回到菜单；
- **模型商店**：`zlrh search` 与向导里像应用商店一样搜索、分类、排序、看详情、判断本机内存放不放得下、一键下载；
- **生图 / 生成视频**：`zlrh image` / `zlrh video`（SD-Turbo、SDXL、SSD-1B、FLUX、Wan2.1 等），针对 Mac 小内存自动优化；
- **直接和 LM Studio 里的 GGUF / MLX 模型对话**，或把它们接入本地 API 网关；
- **0.7.2**：商店里选中模型后可以**选择下载格式**（原始权重 / MLX / GGUF …，并说明每种能做什么——消融、融合、训练要选原始权重）；
  斗兽场**一键备战**（按内存挑 2 个以上模型、下载后直接开打）；本机模型服务加随机访问密钥；
- **0.7.3**：斗兽场**上帝视角**（实时看到每个模型的思考、策略、命令与结果，每轮局势面板，赛后复盘）；
  **后台模型管理**（`zlrh ps` 查看正在运行的模型与内存，`zlrh stop` 关闭，向导主菜单「🧹 后台模型管理」）。

| 文件 | 适用设备 |
|---|---|
| `zlrh-0.7.3-beta0.3-macos-arm64.tar.gz` | **MacBook Air / Pro（M1~M4）** |
| `zlrh-0.7.3-beta0.3-macos-x86_64.tar.gz` | Intel 芯片的 Mac |
| `zlrh-0.7.3-beta0.3-linux-x86_64.tar.gz` | Linux x86_64（静态链接，任何发行版） |
| `zlrh-0.7.3-beta0.3-windows-x86_64.zip` | Windows 10 / 11 64 位 |
| `SHA256SUMS` | 以上安装包的 SHA-256 校验值 |

旧版本保留在 `releases/0.6.0-beta0.2/`、`releases/0.5.0-beta0.1/`。

## 已经装了 0.6 或更新的版本：一条命令更新

```bash
zlrh update
```

程序会校验作者签名和 SHA-256、拒绝降级，然后替换自己；每天也会在后台检查一次新版本并提示。
更新后第一次运行 `zlrh` 会显示新版用户协议，阅读后输入 `同意`。

## 从 0.5（beta0.1）升级（只需手动这一次）

> 请**逐行**复制执行。Mac 的终端（zsh）默认不认行尾的 `#` 注释，所以这里的命令都不带注释。

```bash
cd ~/zlrh-beta
git pull
cd releases/0.7.3-beta0.3
shasum -a 256 -c SHA256SUMS --ignore-missing
tar -xzf zlrh-0.7.3-beta0.3-macos-arm64.tar.gz
cd zlrh-0.7.3-beta0.3-macos-arm64
sh install.sh
```

新开一个终端窗口，确认版本（应显示 `zlrh 0.7.3 (beta0.3)`），再运行 `zlrh`（用户协议有更新，会请你重新输入 `同意`）：

```bash
zlrh --version
zlrh
```

## 第一次安装（MacBook Air M4）

**第 1 步：拉取并校验**（最后一行必须显示 `…macos-arm64.tar.gz: OK`）。用 git 拉取的文件不会被 macOS 标记为“来自互联网”。

```bash
git clone --depth 1 https://github.com/YeraldoSmith/ZLRH-beta0.1.git ~/zlrh-beta
cd ~/zlrh-beta/releases/0.7.3-beta0.3
shasum -a 256 -c SHA256SUMS --ignore-missing
```

**第 2 步：解压并安装**到 `~/.local/bin`（不需要 sudo；询问是否加入 PATH 时直接回车）：

```bash
tar -xzf zlrh-0.7.3-beta0.3-macos-arm64.tar.gz
cd zlrh-0.7.3-beta0.3-macos-arm64
sh install.sh
```

**第 3 步：新开一个终端窗口**，确认版本后运行 `zlrh`，首次运行会显示用户协议，输入 `同意` 后进入中文向导：

```bash
zlrh --version
zlrh
```

之后的训练环境（MLX）、模型商店、生图、对话、本地 API 网关、定向蒸馏、上传 Hugging Face 等步骤见 [`INSTALL.md`](INSTALL.md)，
所有命令与参数见安装包里的 `MANUAL.md`。

## 卸载

```bash
sh ~/zlrh-beta/releases/0.7.3-beta0.3/zlrh-0.7.3-beta0.3-macos-arm64/uninstall.sh
```

## 安全

- 这个仓库是公开的，因此**自动更新不信任仓库本身**：版本清单 `releases/latest.json` 必须带有作者私钥的 Ed25519 签名（`latest.json.sig`），
  程序内嵌公钥校验，并逐一比对安装包与程序的 SHA-256；任何人改动仓库或劫持网络都无法让 `zlrh update` 安装未经签名的程序；
- 程序不含遥测；在线服务的 API Key 只从环境变量读取，不写入任何文件；
- 安装脚本不使用 sudo，安装前用 SHA-256 校验程序；macOS 版为 ad-hoc 签名（未经苹果公证，首次运行若被拦截见 INSTALL.md）；
- 斗兽场是纯内存模拟环境，模型输入的任何命令都不会在你的电脑上真正执行；
- 使用 LM Studio 里的模型时只读取它的模型目录、不修改任何文件；生成图片 / 视频请遵守所用模型的许可证。
