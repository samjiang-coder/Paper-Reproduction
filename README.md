# Paper-Reproduction
Reproducibility study for papers

# Paper 1：AbMIL（Attention-based Deep MIL）
## 完成狀態
* code 成功執行
* python 3.10

## 遇到問題
### 1. NumPy API 更新問題
```
np.int → removed
```
✔ 修復方式：
```python
bag_length = int(...)
```
## 結論
已可正常 training
---

#  Paper 2：CLAM
## 狀態
* main.py可執行
---

##  遇到問題
### 1. numpy deprecated API
```
np.int → int
```
### 2. env.yml

套件可能有遺漏或有下載問題，執行main.py後補齊h5py及timm

### 3. 執行

main.py已成功運作，但資料尚未完成 feature preprocessing 階段

## 結論
✔ CLAM preprocessing pipeline 已可用
已部分完成 / 但不完整
② Feature extraction
目前：
沒有執行 extract_features_fp.py
沒有 .pt feature files
---

# Paper 3：TransMIL
## 目前狀態
* main.py可執行(epoch 0)
---

## 已解決問題

### 1. 官方 requirements 安裝問題

```
缺少 pytorch_toolbelt，安裝最新版後會自動升級 torch。
導致與官方指定的 PyTorch 1.7.1 發生版本衝突。
```
解決方法:pip install pytorch-toolbelt==0.4.4
---

##  目前問題

### 1.缺少輸入特徵

錯誤：FileNotFoundError: Camelyon16\pt_files\normal_024.pt
訓練所需的 .pt 特徵檔尚未建立。

## 結論
TransMIL 尚未完成 training
需先完成 CLAM 特徵檔（.pt）的生成
---
# Paper4:NGMIL
## 目前查無code
---

# Paper5:RethinkingMIL
## 目前狀態
已成功進入 training log，但實際未開始訓練