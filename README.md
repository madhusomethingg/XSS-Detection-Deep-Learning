# 🛡️ Cross-Site Scripting Detection Using Deep Learning
### Benchmarking CNN, RNN, and Transformer Models for Web Security

> A deep learning pipeline that detects Cross-Site Scripting (XSS) vulnerabilities in web code snippets — comparing CNN, Bidirectional RNN, and Transformer architectures on 100,000 labeled samples, with the Transformer achieving **99.83% accuracy** and the lowest loss of any model tested.

---

## 📌 Overview

Cross-Site Scripting (XSS) attacks inject malicious scripts into web pages, enabling unauthorized actions, data theft, and session hijacking. Traditional rule-based detection systems struggle to keep up with evolving attack patterns — they can't generalize to new variants they haven't seen before.

This project treats XSS detection as a binary classification problem on web code snippets, and asks: which deep learning architecture — CNN, RNN, or Transformer — is most effective at distinguishing malicious code from benign code?

| Research Question | Approach |
|---|---|
| Can ML reliably detect XSS vulnerabilities in code snippets? | Binary classification on 100K labeled HTML/JS samples |
| Which architecture captures XSS patterns most effectively? | CNN vs Bidirectional RNN vs Transformer — full benchmark |
| What are the accuracy/complexity tradeoffs between architectures? | Evaluation on accuracy, loss, precision, recall, and confusion matrices |

---

## 📂 Dataset

**Cross-Site Scripting (XSS) Dataset**  
Source: Kaggle

- **File:** `XSS_dataset.csv`
- **Size:** ~100,000 web-based code snippets
- **Modality:** Text — HTML and JavaScript snippets
- **Labels:** Binary — `0` (benign), `1` (XSS/malicious)
- **Key features:** `<script>` tags, `alert()` functions, JavaScript event handlers, HTML attributes
- **Split:** 70% train / 15% validation / 15% test

---

## 🔧 Tech Stack

| Category | Libraries / Tools |
|----------|-----------|
| Deep Learning | `TensorFlow`, `Keras` |
| Data Manipulation | `pandas`, `numpy` |
| Evaluation | `scikit-learn` |
| Visualization | `matplotlib`, `seaborn` |
| Notebooks | `Jupyter / Google Colab` |

---

## 🗂️ Repository Structure

```
├── CNN_Model.ipynb              # CNN architecture and training
├── RNN_Model.ipynb              # Bidirectional RNN architecture and training
├── Transformer_Model.ipynb      # Transformer encoder architecture and training
├── XSS_dataset.csv              # Dataset
└── README.md
```

---

## 🔬 Methodology

### 1. Data Preprocessing
- Removed duplicate entries and irrelevant HTML elements to reduce noise
- Tokenized code snippets into subword components
- Generated numerical embeddings for ML input
- Padded sequences to uniform fixed length for CNN and RNN compatibility
- Highlighted key XSS indicators: `<script>` tags, `alert()` calls, JavaScript patterns

### 2. Model Architectures

Three architectures were implemented and trained with identical settings for fair comparison — **Batch size: 128 | Epochs: 10**

**CNN**
- Convolutional layers for spatial/local pattern recognition
- Identifies HTML/JS tag structures and XSS-indicative patterns in fixed windows
- Conv layers → MaxPooling → 4 Dense layers

**Bidirectional RNN**
- 3 Bidirectional SimpleRNN layers — processes sequences both forward and backward
- Captures cross-token dependencies characteristic of complex multi-step XSS payloads
- Bidirectional RNN layers → Dense layers

**Transformer**
- 3 Transformer Encoder layers with self-attention mechanism
- Embedding size: 64
- Models long-range dependencies across entire code sequences — understands full context
- Self-attention detects novel and obfuscated XSS patterns that local models miss

> **Key Design Insight:** CNN identifies *local* patterns (a suspicious tag, a function call). RNN captures *sequential* dependencies across tokens. Transformer understands *global context* — how any token relates to any other in the entire sequence, regardless of distance. For XSS, where attacks can span an entire code snippet, global context wins.

---

## 📊 Key Results

| Model | Accuracy | Val Accuracy | Loss | Val Loss | Precision | Recall |
|-------|:--------:|:------------:|:----:|:--------:|:---------:|:------:|
| CNN | 99.21% | 98.83% | 0.0256 | 0.0368 | 98.72% | 99.12% |
| Bidirectional RNN | 98.90% | 98.90% | 0.0316 | 0.0384 | 99.01% | 99.01% |
| **Transformer ★** | **99.83%** | **99.78%** | **0.0045** | **0.0076** | 99.03% | 99.03% |

**Confusion matrix highlights (test set):**
- CNN: 1,241 true negatives · 1,465 true positives · 19 false positives · 13 false negatives
- RNN: 969 true negatives · 1,197 true positives · 12 false positives · 12 false negatives
- Transformer: 1,267 true negatives · 1,461 true positives · **only 4 false positives · 6 false negatives**

**Tradeoffs:**
- CNN achieves slightly higher recall than Transformer — marginally better at catching all XSS instances, but at the cost of more false positives
- RNN balances moderate accuracy with moderate compute — a practical middle ground for resource-constrained environments
- Transformer is the clear winner on accuracy and loss, but self-attention scales quadratically with sequence length

---

## ⚠️ Limitations

- All three models are trained on a Kaggle dataset — performance on real-world, adversarially crafted XSS payloads may differ
- Transformer self-attention is computationally expensive at inference time — not suitable for deployment without optimization (quantization, pruning)
- Binary classification only — multi-class XSS type detection (reflected, stored, DOM-based) is not addressed
- Tokenization may lose some structural information embedded in raw HTML formatting

---

## 🔮 Future Work

- **Fine-tune CodeBERT or GraphCodeBERT** — pretrained models on code corpora would likely outperform all three architectures here
- **Multi-class classification** — distinguish between reflected XSS, stored XSS, and DOM-based XSS attack types
- **Real-time detection pipeline** — integrate model into a browser extension or WAF (Web Application Firewall) for live payload screening
- **Adversarial robustness testing** — evaluate performance against obfuscated XSS payloads specifically designed to evade ML detection

---

## 🚀 Getting Started

```bash
pip install tensorflow pandas numpy scikit-learn matplotlib seaborn
```

1. Place `XSS_dataset.csv` in the root directory
2. Run notebooks in any order — each is fully self-contained:
   - `CNN_Model.ipynb` — CNN training and evaluation
   - `RNN_Model.ipynb` — Bidirectional RNN training and evaluation
   - `Transformer_Model.ipynb` — Transformer training and evaluation

---

## 📄 Research Report

This project was submitted as the final term project for **DATA603 – Principles of Machine Learning**  
University of Maryland, College Park

---

## 👤 Author

Madhumitha Rajagopal

---

## 📄 License

This project is for educational and research purposes.
