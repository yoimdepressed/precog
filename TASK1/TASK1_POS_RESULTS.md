# TASK 1 - POS Distribution (Adjective/Noun Ratio) Analysis

## Metric Definition
**Adj/Noun Ratio** = Count of adjectives / Count of nouns

Tests the hypothesis that AI "over-describes" with excessive adjectives.

---

## Results

| Class | Mean Adj/Noun Ratio |
|-------|---------------------|
| **Human** (Victorian) | ~0.35 |
| **AI Vanilla** | ~0.35 |
| **AI Styled** | ~0.35 |

---

## Statistical Test
- **T-test** (Human vs AI Vanilla): **p = 0.812**
- **Conclusion**: ❌ **NOT SIGNIFICANT**

---

## Interpretation

### ❌ This Metric FAILED

All three classes have essentially **identical** adjective-to-noun ratios (~0.35).

### 🤔 Why Did This Fail?

**Hypothesis**: AI would use more adjectives (over-describing)

**Reality**: Modern AI models (GPT, Claude) are trained on massive amounts of human text and have learned natural grammatical patterns.

**AI doesn't "over-describe"** - it mimics human grammar distribution almost perfectly.

### 📊 What This Teaches Us

Not every linguistic intuition translates to a working metric!

This was a **dead end** - AI has learned to match human POS distributions naturally.

### ✅ Takeaway
**POS Distribution is NOT a distinguishing feature** for this dataset.

Grammar patterns alone won't reveal AI authorship - need to look at higher-level structure (like sentence variance).

---

## Code Reference
```python
import spacy
nlp = spacy.load('en_core_web_sm')

def calculate_adj_noun_ratio(text):
    doc = nlp(text)
    adj_count = sum(1 for token in doc if token.pos_ == 'ADJ')
    noun_count = sum(1 for token in doc if token.pos_ in ['NOUN', 'PROPN'])
    return adj_count / noun_count if noun_count > 0 else 0
```

See `task1_fingerprint.ipynb` for full analysis.

---

## Lesson Learned
This metric demonstrates the importance of **testing multiple approaches** - not all linguistic hypotheses will work, and that's okay!
