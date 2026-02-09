
# PRECOG NLP Task: AI Authorship Detection
## "The Ghost in the Machine"

**Author:** S.Yogansh
---

##  Project Overview

This project detects AI-generated text through **linguistic fingerprinting** - proving that structural and stylistic features alone can distinguish human from AI writing, regardless of topic.

**Key Finding:** Sentence length variance is THE smoking gun - humans vary sentence length 3x more than AI.

---

##  Dataset

**Approx 1500 paragraphs across 3 balanced classes:**
- **Class 0 (Human):** 500 paragraphs from Victorian literature (Twain, Austen)
- **Class 1 (AI Vanilla):** 500 generic AI-generated paragraphs
- **Class 2 (AI Styled):** 500 AI paragraphs mimicking Victorian style

**Splits:**
- Train: 1050 (70%)
- Val: 225 (15%)
- Test: 225 (15%)

---

##  Project Structure

```
precog/
├── README.md                              (This file - Quick start guide)
├── REPORT.md                              (Comprehensive 2700-line project report)
├── requirements.txt                       (All dependencies with versions)
│
├── TASK0/ Dataset Creation
│   ├── task0_data_creation.ipynb         (Main notebook - Dataset generation)
│   ├── TASK0_QUICKSTART.md               (Fast-track guide)
│   ├── TASK0_JOURNEY.md                  (Detailed narrative)
│   └── TASK0_PROCESS_LOG.md              (Technical log)
│
├── TASK1/ Linguistic Analysis (The Fingerprint)
│   ├── task1_fingerprint.ipynb           (Main notebook - 7 feature analyses)
│   ├── TASK1_TTR_RESULTS.md              (Type-Token Ratio)
│   ├── TASK1_HAPAX_RESULTS.md            (Hapax Legomena)
│   ├── TASK1_POS_RESULTS.md              (POS distribution)
│   ├── TASK1_SENTENCE_VARIANCE_RESULTS.md (THE KEY FINDING - 22σ effect!)
│   ├── TASK1_PUNCTUATION_RESULTS.md      (Punctuation patterns)
│   ├── TASK1_READABILITY_RESULTS.md      (Flesch-Kincaid scores)
│   └── TASK1_CREATIVE_ANALYSIS.md        (Synthesis & insights)
│
├── TASK2/ Classification (Three Tiers)
│   ├── task2_tier_a_statistician.ipynb   (Random Forest - 83-91% accuracy)
│   ├── task2_tier_b_linguist.ipynb       (GloVe + Neural Net - 95.45% accuracy)
│   ├── task2_tier_c_philosopher.ipynb    (DistilBERT + LoRA - 97.20% accuracy)
│   └── task2_tier_c_distilbert_lora/     (Saved model weights)
│
├── TASK3/ Explainability (SHAP Analysis)
│   └── task3_explainability.ipynb        (SHAP on Tier B - AI-isms myth debunked)
│
├── TASK4/ Adversarial Testing (Genetic Algorithm)
│   ├── task4_adversarial_ga.ipynb        (Manual GA - 60% ceiling discovered)
│   ├── BREAKTHROUGH_STRATEGY.md          (How to break 40% plateau)
│   └── generations/                       (Saved generation results)
│
├── data/
│   ├── raw/
│   │   ├── austen_pride_prejudice.txt    (Project Gutenberg)
│   │   └── dickens_great_expectations.txt
│   ├── processed/
│   │   ├── austen_cleaned.txt            (Cleaned Victorian text)
│   │   └── dickens_cleaned.txt
│   └── dataset/
│       ├── class1_human.jsonl            (500 Victorian literature paragraphs)
│       ├── class2.txt                    (500 AI Vanilla paragraphs)
│       ├── class3.txt                    (500 AI Styled paragraphs)
│       ├── class3_OLD.txt                (Legacy version)
│       ├── train.jsonl                   (1050 samples - 70%)
│       ├── val.jsonl                     (225 samples - 15%)
│       └── test.jsonl                    (225 samples - 15%)
│
└── results/
    └── visualizations/                    (Plots and figures)
```

---

##  Quick Start

### 1. Clone Repository
```bash
git clone <your-repo-url>
cd precog
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

### 3. Explore Notebooks (In Order)

```bash
# Task 0: Dataset Creation
jupyter notebook TASK0/task0_data_creation.ipynb

# Task 1: Linguistic Fingerprinting (Start here to see the key finding)
jupyter notebook TASK1/task1_fingerprint.ipynb

# Task 2: Classification Models
jupyter notebook TASK2/task2_tier_a_statistician.ipynb  # Random Forest
jupyter notebook TASK2/task2_tier_b_linguist.ipynb      # GloVe + Neural Net
jupyter notebook TASK2/task2_tier_c_philosopher.ipynb   # DistilBERT + LoRA

# Task 3: Explainability
jupyter notebook TASK3/task3_explainability.ipynb       # SHAP analysis

# Task 4: Adversarial Testing
jupyter notebook TASK4/task4_adversarial_ga.ipynb       # Genetic Algorithm
```

### 4. Quick Data Exploration
```python
import json
import pandas as pd

# Load dataset
with open('TASK0/data/dataset/train.jsonl', 'r') as f:
    data = [json.loads(line) for line in f]

df = pd.DataFrame(data)
print(df.groupby('class').size())
print(df.groupby('class')['sentence_variance'].mean())
```

---

## 🔬 Key Findings

### Task 1: The Fingerprint Discovery

** The Smoking Gun: Sentence Length Variance**
- **Human:** 13.7 variance (wild rhythm: 3-word → 60-word sentences)
- **AI:** 5.5 variance (monotonous 15-25 word "middle zone")
- **Effect size:** 22σ (p < 0.0000000001) - STRONGEST signal
- **Insight:** AI cannot replicate natural human rhythm instinctively

**Supporting Metrics:**
1.  **Punctuation Patterns** - Humans 3-31x more dramatic
   - Exclamations: 3.5 vs 0.1 per 1000 words (31x!)
   - Semicolons: 13.2 vs 4.3 per 1000 words (3.1x)
   - Em-dashes: 12.4 vs 6.4 per 1000 words (1.9x)

2.  **Type-Token Ratio** - Humans more diverse vocabulary
   - Human: 0.80 (80% unique words)
   - AI: 0.75 (more repetitive)

3. **Paragraph Length** - Humans write 37-44 words longer
   - Human: 134 words average
   - AI: 90-97 words average

4.  **POS Ratios** - Failed (p = 0.812)
   - AI mimics grammar perfectly

**The Insight:**
> "AI can match our vocabulary. AI can match our grammar. AI can match our topics. But AI cannot match our rhythm. That's the fingerprint." - 22σ effect size

---

### Task 2: Classification Results (Three Tiers)

| Tier | Model | Accuracy | Key Insight |
|------|-------|----------|-------------|
| **Tier A** | Random Forest | 83-91% | Structure (sentence variance) works! |
| **Tier B** | GloVe + Neural Net | 95.45% | Semantics beat structure (+11.9 points) |
| **Tier C** | DistilBERT + LoRA | **97.20%**  | Structure + semantics combined (+1.75 points) |

**Tier C Highlights:**
-  **100% Human detection** (zero false positives!)
-  **93.94% AI_Styled detection** (best across all tiers)
-  Only 8 errors out of 286 test samples (2.8% error rate)
-  LoRA efficiency: 1.09% trainable params (740K vs 66M total)

**Cross-Baseline Validation:**
- Victorian dataset: 91% accuracy (complex baseline)
- Twain dataset: 83.57% accuracy (variable baseline)
- Proves author baseline dependency

---

### Task 3: Explainability (SHAP Analysis)

**Major Discovery: AI-isms Are a MYTH!**
-  No "tapestry," "delve," "robust," "comprehensive" in top features
-  Victorian vocabulary NOT detected ("countenance," "parlour")
-  Function words matter: "a" signals Human, "their" signals AI
-  Model learned **discourse style** (abstract vs concrete), not specific vocabulary

**What the model ACTUALLY learned:**
- Modern abstract philosophical discourse → AI
- Concrete Victorian narrative prose → Human
- Era/genre patterns, NOT authorship patterns

---

### Task 4: Adversarial Testing (Genetic Algorithm)

**Evolution Results:**
| Generation | Strategy | Best Score | Gain |
|------------|----------|------------|------|
| Gen 0 | Baseline AI | 12.94% | - |
| Gen 1-3 | Victorian vocab | 40.77% | +27.83 pts |
| Breakthrough | + Sentence variance | 51.34% | +10.57 pts |
| Peak | + Extreme chaos | **59.76%**  | +8.42 pts |
| Over-optimization | Too extreme | 57.12% | -2.64 pts  |

**Key Discoveries:**
1.  **60% Ceiling** - Can't break 60% with readable text
2.  **Chaos Paradox** - Over-optimization backfires (59.76% → 57.12%)
3.  **Multi-strategy requirement** - Single (40%), dual (50%), triple (60%)
4.  **Validated Task 1** - Sentence variance confirmed as 70% weight

** Critical Finding: Genre Bias Revealed**

Tested detector on author's own modern writing (Statement of Purpose):
- **Original modern SOP:** 14.95% Human (misclassified as AI!) 
- **Same content as Victorian narrative:** 58.52% Human (+43.57 points) 

**Implication:** Detector learned **Victorian era/genre patterns**, NOT human authorship!
- Can't detect modern human writing (2020s academic prose)
- 97% accuracy from detecting era (1810-1850s vs 2024), not authorship
- Invalidates real-world deployment for contemporary text

---

### Summary: What We Proved & What We Learned

** Successes:**
1. Sentence variance is THE AI fingerprint (22σ effect)
2. Multi-tier classification validates feature importance
3. DistilBERT achieves 97.20% accuracy on Victorian test set
4. Adversarial attacks plateau at 60% (detector robustness)
5. Over-optimization is detectable (chaos paradox)

** Critical Limitations:**
1. **Genre bias:** Detector learned Victorian narrative ≠ modern human writing
2. **Era dependency:** Can't recognize 2020s human text (15% Human score)
3. **Not production-ready:** 97% accuracy is era discrimination, not authorship detection
4. **Training data matters:** 170-year gap created fundamental flaw

** Key Lesson:**
> "This project demonstrates strong NLP techniques and rigorous analysis, but the genre bias discovery shows that **training data selection** is critical for generalization. High test accuracy (97%) doesn't guarantee real-world applicability if training distribution doesn't match deployment scenarios."

---

**Reproducibility:**
-  All notebooks contain logged outputs and results
-  All experiments documented in REPORT.md with methodology
-  Old dataset(Dickens+Austen) saved in `data/dataset/`
-  New dataset(Twain+Austen) saved in `TASK0/data/dataset/`
-  Trained models saved in `TASK2/task2_tier_c_distilbert_lora/`
-  requirements.txt includes all dependencies with versions

---

##  Creative Analysis Highlights

**"Surprise Us!" Moments:**

1. ** Genre Bias Discovery** (Personal SOP Test)
   - Tested detector on author's modern academic writing → 15% Human 
   - Rewrote as Victorian narrative → 59% Human  (+44 points!)
   - **Revealed:** Detector learned era/genre, not authorship
   - **Impact:** Most important finding - questions detector validity

2. ** Chaos Paradox** (Adversarial Testing)
   - Over-optimization backfires: 59.76% → 57.12% when pushed too far
   - **Novel finding:** Model detects "trying too hard" patterns
   - Proves detector has ensemble robustness, not single-feature

3. ** AI-isms Are a Myth** (SHAP Analysis)
   - No "tapestry," "delve," "robust" in top features
   - Challenges common assumptions about AI writing
   - Model learned discourse style, not vocabulary clichés

4. ** Length Bias Confound** (Task 0)
   - Discovered 37-44 word length difference invalidated TTR/Hapax
   - Led to methodological fix: total aggregation (per 1000 words)
   - Shows attention to experimental design

5. ** Three-Tier Systematic Comparison** (Task 2)
   - Structure (83%) vs Semantics (95%) vs Both (97%)
   - Proves semantics > structure for Victorian dataset
   - Cross-baseline validation (Victorian 91% vs Twain 83%)

---

## � What's Completed vs. What's Not

###  Completed Tasks:

**Task 0: Dataset Creation**
-  1500 paragraphs (500 Human, 500 AI Vanilla, 500 AI Styled)
-  70/15/15 train/val/test split
-  Cleaned and processed Victorian texts
-  Two-baseline validation (Dickens + Twain)

**Task 1: Linguistic Analysis**
-  7 feature analyses (sentence variance, TTR, hapax, POS, punctuation, readability, length)
-  Statistical testing (p-values, effect sizes, Cohen's d)
-  Cross-baseline comparison (Victorian vs Twain)
-  Discovered THE fingerprint: sentence variance (22σ effect)

**Task 2: Classification**
-  Tier A: Random Forest (83-91% accuracy)
-  Tier B: GloVe + Neural Network (95.45% accuracy)
-  Tier C: DistilBERT + LoRA (97.20% accuracy, 100% Human detection)
-  Systematic comparison across all tiers
-  Model saved and documented

**Task 3: Explainability**
-  SHAP analysis on Tier B model
-  Word-level importance scores
-  Debunked AI-isms myth
-  Hypothesis: Era-specific semantic density

**Task 4: Adversarial Testing**
-  Manual Genetic Algorithm (5 mutation strategies)
-  Generation 0-3 evolution documented
-  Breakthrough testing (40% → 60% ceiling)
-  Chaos paradox discovered
-  Personal writing test (genre bias revealed)

### Acknowledged Limitations:

**What We Discovered Doesn't Work:**
1.  **Detector can't recognize modern human writing** (15% Human on 2026 SOP)
2.  **Era/genre bias** (Victorian narrative ≠ modern academic prose)
3.  **Not generalizable** to contemporary text
4.  **POS ratios fail** as discriminator (p = 0.812)

**Why These Limitations Exist:**
- Training data: 170-year era gap (1810-1850s vs 2024)
- Genre mismatch: Narrative fiction (human) vs analytical prose (AI)
- Model learned temporal patterns, not authorship patterns

**What This Teaches:**
> "High test accuracy (97%) doesn't guarantee real-world applicability. Training data selection is CRITICAL for generalization. This project successfully demonstrates NLP techniques but reveals the importance of distribution matching between training and deployment."

---

## 💭 Theoretical Understanding

**Why Sentence Variance Works:**
- Humans write instinctively with rhythm variation (emotional flow)
- AI optimizes for "middle zone" (avoiding extremes reduces error)
- 3-word punchy statements feel natural to us, risky to AI
- 60-word flowing sentences feel expressive to us, verbose to AI

**Why Semantics Beat Structure (Task 2):**
- Victorian vocabulary is era-specific (rare in modern web corpus)
- GloVe embeddings capture era/genre implicitly
- 170-year gap creates semantic distance AI can't bridge
- But this is a FEATURE of our dataset choice, not universal truth

**Why Adversarial Attacks Plateau at 60%:**
- Must transform analytical → narrative (destroys coherence)
- Model detects over-manipulation (chaos paradox)
- Feature interactions: improving variance hurts readability
- 60% is the readability-foolability tradeoff limit

---

**GitHub Repository:** yoimdepressed

## Trained Models & Embeddings

Due to size constraints, pretrained embeddings (GloVe) and trained model weights
(checkpoints, LoRA adapters) are excluded from this repository.

All experiments are fully logged in the notebooks with outputs.
Models can be reproduced using the provided notebooks, or shared upon request.


**Key Documents:**
- **README.md** (this file) - Project overview & quick start
- **REPORT.md** (2700 lines) - Comprehensive findings & methodology
- **7 Jupyter Notebooks** - All experiments with logged outputs

**For Interview Discussion:**
- Genre bias discovery (most important finding)
- Chaos paradox in adversarial testing
- AI-isms myth debunking (SHAP analysis)
- Sentence variance as THE fingerprint (22σ effect)

---

##  License & Data Sources

**Code:** Original work for PRECOG recruitment  
**Human Text:** Project Gutenberg (public domain)
- Mark Twain: *Tom Sawyer*
- Charles Dickens: *Great Expectations*
- Jane Austen: *Pride and Prejudice*

**AI Text:** Generated using Google Gemini

---

##  Acknowledgments

This project demonstrates:
-  Did Complete implementation of all 4 tasks (+ explainability & adversarial testing)
-  Solid Experimental methodology
-  Creative analysis beyond basic metrics
-  Documented Every failure and success
