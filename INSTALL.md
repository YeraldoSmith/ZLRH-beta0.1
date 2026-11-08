# ZLangRH（zlrh）安装与部署说明

> 私人 Beta 版。安装和使用前请阅读同目录下的 `EULA.md`（用户协议与版权声明），首次运行 `zlrh` 时也会显示并请你确认。
> Copyright © 2026 Yeraldo Smith. 保留所有权利。

## 安装包一览

| 文件 | 适用设备 |
|---|---|
| `zlrh-<版本>-macos-arm64.tar.gz` | **MacBook Air / Pro（M1~M4 芯片）** ← MacBook Air M4 用这个 |
| `zlrh-<版本>-macos-x86_64.tar.gz` | 2020 年及更早的 Intel 芯片 Mac |
| `zlrh-<版本>-linux-x86_64.tar.gz` | Linux（x86_64，静态链接，任何发行版都能运行） |
| `zlrh-<版本>-windows-x86_64.zip` | Windows 10 / 11（64 位） |
| `SHA256SUMS` | 所有安装包的 SHA-256 校验值 |

每个安装包里都有：`zlrh` 程序、`install.sh` / `install.ps1`（安装）、`uninstall.sh` / `uninstall.ps1`（卸载）、
本说明、`EULA.md`、`COPYRIGHT.md`、`MANUAL.md`（完整使用手册）、`THIRD_PARTY_LICENSES.txt`（第三方组件许可证）、`SHA256SUMS`。

---

## 一、MacBook Air M4（Apple Silicon）—— 详细步骤

下面所有命令都在「终端」里输入（启动台 → 其他 → 终端，或按 ⌘+空格 搜索“终端”）。

### 1. 下载并校验

把 `zlrh-<版本>-macos-arm64.tar.gz` 和 `SHA256SUMS` 下载到“下载”文件夹，然后：

```bash
cd ~/Downloads
shasum -a 256 -c SHA256SUMS --ignore-missing
# 看到  zlrh-…-macos-arm64.tar.gz: OK  才继续；显示 FAILED 说明文件不完整或被改动，请重新下载
```

### 2. 解压并安装

```bash
tar -xzf zlrh-*-macos-arm64.tar.gz
cd zlrh-*-macos-arm64
sh install.sh
```

安装脚本会：校验程序 → 复制到 `~/.local/bin/zlrh` → 去掉 macOS 的“来自互联网”隔离标记 → 询问是否把 `~/.local/bin` 加入 PATH（回车即同意）。
**全程不需要 sudo，也不需要输入开机密码。**

### 3. 确认安装、同意用户协议

**新开一个终端窗口**（让 PATH 生效），然后：

```bash
zlrh --version        # 显示 zlrh 0.5.0 即安装成功
zlrh license          # 查看用户协议与版权声明
zlrh doctor           # 首次运行任何功能都会显示用户协议，阅读后输入「同意」
```

> 如果弹出“无法打开 zlrh，因为无法验证开发者”：打开 **系统设置 → 隐私与安全性**，在页面下方点 **仍要打开**；
> 或在终端执行 `xattr -d com.apple.quarantine ~/.local/bin/zlrh`。这是因为 Beta 版没有经过苹果公证，不影响安全性（你可以用 SHA256SUMS 自行核对）。

### 4. 安装训练环境（MLX，只需一次）

zlrh 用 Python 环境里的 MLX 在 M 芯片上训练和推理。推荐先装 **uv**（它会自动准备合适版本的 Python，不污染系统）：

```bash
# 二选一：
brew install uv                                    # 已安装 Homebrew 的话
curl -LsSf https://astral.sh/uv/install.sh | sh    # 官方安装脚本

zlrh env setup                                     # 自动选择 MLX 后端，创建 ~/.zlrh/venv
# 国内网络慢可以加 PyPI 镜像：
zlrh env setup --pip-index https://pypi.tuna.tsinghua.edu.cn/simple
```

装完运行 `zlrh doctor`，看到 MLX 一项为 ✔ 即可。

### 5. 第一个模型

```bash
mkdir -p ~/zlrh-work && cd ~/zlrh-work
zlrh init                                          # 生成 zlrh.toml 与 models/ runs/ skills/ 目录
zlrh pull Qwen/Qwen2.5-0.5B-Instruct               # 自动测速选择最快的下载线路，下载后校验
zlrh chat models/Qwen2.5-0.5B-Instruct             # 和它聊几句，确认能正常推理
zlrh plan                                          # 看看你的 16GB 内存能训练多大的模型
```

### 6. 试试斗兽场和蒸馏

```bash
zlrh arena check                                   # 本机能让几个模型同时上场
zlrh arena capture -a bot:attack -l C1             # 不用模型，先看斗兽场长什么样
zlrh arena trick -a bot:social -d local:models/Qwen2.5-0.5B-Instruct -s T1

export DEEPSEEK_API_KEY=sk-...                      # 用在线大模型当老师（Key 只在本次终端有效，不会写入文件）
zlrh distill --teacher deepseek/deepseek-chat --student models/Qwen2.5-0.5B-Instruct \
  --skills code:0.6,math:0.4 -n 200 --fit
```

或者直接运行 `zlrh`，进入中文交互向导，一路选择即可。

### 7. MacBook Air M4 16GB 的建议

- 训练：0.5B~1.5B 可以 LoRA 舒服地训练；3B 建议 `--quant 4`（QLoRA）；7B 只建议 QLoRA + 短序列，`--fit` 会自动帮你调；
- 推理 / 斗兽场：0.5B~3B 原精度、7B 用 4bit 都能跑；14B 以上用在线 API；
- MacBook Air 没有风扇，长时间训练会降频，建议插电、放在通风处；
- 所有数据都在 `~/.zlrh`（环境、日志）和你自己的工作目录（模型、运行结果）里。

---

## 二、Intel Mac / Linux

步骤与上面相同，只是下载对应的安装包；Linux 上训练后端是 PyTorch：

```bash
tar -xzf zlrh-*-linux-x86_64.tar.gz && cd zlrh-*-linux-x86_64 && sh install.sh
zlrh env setup                    # NVIDIA 显卡会自动选 CUDA 版 PyTorch，可用 --cuda cu124 指定
```

## 三、Windows 10 / 11

1. 解压 `zlrh-<版本>-windows-x86_64.zip`；
2. 在解压出的文件夹里右键 → **在终端中打开**，执行：
   ```powershell
   powershell -ExecutionPolicy Bypass -File .\install.ps1
   ```
3. 新开一个终端，运行 `zlrh --version`，再运行 `zlrh` 并同意用户协议；
4. 训练环境：先安装 uv（`winget install astral-sh.uv`）或 Python 3.11，然后 `zlrh env setup`。

> Windows 可能提示“Windows 已保护你的电脑”：点 **更多信息 → 仍要运行**（Beta 版没有代码签名）。

---

## 四、卸载

```bash
sh uninstall.sh                                          # macOS / Linux（在安装包目录里运行）
powershell -ExecutionPolicy Bypass -File .\uninstall.ps1 # Windows
```

默认只删除 `zlrh` 程序；`~/.zlrh` 工作目录会先询问；你自己的模型和数据不会被动到。

## 五、安全说明

- 程序不含遥测，不收集任何数据；API Key 只从环境变量读取，不写入任何文件；
- 安装脚本不需要管理员权限，只写入安装目录（默认 `~/.local/bin`）和你同意修改的 shell 配置文件；
- 斗兽场是纯内存的模拟环境，模型输入的任何命令都不会在你的电脑上真正执行；
- 请只从作者提供的渠道获取安装包，并用 `SHA256SUMS` 核对。
