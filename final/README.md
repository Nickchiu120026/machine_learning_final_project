# Final Project：Toy Model — 多模態情緒理解與策略選擇

本專案的核心問題是：未來的人工智慧是否能「理解人類的動機、情緒與價值」，並且根據使用者的潛在心理狀態主動做出合適協作？  
這是一個非常困難的目標，因此我設計了一個 **Toy Model（簡化版模型）**，用於模擬未來能力的最小可行雛形。本模型包含兩大部分：

- 多模態情緒分類（Perception）  
- 策略選擇強化學習（Action / RL）

本模型的目的不是完美複製真實世界的對話 AI，而是建立一個能「測試觀念、可運行、可評估」的架構。

---

## 1. Toy Model 的目的

Toy Model 要做的事情，是把未來「理解人 → 主動協作」的複雜行為，拆解成目前可實作的最小雛形：

1. 讓 AI 接收簡化的多模態資料  
2. 推測使用者的情緒狀態  
3. 根據情緒選擇回應策略  
4. 使用強化學習優化策略品質  

這個流程建立了未來 AI 會需要的兩大能力：**理解（Perception）** 與 **行動（Decision-Making）**。

---

## 2. Toy Model 的輸入與輸出

### 輸入：20 維多模態向量

Toy Model 的資料由三種模態組成（以隨機向量模擬）：

- 語音特徵：5 維  
- 臉部特徵：5 維  
- 文本 embedding：10 維  

組合成：

x ∈ R^20

---

### 輸出（兩部分）

#### (A) EmotionClassifier — 情緒分類輸出

| 類別 | 意義 |
|------|------|
| 0 | 平靜 Calm |
| 1 | 焦慮 Anxious |
| 2 | 負向 Negative |

#### (B) PolicyNet（DQN）— 行為策略輸出

| action | 回應策略 | 說明 |
|--------|----------|------|
| 0 | 安撫 Soothing | 降低情緒 |
| 1 | 資訊 Informative | 提供資訊 |
| 2 | 引導 Guiding | 引導使用者思考 |
| 3 | 支持 Supportive | 給予支持 |

這兩個輸出合在一起，就形成 Toy Model 的完整「理解 → 回應」流程。

---

## 3. 使用到的神經網路架構

### 3.1 EmotionClassifier（情緒分類）

使用三層全連接神經網路（MLP）：

Input (20)
↓ Linear(20 → 32)
ReLU
↓ Linear(32 → 16)
ReLU
↓ Linear(16 → 3)
Output (3 logits)

這對應到真實世界的「多模態情緒理解」任務。

---

### 3.2 PolicyNet（DQN）

DQN 策略網路結構如下：

Input (情緒 e: 1 維)
↓ Linear(1 → 16)
ReLU
↓ Linear(16 → 4)
Output Q-values for 4 strategies


輸出為四個策略的 Q-value。

---

## 4. Loss Function（兩種）

### 4.1 EmotionClassifier 的 Loss  
➡ **CrossEntropyLoss**

用於衡量：

- 模型輸出的三類 logits  
- 正確的情緒標籤  

這是分類任務的標準 Loss。

---

### 4.2 PolicyNet（DQN）的 Loss  
➡ **MSELoss（均方誤差）**

DQN 的 Temporal Difference (TD) 目標為：

\[
\text{target} = r + \gamma \max Q(s', a')
\]

模型會最小化：

\[
(Q(s, a) - \text{target})^2
\]

因此使用 MSELoss 逼近 TD target。

---

## 5. 強化學習中的 Reward 設計

為了模擬「使用者被回應後的情緒變化」，Toy Model 設計了：

- 策略適合情緒 → +1  
- 策略不適合 → -1  
- 對話中斷（不舒服） → -2  

這個 reward system 讓 AI 學會：

- 焦慮 → 使用安撫  
- 負向 → 使用支持  
- 平靜 → 使用資訊  

策略會透過反覆試錯越來越合理。

---

## 6. 模型是否有進行優化？

是的，Toy Model 具備基本優化機制：

### EmotionClassifier
- 使用 **Adam optimizer**  
- 使用 **CrossEntropyLoss** 更新分類能力  

### DQN PolicyNet
- 使用 **Adam optimizer**  
- **MSELoss** 更新 Q-value  
- 採用 **epsilon-greedy 探索策略**（20% 隨機）  
- 使用 **TD Target** 更新策略  

這些優化方法能讓模型在短時間內收斂。

---

## 7. Toy Model 的完整流程（Perception → Action）

輸入 x (20 維)
↓ EmotionClassifier
情緒 e ∈ {0,1,2}
↓ PolicyNet (DQN)
選擇策略 a ∈ {0,1,2,3}
↓
使用 reward 更新策略

這是一個完整的 AI 對話「理解 → 決策」架構。

---

## 8. Toy Model 的成功性

### 概念代表性
- 多模態訊號  
- 情緒推測  
- RL 行為選擇  
- 回饋式學習  

### 可測試性
- 情緒分類準確度  
- 策略 reward 是否提升  
- End-to-end 行為是否合理  

### 可行性
- 資料可立即生成  
- 訓練快速（幾秒至十秒）  
- 結構簡單、易於調整  

因此它是非常標準的 *Solvable Model*。

---

## 10. 結語

這份 Toy Model 展示了未來「能理解人類動機並主動協作的 AI」的第一步。  
雖然是簡化版，但它成功展現了：

- **理解能力（Perception）**  
- **協作能力（Decision Making）**  

它是一個能運行、能展示、能測試的完整雛形，呈現了未來人性化人工智慧的基礎概念。
