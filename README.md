# Neural Sieve Cascade (NSC)
*A Multi-Stage Deep Learning Pipeline for Robust Malicious URL Detection*

The **Neural Sieve Cascade (NSC)** is a three-stage malicious URL detection framework designed to balance **speed, accuracy, and real-time feasibility**. Instead of passing every URL into heavy deep learning or transformer models, NSC filters URLs **progressively**:

| Sieve | Model Type | Purpose | URLs Handled |
|------|------------|---------|--------------|
| **Sieve-1** | Logistic Regression (TF-IDF) | Fast filtering of clear benign/malicious URLs | ~75% |
| **Sieve-2** | (CNN + LSTM + BiLSTM) Ensemble-Soft Voting | Handles structurally ambiguous and obfuscated URLs | ~14% |
| **Sieve-3** | TinyBERT Transformer | Resolves the hardest adversarial/phishing cases | ~11% |

This cascaded approach significantly reduces false negatives — especially for **phishing** and **malware** URLs — and enables **real-time cybersecurity defense**.

---

## 📌 Dataset

**Total URLs:** 651,191  
**Classes:**  
- Benign  
- Defacement  
- Phishing  
- Malware

**Example format:**

| url | label |
|-----|-------|
| `http://secure-login.bank.verify-pay.com` | phishing |
| `https://google.com` | benign |

Full dataset originally from:  
https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset

---

## 🧠 Pipeline Architecture

<p align="center">
  <img src="images/nsc_workflow.png" width="650">
</p>

The **Confidence Controller (90%)** decides when a URL advances to the next sieve.

---

## ⚙️ Model Details

### **Sieve-1: Logistic Regression**
- TF-IDF character-level n-grams
- Extremely fast inference (<5 ms)
- Accepts predictions with ≥ 90% confidence

### **Sieve-2: Deep Learning Ensemble**

| Model | Strength |
|-------|----------|
| **CNN** | Captures lexical obfuscations (`paypa1.com`) |
| **LSTM** | Captures long-range token dependencies |
| **BiLSTM** | Learns forward and reverse context |

Soft voting ensures robustness across attack styles.

### **Sieve-3: TinyBERT Transformer**
- Handles **adversarial**, **camouflaged**, and **semantic** manipulations
- Prioritizes recall for phishing and malware cases

---

## 📊 Performance

<p align="center">
  <img src="images/accuracy_comparison.png" width="580">
</p>

| Model | Accuracy (%) |
|------|--------------|
| Logistic Regression | 99.0 |
| CNN | 93.45 |
| LSTM | 90.48 |
| BiLSTM | 89.93 |
| Transformer (TinyBERT) | 94.86 |
| **Neural Sieve Cascade (Final)** | **97.28** ✅ |

### Final Confusion Matrix
<p align="center">
  <img src="images/confusion_matrix.png" width="580">
</p>

### Class-wise Performance

| Class | Precision (%) | Recall (%) | F1-Score (%) |
|-------|--------------|------------|--------------|
| Benign | 97 | 99 | 98 |
| Defacement | 99 | 99 | 99 |
| Malware | 99 | 93 | 96 |
| Phishing | 95 | 87 | 91 |

**Key Improvement:**  
False negatives reduced by:
- **15% in phishing**
- **12% in malware**

---

## 🚀 How to Run

```bash
pip install -r requirements.txt
jupyter notebook notebooks/NSC_Final.ipynb
