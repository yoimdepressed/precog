# TASK 1 RESULTS: Type-Token Ratio (TTR)

## Metric: Type-Token Ratio
**Definition:** Unique words / Total words

**Example:** "the cat sat on the mat" → 5 unique / 6 total = 0.833

---

## Results Obtained

| Class | Average TTR | Std Dev |
|-------|-------------|---------|
| **Human** | 0.804 | 0.041 |
| **AI Vanilla** | 0.843 | 0.036 |
| **AI Styled** | 0.817 | 0.038 |

**Statistical Test (Human vs AI Vanilla):**
- t-statistic: -39.85
- p-value: < 0.000001
- **✅ Statistically significant difference**

---

## Interpretation

**COUNTERINTUITIVE RESULT:** AI Vanilla shows HIGHER TTR than Human writing.

**Why this seems backwards:**
- We expected humans to have more vocabulary diversity
- Got the opposite - AI has higher TTR

**Possible explanations:**

1. **Length Bias** - TTR is inversely related to text length
   - Human paragraphs: ~135 words average
   - AI paragraphs: ~90 words average
   - Shorter texts naturally have higher TTR

2. **AI Training** - Modern LLMs trained to maximize vocabulary diversity
   - Avoids repetition to sound "intelligent"
   - Uses synonyms aggressively

3. **Human Coherence** - Humans purposefully repeat words for:
   - Narrative cohesion
   - Thematic consistency
   - Natural flow

4. **Victorian Style** - Dickens/Austen used:
   - Pronoun references
   - Repeated motifs
   - Character name repetition

---

## Bottom Line

✅ **Classes ARE statistically distinct** (p < 0.000001)

✅ **Metric is valid for classification** - Direction doesn't matter, just that classes differ

⚠️ **Not about "better"** - High TTR doesn't mean better writing, just different

---

## For Task 2

**Use TTR as a feature** - It successfully distinguishes the classes, even if counterintuitively.
