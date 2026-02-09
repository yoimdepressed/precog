# TASK 1 - Sentence Length Variance Analysis

## Metric Definition
**Sentence Length Variance** = Standard deviation of sentence lengths (in words)

Measures how much sentence lengths vary within a paragraph. Higher variance = more rhythmic variation.

---

## Results 🏆

| Class | Mean Variance | Interpretation |
|-------|---------------|----------------|
| **Human** (Victorian) | **14.579** | High variation ⭐⭐⭐ |
| **AI Vanilla** | **5.041** | Low variation (monotonous) |
| **AI Styled** | **7.823** | Medium variation |

**Difference (Human - AI Vanilla)**: **+9.538** (THE LARGEST OBSERVED)

---

## Statistical Test
- **T-test** (Human vs AI Vanilla): **p < 0.000001** (extremely significant)
- **Conclusion**: ✅ **STRONGEST DISTINGUISHER**

---

## Interpretation

### 🏆 THE SMOKING GUN

This is **THE** metric that reveals AI writing!

### 📊 What It Shows

**Humans**:
- Use short punchy sentences
- Mix in longer flowing sentences
- Create natural rhythm and pace
- Variance ~14-15 (highly varied)

**AI**:
- Stays in the "middle zone"
- Every sentence ~15-20 words
- Mechanical, predictable rhythm
- Variance ~5 (monotonous)

### 🎯 Why This Works

Unlike TTR/Hapax, this metric is **NOT affected by paragraph length**!

Variance measures **internal structure**, not vocabulary. This reveals:
- Human writing has **natural ebb and flow**
- AI writing is **structurally monotonous**

### 🧪 The Numbers Don't Lie

**3x difference** between human and AI variance.

This is the largest gap observed across ALL metrics tested.

### 📈 Visual Evidence

See the boxplot in `task1_fingerprint.ipynb` - the separation is crystal clear.

Human paragraphs spread widely, AI clusters tightly.

---

## Example Comparison

**Human (Victorian)**:
```
"It was the best of times." (6 words)
"It was the worst of times, it was the age of wisdom, it was the age of foolishness, 
it was the epoch of belief, it was the epoch of incredulity..." (28 words)
```
→ High variance!

**AI**:
```
"The weather was pleasant today." (5 words)
"We went to the park for a walk." (8 words)
"The flowers were blooming everywhere." (5 words)
```
→ Low variance (all 5-8 words)

---

## Code Reference
```python
import re
import numpy as np

def calculate_sentence_variance(text):
    sentences = re.split(r'[.!?]+', text)
    sentences = [s.strip() for s in sentences if s.strip()]
    lengths = [len(s.split()) for s in sentences]
    return np.std(lengths) if len(lengths) > 1 else 0
```

See `task1_fingerprint.ipynb` for full analysis.

---

## ✅ Final Takeaway

**Sentence Length Variance is THE fingerprint of AI writing.**

If you had to pick ONE metric to detect AI, this is it.

- ✅ Not confounded by paragraph length
- ✅ Measures structural pattern, not vocabulary
- ✅ 3x difference (massive gap)
- ✅ Extremely significant (p < 0.000001)

**This is the metric that proves the classes are distinct.**
