# Toy Model — 一維抽象情緒線索的情緒分類與策略選擇

本專案實作一個極簡的 **Toy Model（簡化模型）**，用以展示未來 AI 可能具備的兩大能力：

1. **理解能力（Perception）**：從訊號推測使用者情緒  
2. **行動能力（Action）**：根據情緒選擇適合的回應策略  

為符合 Toy Model 的設計精神，本模型刻意不使用真實語音、影像或文本，而是將「人類情緒線索」抽象化成一個 **1 維連續值**，以最小複雜度重現「理解 → 決策」的 AI 基本流程。

---

# 📌 1. Toy Model 的概念

本 Toy Model 的核心理念是：

> 用最簡化的設定呈現 AI 如何從一個訊號理解人類狀態，並選擇合適行為。

因此輸入不需要真實資料，而是用一個簡單的數值 \( x ∈ R^1 \) 來表示人類情緒強度，例如：

- x < -0.3 → 負向  
- -0.3 ≤ x ≤ 0.3 → 平靜  
- x > 0.3 → 焦慮  

EmotionClassifier 根據 x 推測情緒狀態（0,1,2），再由 PolicyNet（DQN）選擇策略（0~3）。

---

# 📌 2. 模型輸入 / 輸出

## **輸入：一維情緒線索**

x ∈ R^1

這個數值代表「抽象化後的情緒強度」，是多模態情緒訊號的簡化版本。

---

## **EmotionClassifier（分類模型）輸出：情緒類別 e**

| e | 意義 |
|---|------|
| 0 | 平靜 Calm |
| 1 | 焦慮 Anxious |
| 2 | 負向 Negative |

EmotionClassifier 是一個小型 MLP，用於示範 AI 的「理解能力」。

---

## **PolicyNet（DQN）輸出：策略 action**

| action | 策略類型 | 說明 |
|--------|-----------|------|
| 0 | 安撫 Soothing | 放鬆語氣、降低緊張 |
| 1 | 資訊 Informative | 提供具體資訊 |
| 2 | 引導 Guiding | 協助釐清想法 |
| 3 | 支持 Supportive | 情感支持 |

這代表 AI 對使用者狀態所做出的「回應選擇」。

---

# 📌 3. 神經網路架構

## **EmotionClassifier（分類器）**

Input (1)  
↓ Linear 1→16  
ReLU  
↓ Linear 16→3  
Output (3 logits)  

用途：從 x 判斷情緒類別。

---

## **PolicyNet（DQN 策略網路）**

Input (情緒 e: 1 維)  
↓ Linear 1→16  
ReLU  
↓ Linear 16→4  
Output Q-values for 4 actions  

用途：選擇最高 Q-value 的策略。

---

## 4. Loss Function

本專案使用兩種 Loss（符合作業要求）：

### (1) CrossEntropyLoss — EmotionClassifier

用於多類情緒分類，目標函數為：

$$
\mathcal{L}_{\text{CE}} = -\sum_i y_i \log \hat{y}_i
$$

其中 \(y_i\) 為真實標籤的 one-hot 向量，$$\(\hat{y}_i\)$$ 為模型輸出的機率分佈。

---

### (2) MSELoss — DQN 策略學習

DQN 的 TD target 定義為：

$$
\text{target} = r + \gamma \max_a Q(s', a)
$$

策略網路最小化的目標為：

$$
\mathcal{L}_{\text{DQN}} = \bigl(Q(s, a) - \text{target}\bigr)^2
$$


---

# 📌 5. 強化學習 Reward 設計

模擬使用者被 AI 回應後的變化：

- 策略合適 → **+1**
- 策略不合適 → **-1**
- 對話中斷 → **-2**

這讓 AI 能透過強化學習學習「更適合的回應行為」。

---

# 📌 6. 模型流程（Perception → Action）

以下是完整流程（每行一句，適合 README）：

輸入 x (1 維)  
↓  
EmotionClassifier 預測情緒 e ∈ {0,1,2}  
↓  
PolicyNet (DQN) 選擇策略 a ∈ {0,1,2,3}  
↓  
根據 reward 更新策略  

---
