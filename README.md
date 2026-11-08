# ZLRH-beta0.1

**ZLangRH（`zlrh`）私人 Beta 安装包** —— 本地大模型「蒸馏 + 融合」工具：把在线 API 或本地大模型的能力蒸馏进小模型、
按能力权重融合多个模型、在 MacBook 上做轻量训练，以及用“斗兽场”让多个模型在纯内存的封闭环境里攻防评测。

Copyright © 2026 **Yeraldo Smith**（GitHub：YeraldoSmith · Hugging Face：Yeraldo）。保留所有权利。

> 本仓库只发布安装包，不含源代码。使用前请阅读 [`EULA.md`](EULA.md)（用户协议与版权声明），
> 程序首次运行时也会显示并要求你输入“同意”。未经作者许可，请勿转发或公开传播安装包。

## 安装包（v0.5.0 · beta0.1）

位于 [`releases/0.5.0-beta0.1/`](releases/0.5.0-beta0.1/)：

| 文件 | 适用设备 |
|---|---|
| `zlrh-0.5.0-beta0.1-macos-arm64.tar.gz` | **MacBook Air / Pro（M1~M4）** |
| `zlrh-0.5.0-beta0.1-macos-x86_64.tar.gz` | Intel 芯片的 Mac |
| `zlrh-0.5.0-beta0.1-linux-x86_64.tar.gz` | Linux x86_64（静态链接，任何发行版） |
| `zlrh-0.5.0-beta0.1-windows-x86_64.zip` | Windows 10 / 11 64 位 |
| `SHA256SUMS` | 以上安装包的 SHA-256 校验值 |

## MacBook Air M4 快速安装（终端）

> 请**逐行**复制执行。Mac 的终端（zsh）默认不认行尾的 `#` 注释，所以这里的命令都不带注释。

**第 1 步：拉取并校验**（最后一行必须显示 `…macos-arm64.tar.gz: OK`）。用 git 拉取的文件不会被 macOS 标记为“来自互联网”。

```bash
git clone --depth 1 https://github.com/YeraldoSmith/ZLRH-beta0.1.git ~/zlrh-beta
cd ~/zlrh-beta/releases/0.5.0-beta0.1
shasum -a 256 -c SHA256SUMS --ignore-missing
```

**第 2 步：解压并安装**到 `~/.local/bin`（不需要 sudo；询问是否加入 PATH 时直接回车）：

```bash
tar -xzf zlrh-0.5.0-beta0.1-macos-arm64.tar.gz
cd zlrh-0.5.0-beta0.1-macos-arm64
sh install.sh
```

**第 3 步：新开一个终端窗口**，先确认版本（应显示 `zlrh 0.5.0 (beta0.1)`），再运行 `zlrh`：
首次运行会显示用户协议，输入 `同意` 后进入中文向导。

```bash
zlrh --version
zlrh
```

> 仓库改为私有后，`git clone` 需要登录：先执行 `brew install gh` 和 `gh auth login`，再用
> `gh repo clone YeraldoSmith/ZLRH-beta0.1 ~/zlrh-beta -- --depth 1` 代替第 1 步的第一行。

之后的训练环境（MLX）、第一个模型、斗兽场、蒸馏等完整步骤见 [`INSTALL.md`](INSTALL.md)，
所有命令与参数见安装包里的 `MANUAL.md`。

## 更新与卸载

更新：进入拉取目录执行 `git pull`，再按上面第 2 步重新安装（会覆盖旧版）：

```bash
cd ~/zlrh-beta
git pull
```

卸载（只删除程序；`~/.zlrh` 工作目录会先询问）：

```bash
sh ~/zlrh-beta/releases/0.5.0-beta0.1/zlrh-0.5.0-beta0.1-macos-arm64/uninstall.sh
```

## 安全

- 程序不含遥测；API Key 只从环境变量读取，不写入任何文件；
- 安装脚本不使用 sudo，安装前用 SHA-256 校验程序；macOS 版为 ad-hoc 签名（未经苹果公证，首次运行若被拦截见 INSTALL.md）；
- 斗兽场是纯内存模拟环境，模型输入的任何命令都不会在你的电脑上真正执行。
