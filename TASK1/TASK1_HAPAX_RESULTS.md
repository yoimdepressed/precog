# TASK 1 - Hapax Legomena Analysis

## Metric Definition
**Hapax Legomena** = Words appearing exactly once / Total words

Measures the ratio of "rare" or unique words that appear only once in a text.

---

## Results

| Class | Mean Hapax Ratio |
|-------|------------------|
| **Human** (Victorian) | 0.805 |
| **AI Vanilla** | 0.848 |
| **AI Styled** | 0.823 |

---

## Statistical Test
- **T-test** (Human vs AI Vanilla): p < 0.000001
- **Conclusion**: ✅ **SIGNIFICANT** difference

---

## Interpretation

### 🤔 Same Pattern as TTR
**AI Vanilla has HIGHER hapax ratio than Humans!**

Again, this seems counterintuitive - shouldn't humans use more rare words?

### 🧪 Same Length Bias Problem

This metric suffers from the **exact same issue as TTR**:
- Longer texts (humans) → more word repetition → lower hapax ratio
- Shorter texts (AI) → less repetition → higher hapax ratio

### 📊 Pattern Emerging
Both TTR and Hapax show the same counterintuitive result for the same reason: **paragraph length confounding**.

### ✅ Takeaway
**Hapax Legomena is statistically significant** but shares TTR's length bias problem.

Both vocabulary metrics (TTR & Hapax) fail to reveal true human vs AI differences when text lengths vary.

---

## Code Reference
```python
def calculate_hapax_ratio(text):
    words = word_tokenize(text.lower())
    words = [w for w in words if w.isalnum()]
    word_counts = Counter(words)
    hapax_count = sum(1 for count in word_counts.values() if count == 1)
    return hapax_count / len(words) if len(words) > 0 else 0
```

See `task1_fingerprint.ipynb` for full analysis.
