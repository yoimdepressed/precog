# TASK 0: THE JOURNEY - Data Creation Process
## The Ghost in the Machine

**Author:** S.Yogansh  
**Date:** February 3-4, 2026  
**Status:** ✅ COMPLETE

---

## 🎯 Mission Overview

Create a balanced dataset of 1500 paragraphs to prove AI authorship can be detected through linguistic analysis.

**Classes:**
1. **Human** (500) - Victorian literature (Dickens, Austen)
2. **AI Vanilla** (500) - Generic AI paragraphs
3. **AI Styled** (500) - AI mimicking Victorian style

---

## 📖 The Story

### Phase 1: The Grand Plan

**Author Selection: Charles Dickens + Jane Austen**

Why these two?
- Distinctive, well-documented styles
- Dickens: Verbose, dramatic, social commentary
- Austen: Witty, precise, ironic
- Both Victorian era = consistent time period
- Available on Project Gutenberg

**Paragraph Constraints:**
- Length: 100-200 words
- Long enough for stylistic features
- Short enough for AI generation
- Matches analytical paragraph length

**10 Universal Topics:**
1. Social class and ambition
2. Love and marriage
3. Personal growth
4. Family bonds
5. Appearance vs reality
6. Wealth and morality
7. Justice and redemption
8. Society's influence
9. Pride and prejudice
10. Dreams and disappointment

---

### Phase 2: Human Text Collection

**Step 1: Download from Project Gutenberg**
- Dickens: Great Expectations (ID: 1400)
- Austen: Pride and Prejudice (ID: 1342)

**Step 2: Clean Gutenberg Metadata**

The problem: Project Gutenberg adds boilerplate:
```
*** START OF THIS PROJECT GUTENBERG EBOOK ***
[legal text, transcriber notes, etc.]
*** END OF THIS PROJECT GUTENBERG EBOOK ***
```

**Critical:** If not removed, "human" class gets contaminated with modern legal text!

**Solution:** Regex patterns to strip headers, footers, chapter markers

**Results:**
- Dickens: 982,451 characters of pure prose
- Austen: 726,489 characters of pure prose

**Step 3: Extract Paragraphs**
- Split by double newlines
- Filter: 100-200 words only
- Random sample 250 per author

**Findings:**
- Dickens: 847 valid → sampled 250
- Austen: 623 valid → sampled 250

**✅ Class 1 Complete: 500 human paragraphs**

---

### Phase 3: The API Disaster (AI Vanilla)

**Original Plan: Gemini API Automation**

Setup:
```python
import google.generativeai as genai
model = genai.GenerativeModel('gemini-1.5-flash')
```

Prompt:
```
Write a single paragraph (100-200 words) discussing:
Topic: [topic]

Requirements:
- Clear, analytical style
- Varied sentence structures
- NO title or meta-commentary
```

### ❌ THE FAILURE

**What went wrong:**
- Free tier: 60 requests/minute
- Needed 500 paragraphs
- Rate limit errors everywhere
- API intermittently failed
- Some generations out of word range

**Attempts to fix:**
1. ✅ Added 1-second delay → still failed
2. ✅ Retry logic (3 attempts) → helped but slow
3. ✅ Checkpoint saving every 50 → good
4. ❌ Increased to 2-second delay → still rate limited

**Result:** After 2 hours, only ~150 paragraphs

---

### ✅ THE PIVOT: Manual Generation

**Decision:** Use Google Gemini web interface

**New Process:**
1. Open https://gemini.google.com
2. For each topic (10 topics):
   - Generate 50 paragraphs in batches of 10
   - Copy-paste to text file
   - Validate word counts
3. Combine all 500 paragraphs
4. Save to `class2_ai_vanilla.jsonl`

**Time:** ~3 hours of manual work

**Success rate:** 100%

**Lesson:** Sometimes brute force > automation for one-time tasks

**✅ Class 2 Complete: 500 AI vanilla paragraphs**

---

### Phase 4: AI Styled Generation

**Goal:** Make AI write EXACTLY like Dickens/Austen

**Enhanced Prompt with Style Profiles:**

**Dickens Style:**
- Long, flowing sentences with multiple clauses
- Rich descriptive adjectives
- Serialization (lists of 3+ items)
- Frequent semicolons and em-dashes
- Social commentary woven in
- Victorian vocabulary
- Dramatic, emotional language

**Austen Style:**
- Witty, ironic tone
- Free indirect discourse
- Elegant, balanced sentences
- Sharp social observations
- Restrained emotion
- Regency-era propriety
- Precise, economical language

**Prompt Template:**
```
You are trained to mimic [Author Name].

[Author's] distinctive writing style:
[style profile]

Sample of [Author's] actual writing:
[quote from book]

Now write (100-200 words) on:
Topic: [topic]

Capture: sentence structure, vocabulary, tone, era-appropriate language
```

**Process (Manual Again):**
- 250 paragraphs in Dickens style
- 250 paragraphs in Austen style
- Used Gemini web interface
- Careful validation

**Quality Check:**
- ✅ Victorian vocabulary present
- ✅ Author-specific punctuation
- ✅ Appropriate tone
- ✅ No anachronisms

**Time:** ~3 hours

**✅ Class 3 Complete: 500 AI styled paragraphs**

---

## 📊 Dataset Assembly

### Combining Classes

**Final Structure:**
```json
{
  "text": "The paragraph...",
  "word_count": 156,
  "class": "human",
  "class_numeric": 0,
  "author": "dickens",
  "author_full": "Charles Dickens",
  "book": "Great Expectations",
  "id": 42
}
```

**Process:**
1. Load all 3 classes
2. Add labels (0, 1, 2)
3. Add metadata
4. Shuffle thoroughly
5. Assign global IDs

**Result:** `final_dataset.jsonl` - 1500 samples

---

### Train/Val/Test Splits

**Strategy:** Stratified split (maintain class balance)

**Ratios:**
- Train: 70% (1050 samples) - 350 per class
- Val: 15% (225 samples) - 75 per class
- Test: 15% (225 samples) - 75 per class

---

## 📈 Validation Results

### Statistics

**Word Count Distribution:**
```
Human:      Mean=147.3, Std=24.6
AI Vanilla: Mean=149.1, Std=22.8
AI Styled:  Mean=148.7, Std=23.4
```

**Observation:** Nearly identical! Good - length won't confound classification.

**Class Balance:** Perfect 33.3% each

---

### Quality Check: Reading Examples

**Human (Dickens):**
> "My father's family name being Pirrip, and my Christian name Philip, my infant tongue could make of both names nothing longer or more explicit than Pip..."

✅ Long sentences, personal voice, Victorian grammar

**AI Vanilla:**
> "The nature of social class remains a complex and multifaceted issue in contemporary society..."

✅ Clear modern prose, generic tone, no period features

**AI Styled (Austen-like):**
> "It is a truth universally acknowledged, that a young person of ambition must be in want of proper guidance..."

✅ Mimics Austen's style, Regency diction, ironic tone

---

## 💡 Key Lessons

### What Worked

1. **Manual generation > API** for one-time tasks
2. **Stratified splits** prevent class imbalance
3. **Checkpointing** saves progress
4. **Style profiles in prompts** improve mimicry
5. **Rich metadata** enables deeper analysis

### What Didn't Work

1. **Gemini API free tier** - too restrictive
2. **Initial vague prompts** - wrong length/topic
3. **Not validating early** - had to regenerate

### Unexpected Challenges

1. **Gutenberg metadata** - complex regex needed
2. **Word count variability** - strict validation required
3. **Style consistency** - regenerated ~10% of styled paragraphs

---

## 🏆 Success Metrics

✅ Exactly 1500 paragraphs  
✅ Perfect class balance (500/500/500)  
✅ All 100-200 words  
✅ Train/val/test splits created  
✅ Zero corrupted samples  
✅ High quality validated  

---

## ⏱️ Time Investment

| Phase | Estimated | Actual |
|-------|-----------|--------|
| Planning | 30 min | 45 min |
| Human text | 1 hour | 1.5 hours |
| AI Vanilla | 30 min | 3 hours (API failed) |
| AI Styled | 1 hour | 3 hours |
| Assembly | 30 min | 45 min |
| Documentation | 1 hour | 2 hours |
| **Total** | **4.5 hours** | **~11 hours** |

**Lesson:** Always buffer 2x time!

---

## 📁 Final Files

```
data/dataset/
├── class1_human.jsonl          (500 samples)
├── class2_ai_vanilla.jsonl     (500 samples)
├── class3_ai_styled.jsonl      (500 samples)
├── final_dataset.jsonl         (1500 samples)
├── train.jsonl                 (1050 samples)
├── val.jsonl                   (225 samples)
└── test.jsonl                  (225 samples)
```

---

## 🔜 Next: Task 1

**Goal:** Prove classes are mathematically distinct

**Analyses:**
1. Type-Token Ratio (TTR)
2. Hapax Legomena
3. POS Distribution
4. **Sentence Length Variance** ⭐ (expected strongest)
5. Punctuation Density
6. Flesch-Kincaid Grade

**Hypothesis:** AI shows less sentence variation than humans

---

## ✅ Status: COMPLETE

Task 0 foundation is solid. Ready for linguistic analysis!

*"The data is clean. The classes are balanced. Now we prove they're distinct."*
