# SDF-PatchTST-CESNET

基于 PyTorch 的网络流量时间序列预测项目。本项目将数据预处理、滑动窗口构建、模型训练、模型评估、对比实验、消融实验和结果可视化整合在一个 Jupyter Notebook 中。

## 项目结构

```text
SDF-PatchTST-CESNET/
├── main.ipynb          # 主程序：预处理、训练、评估和可视化
├── raw_data.csv        # 原始数据，需要自行放到项目根目录
├── README.md           # 项目说明
├── requirements.txt    # Python 依赖
└── .gitignore          # Git 忽略规则
```

运行 Notebook 后，会自动生成以下目录：

```text
data/
└── network_traffic_forecast/
    ├── processed/
    └── window_samples_in168_out72/

outputs/
└── network_traffic_forecast/
    ├── figures/
    ├── logs/
    ├── metrics/
    ├── models/
    ├── predictions/
    └── runs/
```

## 数据要求

将 `raw_data.csv` 放在项目根目录，与 `main.ipynb` 位于同一级。

CSV 至少需要包含：

- `date`：连续的整数时间桶编号，相邻记录步长为 1。
- `n_flows`：非负的网络流量预测目标。

Notebook 默认将每个时间桶解释为 10 分钟，并按时间顺序划分：

- 训练集：70%
- 验证集：15%
- 测试集：15%

标准化参数只在训练集上拟合，滑动窗口不会跨越不同数据集边界。

## 环境安装

推荐使用 Python 3.10 或更高版本，并创建独立虚拟环境。

### 使用 venv

```bash
python -m venv .venv
```

Windows：

```bash
.venv\Scripts\activate
```

macOS / Linux：

```bash
source .venv/bin/activate
```

安装依赖：

```bash
pip install -r requirements.txt
```

## 运行方法

启动 Jupyter：

```bash
jupyter lab
```

打开 `main.ipynb`，然后按顺序运行全部单元格。

Notebook 默认配置：

- 历史输入长度：168
- 预测长度：72
- Batch size：128
- 最大训练轮数：40
- 设备：优先使用 CUDA，否则使用 CPU

完整实验可能需要较长时间。首次检查环境时，可以在 Notebook 的 `CONFIG` 中设置：

```python
is_debug = True
```

## 包含的模型

对比实验包括：

- DLinear
- Transformer
- Autoformer
- TimeXer
- FEDformer
- iTransformer
- PatchTST
- SDF-PatchTST

消融实验包括：

- PatchTST
- SDF-PatchTST
- SDF-PatchTST w/o Decomp
- SDF-PatchTST w/o Spectral

## 注意事项

- 不要上传包含隐私、密码、Token 或其他敏感信息的数据。
- `outputs/` 和自动生成的中间数据默认不提交到 Git。
- 若需要公开实验结果，可以修改 `.gitignore` 后再提交指定文件。
- CUDA 版本的 PyTorch 安装方式与电脑显卡及 CUDA 环境有关，必要时请根据本机环境单独安装 PyTorch。
