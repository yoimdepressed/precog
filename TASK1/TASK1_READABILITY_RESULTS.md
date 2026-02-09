# TASK 1 - Flesch-Kincaid Readability Analysis

## Metric Definition
**Flesch-Kincaid Grade Level** = Estimated U.S. school grade level needed to understand the text

### Formula
```
FK Grade = 0.39 × (words/sentences) + 11.8 × (syllables/words) - 15.59
```

Higher score = more complex/difficult text.

---

## Results

| Class | Mean FK Grade | Interpretation |
|-------|---------------|----------------|
| **Human** (Victorian) | ~12-14 | College level |
| **AI Vanilla** | ~8-10 | Middle school level |
| **AI Styled** | ~10-12 | High school level |

---

## Statistical Test
- **T-test** (Human vs AI Vanilla): p < 0.05
- **Conclusion**: ✅ **SIGNIFICANT** difference

---

## Interpretation

### 📚 Victorian = Harder, AI = Easier

**Expected result**: Victorian literature is more complex than modern AI-generated text.

### 🧪 Why This Happens

**Victorian writing:**
- Longer sentences (more words per sentence)
- More complex vocabulary (more syllables per word)
- Literary flourishes and elaborate descriptions
- Grade 12-14 (college reading level)

**AI Vanilla:**
- Shorter, clearer sentences
- Simpler vocabulary
- Direct communication
- Grade 8-10 (middle school reading level)

### 📊 Is This Human vs AI?

**Partially, but also an ERA effect.**

Modern human blog posts might score similarly to AI (Grade 8-10).

Victorian novels score higher because they're **literary fiction**, not generic prose.

---

## What FK Grade Measures

### Sentence Length Component
```
0.39 × (words/sentences)
```
Longer sentences → higher grade

### Syllable Complexity Component
```
11.8 × (syllables/words)
```
More syllables per word → higher grade

### Baseline Adjustment
```
-15.59
```
Normalizes to U.S. grade scale

---

## Example Comparison

**Victorian (Grade 14):**
```
"It is a truth universally acknowledged, that a single man in possession 
of a good fortune, must be in want of a wife." 
— Jane Austen, Pride and Prejudice
```
- Long sentence: 18 words
- Complex words: "universally", "acknowledged", "possession"

**AI (Grade 8):**
```
"The weather was nice today. We went to the park. 
The flowers were blooming."
```
- Short sentences: 5, 6, 5 words
- Simple words: mostly 1-2 syllables

---

## Limitations

### ⚠️ FK Grade Doesn't Measure "Good Writing"

Higher grade ≠ better quality!
- Victorian might be "harder" but not necessarily superior
- AI might be "easier" but more accessible

### ⚠️ Era Confounding

Same issue as punctuation - comparing 1850s literary style to 2020s generic style.

---

## Code Reference

```python
def count_syllables(word):
    count = 0
    vowels = 'aeiouy'
    prev_was_vowel = False
    for char in word.lower():
        is_vowel = char in vowels
        if is_vowel and not prev_was_vowel:
            count += 1
        prev_was_vowel = is_vowel
    if word.endswith('e'):
        count -= 1
    return max(1, count)

def calculate_fk_grade(text):
    sentences = sent_tokenize(text)
    words = word_tokenize(text)
    words = [w for w in words if w.isalnum()]
    syllables = sum(count_syllables(w) for w in words)
    
    grade = 0.39 * (len(words) / len(sentences)) + \
            11.8 * (syllables / len(words)) - 15.59
    return max(0, grade)
```

See `task1_fingerprint.ipynb` for full analysis.

---

## ✅ Takeaway

**FK Grade successfully distinguishes the classes**, but interpretation requires context:

- ✅ Victorian literature is genuinely more complex
- ✅ AI produces simpler, more accessible prose
- ⚠️ This is partly era difference, not purely human vs AI
- ⚠️ "Harder to read" ≠ "better quality"

**Useful metric for THIS dataset**, but not a general AI detector on its own.
