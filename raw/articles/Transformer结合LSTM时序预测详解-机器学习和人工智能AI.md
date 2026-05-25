---
source_url: 微信公众号·机器学习和人工智能AI
ingested: 2026-05-20
sha256: 1300d4f4d770c3bb22b3efaaa94248233a48ebffc12bf45ac9d80e958188e5dd
citation: "机器学习和人工智能AI. 全面讲透Transformer，结合LSTM时序预测！！(微信公众号, 2026)"
domain: machine-learning
---

# 全面讲透Transformer，结合LSTM时序预测 ！!

> **来源：** 机器学习和人工智能AI

大家好~

今儿和大家聊聊将Transformer和LSTM两种网络结构结合起来，用于时间序列的一个完整案例~

首先，咱们看看其中结合起来的优势以及意义~

- **Transformer**：它用一种叫做"自注意力"的机制，可以在数据中直接找出哪些时间点互相影响，即使它们之间隔了很长一段时间。就好比在一大段历史数据里，能快速锁定最关键的信息。
- **LSTM**：这是一种专门处理时间序列（顺序数据）的模型，能够很好地捕捉数据中的连续性和局部趋势，就像记住昨天的情况如何影响今天。

**融合的意义**：当我们把两者结合时，可以既利用Transformer捕捉到长距离的依赖信息，又利用LSTM处理局部的时间变化。这样一来，预测未来数据时既不会遗漏重要的长时依赖，也不会丢失细节上的连续性。

想象你在看一部剧。Transformer就像是能帮你找出全剧中那些关键的剧情线索（不管它们出现在什么时候），而LSTM就像是记住每一集里细节的情节。二者结合，就能让你对接下来剧情的发展有一个更全面的预判。

## 核心原理

### 1. 模型架构与融合策略

- **Transformer模块**

- 利用多头自注意力（Multi-Head Self-Attention）机制，计算每个时间步之间的相互关系。
- 引入位置编码（Positional Encoding）来为输入数据加入顺序信息。

- **LSTM模块**

- 用于捕捉时间序列的局部和短期依赖信息，通过门控机制（遗忘门、输入门和输出门）来更新和维护内部状态。

- **融合策略**

- **并行融合**：Transformer和LSTM可以并行地从输入中提取特征，然后将得到的特征进行拼接或加权求和，得到融合后的表示。
- **级联融合**：也可以先用Transformer提取全局依赖特征，再将这些特征作为附加信息输入到LSTM中，使LSTM在处理局部信息的同时参考全局信息。
- 最终融合后的特征经过一个输出层（通常是全连接层），产生最终的预测结果。

### 2. 训练目标

- 通常采用均方误差（MSE）等损失函数，训练目标为使得预测值与真实值之间的误差最小化。
- 优化过程中，反向传播算法会同时更新Transformer和LSTM中的参数，使得整个模型能够协同工作。

## 核心公式

### 1. Transformer部分

#### 自注意力机制
给定输入序列的表示矩阵 （其中  是序列长度， 是特征维度），首先通过三个线性变换得到查询（Query）、键（Key）和值（Value）：

其中  是可学习的权重矩阵。

自注意力的计算公式为：

这里  是键向量的维度，通过除以  来缓解数值不稳定性。

#### 多头注意力
将自注意力扩展到多头：

其中每个头 ， 为输出投影矩阵。

#### 位置编码
为了保留序列的顺序信息，常用正弦和余弦函数构造位置编码：

将  加入到输入表示中，形成最终的输入 。

### 2. LSTM部分
LSTM单元的计算公式如下（在每个时间步 ）：

- **遗忘门**：

- **输入门**：

- **候选记忆细胞**：

- **更新记忆细胞**：

- **输出门**：

- **更新隐藏状态**：

其中  是Sigmoid函数， 表示逐元素相乘。

### 3. 融合与最终输出
设Transformer提取到的全局特征为  ，LSTM提取的局部特征为  。一种常见的融合方法是通过拼接后经过一个线性层：

其中  和  为输出层的权重和偏置，得到最终的预测值 。

**推理过程**：

- **全局与局部互补**：Transformer能够直接建立任意两个时刻之间的联系，而LSTM则专注于时序的连续性。将两者融合后，模型既能捕捉长距离依赖，也能细致处理短期波动。
- **信息融合**：通过拼接或加权求和，模型将两个模块的信息合并，在训练过程中自适应地调整各部分的权重，优化预测结果。
- **反向传播**：在误差反向传播过程中，梯度会同时流经Transformer和LSTM模块，从而更新各自的参数，使得整个模型的预测性能不断提升。

通过结合Transformer和LSTM的优势，这种时间序列预测方法能够同时考虑长距离依赖和短期局部变化。大白话来说，就是既能"看到全局大图"，又能"抓住细节变化"。

## 完整案例
Transformer以其自注意力机制在捕捉长距离依赖方面表现突出，而LSTM（长短期记忆网络）则在处理局部连续性上有明显优势。因此，将两者进行融合，既能利用Transformer全局建模的优势，又能借助LSTM对局部信息的建模能力，便成为一种颇具吸引力的思路。

下面是完整的Python代码，包含数据生成、模型构建、训练过程~

`

import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np
import matplotlib.pyplot as plt
from torch.utils.data import Dataset, DataLoader
import math
import random

# 设置随机种子以保证结果可复现
torch.manual_seed(42)
np.random.seed(42)
random.seed(42)

# 1. 数据集构建与预处理
class TimeSeriesDataset(Dataset):
def __init__(self, data, seq_length):
self.data = data
self.seq_length = seq_length
self.X, self.y = self.create_sequences(data, seq_length)

def create_sequences(self, data, seq_length):
X = []
y = []
for i in range(len(data) - seq_length):
X.append(data[i:i+seq_length])
y.append(data[i+seq_length])
return np.array(X), np.array(y)

def __len__(self):
return len(self.X)

def __getitem__(self, idx):
return torch.tensor(self.X[idx], dtype=torch.float32), torch.tensor(self.y[idx], dtype=torch.float32)

# 生成虚拟时间序列数据（正弦波+余弦波+噪声）
def generate_synthetic_data(T=1000, freq=0.02):
t = np.arange(T)
A, B = 10, 5
data = A * np.sin(2 * np.pi * freq * t) + B * np.cos(2 * np.pi * freq * t)
noise = np.random.normal(0, 2, size=T)
data = data + noise
return data

# 归一化数据
def normalize_data(data):
mean = np.mean(data)
std = np.std(data)
return (data - mean) / std, mean, std

# 构造数据集
data_raw = generate_synthetic_data(T=1500, freq=0.02)
data_norm, data_mean, data_std = normalize_data(data_raw)

# 设定滑动窗口长度
seq_length = 30

# 划分训练集与测试集（80%训练，20%测试）
split_ratio = 0.8
split_index = int(len(data_norm) * split_ratio)
train_data = data_norm[:split_index]
test_data = data_norm[split_index - seq_length:]

train_dataset = TimeSeriesDataset(train_data, seq_length)
test_dataset = TimeSeriesDataset(test_data, seq_length)

batch_size = 64
train_loader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=1, shuffle=False)

# 2. 模型构建：Transformer + LSTM 融合模型
class PositionalEncoding(nn.Module):
def __init__(self, d_model, max_len=500):
super(PositionalEncoding, self).__init__()
pe = torch.zeros(max_len, d_model)
position = torch.arange(0, max_len, dtype=torch.float).unsqueeze(1)
div_term = torch.exp(torch.arange(0, d_model, 2).float() * (-math.log(10000.0) / d_model))
pe[:, 0::2] = torch.sin(position * div_term)
pe[:, 1::2] = torch.cos(position * div_term)
pe = pe.unsqueeze(0)
self.register_buffer('pe', pe)

def forward(self, x):
seq_len = x.size(1)
x = x + self.pe[:, :seq_len, :]
return x

class TransformerEncoder(nn.Module):
def __init__(self, d_model=64, nhead=4, num_layers=2, dropout=0.1):
super(TransformerEncoder, self).__init__()
self.d_model = d_model
self.pos_encoder = PositionalEncoding(d_model)
encoder_layers = nn.TransformerEncoderLayer(d_model=d_model, nhead=nhead, dropout=dropout)
self.transformer_encoder = nn.TransformerEncoder(encoder_layers, num_layers=num_layers)
self.input_fc = nn.Linear(1, d_model)

def forward(self, src):
src = self.input_fc(src)
src = self.pos_encoder(src)
src = src.transpose(0, 1)
output = self.transformer_encoder(src)
output = output.transpose(0, 1)
global_feature = output[:, -1, :]
return global_feature

class LSTMModule(nn.Module):
def __init__(self, input_size=1, hidden_size=64, num_layers=1, dropout=0.1):
super(LSTMModule, self).__init__()
self.lstm = nn.LSTM(input_size, hidden_size, num_layers, dropout=dropout, batch_first=True)

def forward(self, x):
out, (hn, cn) = self.lstm(x)
local_feature = out[:, -1, :]
return local_feature

class TransformerLSTMFusion(nn.Module):
def __init__(self, d_model=64, nhead=4, num_transformer_layers=2,
lstm_hidden_size=64, lstm_num_layers=1, dropout=0.1, fc_hidden_size=64):
super(TransformerLSTMFusion, self).__init__()
self.transformer_encoder = TransformerEncoder(d_model=d_model, nhead=nhead,
num_layers=num_transformer_layers, dropout=dropout)
self.lstm_module = LSTMModule(input_size=1, hidden_size=lstm_hidden_size,
num_layers=lstm_num_layers, dropout=dropout)
fusion_dim = d_model + lstm_hidden_size
self.fc1 = nn.Linear(fusion_dim, fc_hidden_size)
self.fc2 = nn.Linear(fc_hidden_size, 1)
self.relu = nn.ReLU()

def forward(self, x):
global_feature = self.transformer_encoder(x)
local_feature = self.lstm_module(x)
fusion = torch.cat((global_feature, local_feature), dim=1)
out = self.relu(self.fc1(fusion))
out = self.fc2(out)
return out

# 3. 模型训练与验证
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = TransformerLSTMFusion(d_model=64, nhead=4, num_transformer_layers=2,
lstm_hidden_size=64, lstm_num_layers=1, dropout=0.1, fc_hidden_size=64)
model = model.to(device)

criterion = nn.MSELoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)
num_epochs = 50

train_losses = []
test_losses = []

for epoch in range(num_epochs):
model.train()
epoch_loss = 0
for batch_x, batch_y in train_loader:
batch_x = batch_x.unsqueeze(-1).to(device)
batch_y = batch_y.unsqueeze(-1).to(device)
optimizer.zero_grad()
outputs = model(batch_x)
loss = criterion(outputs, batch_y)
loss.backward()
optimizer.step()
epoch_loss += loss.item() * batch_x.size(0)
epoch_loss /= len(train_loader.dataset)
train_losses.append(epoch_loss)

model.eval()
test_loss = 0
with torch.no_grad():
for batch_x, batch_y in test_loader:
batch_x = batch_x.unsqueeze(-1).to(device)
batch_y = batch_y.unsqueeze(-1).to(device)
outputs = model(batch_x)
loss = criterion(outputs, batch_y)
test_loss += loss.item() * batch_x.size(0)
test_loss /= len(test_loader.dataset)
test_losses.append(test_loss)

print(f"Epoch {epoch+1}/{num_epochs}, Train Loss: {epoch_loss:.4f}, Test Loss: {test_loss:.4f}")

# 4. 模型预测与结果可视化
model.eval()
predictions = []
true_values = []
with torch.no_grad():
for batch_x, batch_y in test_loader:
batch_x = batch_x.unsqueeze(-1).to(device)
output = model(batch_x)
predictions.append(output.item())
true_values.append(batch_y.item())

predictions_denorm = np.array(predictions) * data_std + data_mean
true_values_denorm = np.array(true_values) * data_std + data_mean

time_axis = np.arange(len(true_values_denorm))

plt.figure(figsize=(16, 12))
plt.suptitle("Analysis of Time Series Prediction Results Based on Transformer and LSTM Fusion", fontsize=20)

plt.subplot(2, 2, 1)
plt.plot(range(1, num_epochs+1), train_losses, marker='o', color='dodgerblue', label='Training Loss')
plt.plot(range(1, num_epochs+1), test_losses, marker='s', color='crimson', label='Test Loss')
plt.xlabel("Epochs", fontsize=12)
plt.ylabel("MSE Loss", fontsize=12)
plt.title("Training and Test Loss Curve", fontsize=14)
plt.legend()
plt.grid(True)

plt.subplot(2, 2, 2)
plt.plot(time_axis, true_values_denorm, label="True Values", color='forestgreen', linewidth=2)
plt.plot(time_axis, predictions_denorm, label="Predicted Values", color='orange', linestyle='--', linewidth=2)
plt.xlabel("Time Step", fontsize=12)
plt.ylabel("Values", fontsize=12)
plt.title("True vs Predicted Values Comparison Curve", fontsize=14)
plt.legend()
plt.grid(True)

plt.subplot(2, 2, 3)
errors = np.array(true_values_denorm) - np.array(predictions_denorm)
plt.hist(errors, bins=30, color='mediumvioletred', edgecolor='black', alpha=0.7)
plt.xlabel("Prediction Error", fontsize=12)
plt.ylabel("Frequency", fontsize=12)
plt.title("Prediction Error Histogram", fontsize=14)
plt.grid(True)

plt.subplot(2, 2, 4)
plt.scatter(true_values_denorm, predictions_denorm, color='rebeccapurple', alpha=0.7, edgecolors='w', s=80)
plt.plot([min(true_values_denorm), max(true_values_denorm)], [min(true_values_denorm), max(true_values_denorm)], color='navy', linestyle='--', linewidth=2)
plt.xlabel("True Values", fontsize=12)
plt.ylabel("Predicted Values", fontsize=12)
plt.title("Scatter Plot of Predicted vs True Values", fontsize=14)
plt.grid(True)

plt.tight_layout(rect=[0, 0.03, 1, 0.95])
plt.show()
`
### 数据集
我们利用正弦函数与余弦函数的叠加生成具有周期性的基础信号，同时添加正态分布噪声，使得数据不仅有规律性，也具备随机波动的特点。

函数 `generate_synthetic_data` 根据给定的时间步长和频率生成数据，`normalize_data` 对数据进行归一化，确保输入模型的数据分布均衡，避免训练过程中梯度爆炸或梯度消失的问题。

### 模型架构
**Transformer编码器模块**

- **位置编码**：由于Transformer本身没有顺序信息，利用`PositionalEncoding`模块为输入数据增加位置编码，使得模型能够感知时间序列中各个时刻之间的顺序关系。
- **自注意力机制**：Transformer利用多头自注意力机制，从全局角度捕捉序列中各时间步之间的相关性，生成全局特征。代码中通过 `nn.TransformerEncoder` 实现多层编码器。

**LSTM模块**

- **局部特征提取**：LSTM通过记忆单元和门控机制捕捉输入序列中的局部时序依赖，提取短期动态特征。代码中 `LSTMModule` 通过 `nn.LSTM` 实现，并取最后一个时间步的输出作为局部特征。

**融合策略**

- **特征拼接**：在前向传播过程中，模型将Transformer模块提取的全局特征和LSTM模块提取的局部特征进行拼接，形成融合后的特征向量。
- **全连接层映射**：通过全连接层将融合后的特征映射到预测值上，实现最终的时间序列预测。

### 可视化

![](https://mmbiz.qpic.cn/mmbiz_png/jnpzoaI9v1sZ2a3wz8OiceuFV3jXazoCPBIlcibj875dT7LVouKiaSI9f9fLLy4313IbVR5V5abSTZhbwg3L4dsmg/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_png/jnpzoaI9v1sZ2a3wz8OiceuFV3jXazoCPHHENdupic19OiaAxDXPxD8gnu4WEYYibTYFARicW54Iibic62hoeB5ImGuFQ/640?wx_fmt=png&from=appmsg)

- **训练与测试损失变化曲线**：若训练损失持续下降且测试损失基本保持平稳或同步下降，表明模型具有较好的泛化能力；若测试损失反而上升，则可能存在过拟合问题。
- **真实值与预测值对比曲线**：直观观察预测曲线与真实曲线的吻合程度，能够看出模型对周期性和波动趋势的捕捉效果。两者越接近，说明模型预测效果越好。
- **预测误差直方图**：通过统计误差的分布情况，评估预测误差是否呈现正态分布以及误差的集中趋势。大部分误差聚集在0附近，说明模型预测稳定；若误差分布较宽，则表明模型预测存在较大偏差。
- **预测值与真实值散点图**：通过散点图观察点的分布情况，若大部分点分布在理想拟合直线附近，则说明模型拟合效果好；散点的离散程度可以反映预测的不确定性。

## 最后
**最近准备了[16大块的内容，124个算法问题](http://mp.weixin.qq.com/s?__biz=MzAwNTkyNTUxMA==&mid=2247488962&idx=1&sn=71fb1086b5707d82dc668ec3e7138fd2&chksm=9b146a2bac63e33d7096e03c18abeb56f2067fe1a8334670e5f537853558fb363969d4ae4bfa&scene=21#wechat_redirect)****[的总结](http://mp.weixin.qq.com/s?__biz=MzAwNTkyNTUxMA==&mid=2247488962&idx=1&sn=71fb1086b5707d82dc668ec3e7138fd2&chksm=9b146a2bac63e33d7096e03c18abeb56f2067fe1a8334670e5f537853558fb363969d4ae4bfa&scene=21#wechat_redirect)****，完整的机器学习小册，免费领取~**

![](https://mmbiz.qpic.cn/mmbiz_png/jnpzoaI9v1tvSeeEO4QhqVB7C2REfusicicgq9ZhPbDJa8iah2WYIsUNCfJz06vnicrZiaicnfPclCTbuv1s9cOh1nTg/640?wx_fmt=png&from=appmsg)

**领取：备注「算法小册」即可~**

![](https://mmbiz.qpic.cn/mmbiz_png/jnpzoaI9v1tvSeeEO4QhqVB7C2REfusicBibOAGC9ydiaARDIAXvSeiaV25B9kObNBnql53u5cjibqPYtIKtTlPlt6Q/640?wx_fmt=png&from=appmsg)
