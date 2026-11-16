# P047Pro OFDM 与 AFDM 开发

本项目围绕 P047Pro 板卡，先复现 OFDM 收发与射频回环，再建立可校验、可重建的实现，逐步开展 AFDM 波形研究。当前仓库处于文档和协作环境准备阶段，尚未收录可直接运行的收发程序或完整 FPGA 工程。

## 从这里开始

1. 阅读 [学习开发路线](P047Pro_OFDM到AFDM学习开发路线.md)，了解阶段目标。
2. 阅读 [当前进度](docs/progress.md)，确认已经完成的准备和待验证事项。
3. 按 [复现指南](docs/reproduce.md) 准备环境和记录实验。
4. 开发前阅读 [协作指南](docs/collaboration.md)，在个人分支工作。

## 阶段目标

| 阶段 | 目标 | 当前状态 |
|---|---|---|
| A | 厂家 PYNQ 环境与 OFDM 有线回环 | 待板上验证 |
| B | 已知数据、帧对齐和 BER/CRC/PER 校验 | 待开发 |
| C | 从源码重建并修改 FPGA OFDM | 待获取和核对适配工程 |
| D | AFDM 软件基准与射频原型 | 待开发 |
| E | FPGA 实时 AFDM 收发 | 待开发 |

阶段 C 的工程获取可与 A/B 并行。上表只记录本仓库已有证据支持的状态；算法、通信和感知性能均需独立验证。

## 仓库组织

```text
README.md                         项目入口
AGENTS.md                         AI 协作约定
P047Pro_OFDM到AFDM学习开发路线.md   总体路线
docs/progress.md                  进度与下一步
docs/reproduce.md                 环境和复现步骤
docs/collaboration.md             Git 与双人协作
docs/experiment-template.md       单次实验记录模板
docs/experiments/                 实际实验记录
.github/                         PR 和任务模板
```

后续按实际需要添加 `notebooks/`、`models/`、`hardware/` 和 `scripts/`，并同步更新运行入口。

## 分支与协作者

- `main`：经过对应检查的共同基线。当前基线仅包含文档和协作配置，不代表硬件已验证。
- `dev/xiao`：xiao 的日常开发分支。
- 同事账号暂未提供；收到账号后添加协作者，由同事建立自己的分支。

## 外部资料与实验数据

原学习路线中的 `05.教程和源码/...` 等链接指向本地厂家资料，这些文件未随仓库发布，因此在线链接暂不可用。按复现指南在本地准备资料，并记录版本、来源和校验值。源码是否允许公开需逐项确认。

大体积镜像、原始采样、生成的 bitstream、工具缓存及凭据由 `.gitignore` 排除。Git 记录可重建源码、配置、文档和经检查可公开的实验结论；二进制产物另存并在实验记录中注明获取位置和 SHA-256。
