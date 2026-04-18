# gw-kn-multimodal-preview

`gw-kn-multimodal` 是一个面向千新星（kilonova, KN）识别的多模态研究仓库，覆盖了从模拟数据生成、HDF5 数据集构建，到 GW+光学联合训练、optical-only 基线训练与评估的完整流程。

当前仓库只提供说明预览，不包含源代码，完整源代码将在适时公开。

## 仓库目标

这个仓库主要解决三类问题：

1. 把引力波事件信息和光学测光序列整理成统一的训练数据集。
2. 训练 GW + optical 的联合模型，用于 KN 配对、检索和分类。
3. 训练不依赖 GW 输入的 optical-only 基线，并分析它在真实/模拟负样本上的泛化表现。

当前代码里最核心的研究对象包括：

- BNS / NSBH 模拟事件及其对应的 kilonova 光变。
- GW 标量参数与 skymap 表示。
- 基于 luptitude、first-detection 对齐和时间偏移建模的光学序列表示。
- 多模态 ALBEF / 对比学习训练，以及 optical-only 分类基线。

## 依赖与运行环境

Python 依赖见 `requirements.txt`，可通过以下命令安装：

```bash
pip install -r requirements.txt
```

主要依赖：Python 3.10+、PyTorch、h5py、numpy、pandas、astropy、healpy、ligo.skymap、tqdm、matplotlib、optuna、tensorboard、graphviz。

额外系统依赖：jq、Slurm（HPC 环境）、SNANA + opsimsummaryv2（仅数据生成流程需要）。

建议在 HPC/Slurm 环境中运行。现有 `.sh` 入口脚本默认就是按 Slurm 提交和资源申请来写的。

## 数据与路径约定

### 数据目录结构

| 用途 | 路径 |
|------|------|
| 多模态训练 HDF5 | `$BASE_DIR/data/ALBEF_dataset/` |
| optical-only 训练 HDF5 | `$BASE_DIR/data/Optical_Only_dataset/` |
| 模型 checkpoint | `$BASE_DIR/data/model/` |
| skymap / SNANA 数据 | `$BASE_DIR/data/` 和 `$BASE_DIR/SNANA/` |

## 常用工作流

### 1. 构建 GW + Optical 联合数据集

这个流程会把 BNS / NSBH 的 GW 参数、skymap 和光学光变整理到同一个 HDF5 中。

### 2. 训练多模态 GW + Optical 模型

训练脚本会从 JSON 中读取数据路径、负样本路径、时间偏移设置、模型超参数和 checkpoint 目录。

### 3. 评估多模态模型

支持检索、分类、OOD 监控和负样本时间偏移评估。

### 4. 构建 optical-only 数据集

这个流程会把光变序列做 first-detection 对齐、2 小时同波段合并、luptitude 变换，并输出 optical-only HDF5。

### 5. 训练 optical-only 基线

该脚本训练结束后会自动调用 optical-only 评估脚本。

## 数据获取

本仓库不包含训练数据和大型模拟文件。数据集需通过以下方式获取：

- **GW 模拟事件**：使用 `dataset/KN_sim/` 下的脚本配合 SNANA/OpSim 生成。
- **训练 HDF5**：使用 `Model/script/create_dataset_bns_nsbh.py` 和 `optical_only/create_optical_only_datasets.py` 从模拟数据构建。
- 如需预构建数据集，请联系作者。

## Citation

如果你使用了本仓库的代码，请引用我们的论文：

```
[Paper in preparation]
```

详见 `CITATION.cff`。

## License

本项目采用 MIT 许可证，详见 [LICENSE](LICENSE)。
