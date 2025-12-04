# Toy Model for “AI Understanding Human Motivation and Proactive Collaboration”

This project implements a **minimal viable toy model** that simulates two core abilities
required for future human-centered AI systems:

1. **Perception** — understanding human emotional states from multimodal signals  
2. **Action** — choosing an appropriate response strategy using reinforcement learning  

This toy model serves as a simplified, testable version of the long-term goal described in  
the final project:  
> “An AI system capable of understanding human motivations, emotions, and values,  
> and proactively collaborating with humans.”

---

# 📌 1. Model Overview

The toy model consists of two components:

## **(A) EmotionClassifier — Multimodal Emotion Understanding**
Input:  
- A 20-dimensional concatenated vector representing:  
  - 5-dim voice features  
  - 5-dim facial features  
  - 10-dim text embedding  

Output:  
- Emotion class:  
  - `0 = Calm`  
  - `1 = Anxious`  
  - `2 = Negative`  

This models the **multimodal perception** stage in a simplified form.

---

## **(B) PolicyNet (DQN) — Response Strategy Selection**
Input:  
- Emotion label (0, 1, or 2)

Output:  
- Response strategy (0–3)

| Strategy ID | Response Type |
|-------------|----------------|
| 0 | Soothing |
| 1 | Informative |
| 2 | Guiding |
| 3 | Supportive |

These strategies represent different styles of empathetic assistance.

The model is trained using **Q-learning** with a small simulated environment.

Reward Rules:
- Emotional improvement → `+1`  
- Emotional worsening → `-1`  
- Conversation interruption → `-2`

---

# 📌 2. End-to-End Input / Output

### **Input to entire system**
A 20-dimensional multimodal feature vector (voice + face + text).

### **Outputs**
1. **Predicted Emotion** (0/1/2)  
2. **Chosen Strategy** (0/1/2/3)  
3. **Strategy Meaning** (e.g., “Soothing”, “Guiding”)  

These outputs represent whether the toy model can:
- *Understand* human emotional states  
- *Act* by selecting a context-appropriate supportive strategy  

---

# 📌 3. Why These Outputs Matter

### **Emotion Output → Perception Ability**
This output tests whether the model can “read” human signals.  
It acts as the **state** for reinforcement learning.

### **Strategy Output → Proactive Collaboration**
This output evaluates the AI's ability to take actions that help the user.  
It enables measurement of:
- emotional improvement,  
- conversation continuation,  
- appropriateness of response.  

Together they form a minimal working prototype of a **human-aware AI system**.

---

# 📌 4. Code Overview

The entire implementation is in one block for simplicity.  
It includes:
- dataset simulation,  
- emotion classifier training,  
- DQN training,  
- final demo.

---


