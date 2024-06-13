# DL2024_Team10_Cause-detection-for-increase-in-traffic-accidents


## a. 概述：
主題: 交通事故增加原因偵測(Cause detection for increase in traffic accidents)
簡介: 在本研究中，我們將深入分析台北市警察局提供的 105 至 109 年度道路交通事故調查報告，以探索導致近
年來交通事故增加的可能因素。透過使用模型，來分析並找出影響交通事故發生率的關鍵因素。我們期望成果能夠
為政策制定者提供建議，以制定更有效的交通安全措施，進而減少事故發生並提升公共安全。
目標:根據台北市警察局提供的 105~109 的道路交通事故調查報告表，利用模型分析近年交通事故增加的原因。

# 此資料集為非公開資料!!!
## b. 專案資料：
來源：台北市警察局臺北市交通事故資料
資料處理過程：

## 執行方法
專案根目錄
 -  │ 
 -  ├－105年A1-A4所有當事人.xlsx #資料資料
 -  ├－106年A1-A4所有當事人.xlsx #資料資料
 -  ├－107年A1-A4所有當事人(新增戶籍地).xlsx #資料資料
 -  ├－108年A1-A4所有當事人(新增戶籍地).xlsx #資料資料
 -  ├－109年A1-A4所有當事人(新增戶籍地).xlsx #資料資料
 -  ├── data.py # 執行步驟1：資料處理
 -  ├── RandomForest.py # 執行步驟2：特徵分析
 -  ├── model_training.py # 執行步驟3：模型訓練與評估
 -  ├── shap_analysis.py # 執行步驟4：結果分析
 -  ├── step1.csv # 步驟1產生的合併資料文件
 -  ├── step2.csv # 步驟2產生的特徵重要性文件
 -  └── model # 專案簡介與執行方法（本文件）
 -  └──各種模型
## 簡介

本計畫旨在透過資料處理、特徵分析和模型訓練，建構一個高效的車禍類型分類模型，並使用SHAP分析每年車禍結果的分類變化。以下是具體的執行步驟。

### 步驟1：資料處理

#### `python data.py`

##### 功能
- 讀取Excel文件，合併處理105年至109年的數據，並產生`step1.csv`文件。

##### 實現
- 使用`pandas`庫讀取Excel文件，處理資料空缺和資料排版不一致的問題。
- 合併多年的數據，統一格式，轉換為單一的csv檔。

### 步驟2：特徵分析

#### `python RandomForest.py`

##### 功能
- 讀取`step1.csv`文件，透過隨機森林分析特徵的重要性，並將貢獻較大的特徵輸出到`step2.csv`文件。

##### 實現
- 使用`pandas`庫讀取`step1.csv`文件，進行資料預處理。
- 使用`sklearn`庫的`RandomForestClassifier`分析特徵重要性，篩選出貢獻較大的特徵。

### 步驟3：模型訓練與評估

#### `python model_training.py`

##### 功能
- 讀取`step2.csv`文件，使用各種分類模型調試並選擇最佳模型進行車禍類型分類。

##### 實現
- 使用`pandas`庫讀取`step2.csv`文件，進行資料預處理。
- 嘗試多種分類模型（如決策樹、隨機森林、梯度提升等），調整參數並選擇最佳模型。

### 步驟4：結果分析

#### `python shap_analysis.py`

##### 功能
- 使用調試好的最佳模型結合SHAP分析每年車禍結果的分類變化。

##### 實現
- 使用`pandas`函式庫讀取數據，結合SHAP函式庫分析特徵對預測結果的貢獻。
- 輸出每年車禍類型分類結果的變化趨勢。



## 詳細工作流程

1. **資料準備**
    - 規範化並合併資料為單一csv檔。
    - 處理空缺值：根據特徵的重要性和空缺值的比例，選擇適當的方法處理空缺值，如平均值填補、中位數填補或使用機器學習模型預測填補。

2. **特徵選擇與編碼**
    - 初步特徵選擇：選擇完整數據，使用RandomForestClassifier進行特徵重要性分析，篩選出關鍵特徵。
    - 對分類特徵進行one-hot編碼：將所有非數值型特徵轉換為適合模型訓練的格式。

3. **模型訓練與評估**
    - 模型選擇：選擇幾個常用的分類模型，如決策樹、隨機森林、梯度提升等。
    - 模型訓練：使用選定的模型進行訓練，調整參數以獲得最佳效能。
    - 模型評估：使用交叉驗證方法評估模型的效能，選擇最佳模型。

4. **模型提升與解釋**
    - 模型提升：透過超參數調優和整合方法（如整合學習、Bagging、Boosting等）來提高模型的準確性和穩健性。
    - 模型解釋：使用SHAP值分析模型的決策過程，解釋關鍵特徵對預測結果的影響，並識別出影響事故的主要因素。

### 步驟1：資料處理

#### 問題
1. 在Python中讀取和處理Excel資料速度緩慢：資料排版不一致，有些在第一頁，有些在第二頁。
2. 資料空缺：資料集中存在許多空缺值。

#### 解決方法
1. **統一化**
    - 將資料合併並轉換為單一的csv文件，方便後續拆分為訓練集、測試集和驗證集。

2. **特徵篩選（初步）**
    - 盡量保留足夠完整的特徵。
    - 對於空缺超過70%的特徵，如果沒有明顯的用途，可以先捨棄；如果有半監督學習或填補方法，可以在後續處理時再考慮使用這些數據。

#### 資料保留
1. **地理特徵**
    - 保留座標和區域名稱（轉換為程式碼）。

2. **時間特徵**
    - 晝夜、日、月、年、時間。

3. **人物特徵**
    - 年齡、性別、國籍。

4. **現場特徵**
    - 天氣、光線、道路類型、限速、道路形態、事故位置、事故類型及型態。

5. **案件特徵**
    - 處理別、案號、當事人序、車種。

6. **其他特徵**
    - 死亡人數、2-30日死亡人數、受傷人數、路面狀況1、路面狀況2、路面狀況3、道路障礙1、道路障礙2、號志1、號志2、車道劃分-分向、車道劃分-分道1、車道劃分-分道2、車道劃分-分道3、重大車損、受傷程度、主要傷處、行動電話、車輛用途、當事人行動狀態、駕駛資格情形、駕駛執照種類、飲酒情形、主要車損、其他車損、肇逃否、職業、旅次目的。

7. **結果**
    - 肇因碼-個別、肇因碼-主要。

**注意**：資料多以程式碼呈現，對照表見附圖（對照表.jpg）。

### 步驟2：特徵處理/選擇

#### 目前狀態
   - 已將所有資料轉為csv格式，並保留大部分可考慮使用的特徵。

#### 問題
1. 空缺值尚未處理。
2. 需要篩選出對結果有關鍵影響的特徵。
3. 需要確定是否對每個特徵進行後續分析/處理。

#### 解決方法
1. **選擇特定特徵下完全沒有空缺的資料（完整資料）**
    - 首先處理沒有空缺的子集資料。

2. **特徵重要性分析**
    - 使用RandomForestClassifier分析每個特徵對分類結果的貢獻。
    - 篩選出對結果有顯著貢獻的特徵用於後續模型訓練。

3. **編碼非數值型特徵**
    - 對所有非數值型特徵進行one-hot編碼，準備進行機器學習模型訓練。

### 步驟3：模型測試

#### 操作
1. **模型選擇**
    - 選擇合適的分類模型（如決策樹、隨機森林、梯度提升）。

2. **模型提升**
    - 使用超參數調優、交叉驗證和整合方法來提高事故預測準確率。

3. **解釋性**
    - 利用SHAP（Shapley Additive Explanations）分析並解釋導致事故上升的因素。

#### 工作流程詳解
1. **資料準備**
    - 將所有資料標準化，統一格式後合併為一個csv檔。
    - 處理空缺值：可以使用平均值填補、中位數填補或透過機器學習模型進行預測填補，具體選擇方法依據特徵的重要性和空缺值的比例決定。

2. **特徵選擇與編碼**
    - 進行初步特徵選擇：選擇那些沒有空缺值的數據，使用RandomForestClassifier分析每個特徵的重要性，從中篩選出關鍵特徵。
    - 對非數值型特徵進行one-hot編碼：將所有分類特徵轉換為適合模型訓練的格式，確保資料的一致性和模型的準確性。

3. **模型訓練與評估**
    - 模型選擇：選擇常用的分類模型，如決策樹、隨機森林和梯度提升，進行初步模型訓練。
    - 模型訓練：使用訓練資料集進行模型訓練，並調整參數以最佳化模型效能。
    - 模型評估：採用交叉驗證的方法評估模型效能，選擇表現最佳的模型進行進一步最佳化。

4. **模型提升與解釋**
    - 模型提升：透過超參數調優和整合方法（例如Bagging和Boosting）來提升模型的預測準確性和穩定性。
    - 模型解釋：使用SHAP值分析模型的決策過程，解釋每個特徵對預測結果的影響，並識別影響事故發生的主要因素，從而提升模型的可解釋性。


### 改進方向&改進方法

#### 資料處理

1. **加速Excel讀取**
    - 使用`pandas`的`read_excel`方法可以嘗試並行讀取多個Excel文件，透過`openpyxl`或`xlrd`優化讀取速度。
    - 使用`dask`庫處理大數據集，它能夠更有效率地讀取和處理大規模資料。
    - 對於多頁數據，可以編寫腳本自動偵測並讀取所有頁，並將它們合併為一個資料框。

2. **處理資料空缺**
    - 使用進階填補方法，如插值法（interpolation）或機器學習模型（如KNN）來預測和填補缺失值。
    - 考慮基於相似樣本的填補方法，透過計算相似度選擇最相近的數據來填補。
    - 透過資料增強技術產生額外的資料樣本，以減少資料稀疏性對模型的影響。

#### 特徵處理與選擇

1. **高級特徵選擇**
    - 使用`Boruta`演算法或`LASSO`迴歸等特徵選擇方法，進一步篩選對模型結果有重要影響的特徵。
    - 採用嵌入式方法（如基於樹模型的特徵重要性）來自動選擇特徵。
    - 使用主成分分析（PCA）或線性判別分析（LDA）等降維技術，擷取關鍵特徵，減少資料維度。

2. **處理非數值特徵**
    - 對於類別較多的特徵，可以使用目標編碼（Target Encoding）來取代one-hot編碼，以減少特徵數量和稀疏性。
    - 使用嵌入向量（Embedding）方法，將高維度類別特徵對應到低維向量空間，提升模型的學習能力。

#### 模型訓練與評估

1. **選擇多樣化模型**
    - 除了常用的分類模型，可以嘗試深度學習模型（如神經網路）和整合學習方法（如XGBoost和LightGBM）來提升效能。
    - 利用AutoML工具（如TPOT和AutoKeras），自動化模型選擇和超參數調優，節省時間並提升模型效能。

2. **模型評估與驗證**
    - 採用多種評估指標（如F1-score、ROC-AUC、Precision-Recall曲線）綜合評估模型效能，避免單一指標的偏差。
    - 使用多重交叉驗證（如k折交叉驗證）方法，確保模型在不同資料集上的穩定性和泛化能力。
    - 在測試資料集上進行誤差分析，辨識並處理誤差較大的樣本。

#### 模型提升與解釋

1. **提升模型性能**
    - 使用整合方法（如Stacking）結合多個模型的優勢，提升預測準確度和穩健性。
    - 應用對抗訓練（Adversarial Training）等技術，增強模型對雜訊和異常資料的穩健性。
    - 透過模型監控和線上學習（Online Learning），動態調整模型參數，適應資料變化。

2. **增強模型解釋性**
    - 除了SHAP，還可以使用LIME（Local Interpretable Model-agnostic Explanations）等解釋性工具，提供多角度的解釋結果。
    - 建立視覺化工具（如特徵重要性圖、局部解釋圖），幫助理解和解釋模型的決策過程。
    - 結合業務知識，進行專家審核和解釋，確保模型的解釋結果具有實際意義和可操作性。




#
