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
-----

# Paper 6：dsmil-wsi
## 完成狀態
* code 成功執行
* python 3.10
##  環境配置 (Windows / PowerShell)
為避免 Conda `base` 環境衝突與版本不相容問題，請統一使用獨立的虛擬環境安裝。
### 1. 建立獨立虛擬環境
**注意**：PyTorch 官方預編譯 CUDA 輪子與 Windows 相容性以 **Python 3.10** 最為穩定，請勿直接使用過新的 Python 3.13。
建立並啟動 Python 3.10 環境(有env.yml檔可以使用)
conda env create -f env.yml
```
### 2. 安裝支援 GPU (CUDA) 的 PyTorch
pip install torch torchvision --index-url [https://download.pytorch.org/whl/cu124](https://download.pytorch.org/whl/cu124)
```
### 3. 其餘必要套件
pip install pandas numpy scikit-learn pillow

---

## 🚀 執行與復現流程

### 路徑 A：環境最速驗證（推薦優先執行）

使用體積僅數 MB 的標準 MIL 基準資料集（Musk1 等），驗證訓練迴圈與 GPU 是否完全正常：

# 1. 下載輕量級基準資料集
python download.py --dataset=mil

# 2. 啟動 10-Fold 交叉驗證訓練
python train_mil.py


### 路徑 B：TCGA 肺癌資料集復現（預計算特徵）

直接使用官方預先切片並以 ResNet 抽取好的特徵向量（`.csv`），跳過數百 GB 的原始切片處理：

# 1. 下載 TCGA Lung 預提取特徵
python download.py --dataset=tcga

# 2. 啟動訓練（5-fold 交叉驗證）
python train_tcga.py --dataset=TCGA-lung-default

*論文預期效能（TCGA Lung 5-Fold CV）：*

* **Accuracy**: ~93.78%
* **AUC**: ~0.981

### 路徑 C：生成腫瘤預測熱圖 (Attention Map)

# 1. 下載測試切片
python download.py --dataset=tcga-test

# 2. 切片預處理與裁剪 Patch
python test_crop_single.py --dataset=tcga

# 3. 執行推論並輸出熱圖
python testing_tcga.py
``
* 縮圖輸出位置：`./test/thumbnails`
* 熱圖輸出位置：`./test/output`
---

## 常見問題 (Troubleshooting)
* **`AssertionError: Torch not compiled with CUDA enabled`**
* **原因**：安裝到了 CPU 版的 PyTorch，但程式碼中呼叫了 `.cuda()`。
* **解法**：解除安裝後，使用 `--index-url https://download.pytorch.org/whl/cu124` 重新安裝 GPU 版 PyTorch。

* **`ImportError: DLL load failed while importing _imaging: 作業系統無法執行 %1`**
* **原因**：Pillow 底層二進位庫與架構不匹配，或缺少 C++ 執行庫。
* **解法**：執行 `pip install pillow --no-cache-dir --force-reinstall`，並確保已安裝微軟 [Visual C++ Redistributable (X64)](https://www.google.com/search?q=https://aka.ms/vs/17/release/vc_redist.x64.exe)。
