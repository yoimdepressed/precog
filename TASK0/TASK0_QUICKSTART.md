# TASK 0 - QUICK START GUIDE
## The Ghost in the Machine - Data Creation

---

## 🚀 IMMEDIATE NEXT STEPS

### 1. Open the Notebook (5 minutes)
```bash
cd "C:\Users\yogan\OneDrive\Documents\SEM4\DASS"
jupyter notebook task0_data_creation.ipynb
```

### 2. Get Gemini API Key (5 minutes)
1. Go to: https://makersuite.google.com/app/apikey
2. Sign in with Google account
3. Click "Create API Key"
4. Copy the key
5. Paste it in notebook Cell 4 (replace `'YOUR_API_KEY_HERE'`)

### 3. Run Cells in Order (30 minutes total)
- **Cells 1-3**: Install dependencies & setup (5 min)
- **Cells 4-10**: Download & process human texts (10 min)
- **Cell 11**: Generate AI vanilla paragraphs (10 min) ⚠️ API calls
- **Cell 12**: Generate AI styled paragraphs (10 min) ⚠️ API calls
- **Cells 13-15**: Combine & analyze (5 min)

---

## 📋 DETAILED EXECUTION PLAN

### PHASE 1: Setup (Do Once)

**Step 1.1: Install Dependencies**
```python
# Run Cell 1 in notebook
!pip install requests beautifulsoup4 nltk spacy google-generativeai pandas numpy matplotlib seaborn tqdm python-dotenv scikit-learn
!python -m spacy download en_core_web_sm
```

**Step 1.2: Configure API**
```python
# Cell 4 - Replace with your actual key
GEMINI_API_KEY = 'AIza...'  # Your key here
```

---

### PHASE 2: Human Text Collection (10 minutes)

**Cells to run: 5, 6, 7, 8, 9, 10**

What happens:
- Downloads "Great Expectations" (Dickens)
- Downloads "Pride and Prejudice" (Austen)
- Cleans Project Gutenberg metadata
- Extracts 500 paragraphs (100-200 words each)
- Saves to `data/dataset/class1_human.jsonl`

**Expected output:**
```
✓ Downloaded: data/raw/dickens_great_expectations.txt
✓ Cleaned text: 982451 characters
✓ Found 847 valid paragraphs
✓ Sampled 250 paragraphs
✓ Class 1 (Human) total: 500 paragraphs
```

---

### PHASE 3: AI Vanilla Generation (10 minutes)

**Cell to run: 11**

⚠️ **UNCOMMENT THE CODE FIRST:**
```python
# Remove the # before these lines:
class2_ai_vanilla = generate_class2_dataset(
    TOPICS, 
    samples_per_topic=50,
    output_file='data/dataset/class2_ai_vanilla.jsonl'
)
```

What happens:
- Makes 500 API calls to Gemini
- Generates paragraphs on 10 topics
- 50 paragraphs per topic
- Saves checkpoints every 50 samples
- Total time: ~8-10 minutes

**Expected output:**
```
Generating 500 AI vanilla paragraphs...
[Progress bar: 100%]
Checkpoint: 50 samples saved
Checkpoint: 100 samples saved
...
✓ Generated 500 AI vanilla paragraphs
```

---

### PHASE 4: AI Styled Generation (10 minutes)

**Cell to run: 12**

⚠️ **UNCOMMENT THE CODE:**
```python
class3_ai_styled = generate_class3_dataset(
    TOPICS,
    author_keys=['dickens', 'austen'],
    samples_per_author=250,
    output_file='data/dataset/class3_ai_styled.jsonl'
)
```

What happens:
- Generates 250 paragraphs in Dickens style
- Generates 250 paragraphs in Austen style
- Total: 500 paragraphs
- Total time: ~8-10 minutes

**Expected output:**
```
Generating 500 AI styled paragraphs...
Generating in Charles Dickens style...
[Progress bar]
Generating in Jane Austen style...
[Progress bar]
✓ Generated 500 AI styled paragraphs
```

---

### PHASE 5: Combine & Analyze (5 minutes)

**Cells to run: 13, 14, 15, 16**

⚠️ **UNCOMMENT:**
```python
final_dataset = create_final_dataset(...)
train_data, val_data, test_data = create_splits(...)
df_stats = calculate_dataset_stats(final_dataset)
visualize_dataset(final_dataset)
```

**Expected output:**
```
Final Dataset Summary:
Total samples: 1500
  - Class 0 (Human): 500
  - Class 1 (AI Vanilla): 500
  - Class 2 (AI Styled): 500

Split sizes:
  Train: 1050 samples (70.0%)
  Val: 225 samples (15.0%)
  Test: 225 samples (15.0%)

✓ Visualization saved to results/visualizations/dataset_overview.png
```

---

## ⚠️ COMMON ISSUES & SOLUTIONS

### Issue 1: "API Key Error"
**Solution:** 
- Make sure you copied the FULL API key
- Check for extra spaces
- Key should start with "AIza..."

### Issue 2: "Rate Limit Exceeded"
**Solution:**
- Free tier: 60 requests/minute
- Code already has 1-second delay
- If error persists, increase delay in CONFIG:
  ```python
  'rate_limit_delay': 2  # Change from 1 to 2
  ```

### Issue 3: "Generation Failed"
**Solution:**
- Check your internet connection
- API might be temporarily down
- Code will retry 3 times automatically
- Check checkpoint files - you won't lose progress

### Issue 4: "Out of Memory"
**Solution:**
- Run on Google Colab instead
- Or generate in smaller batches (25 at a time)

---

## 💾 FILES YOU'LL CREATE

After completion:
```
SEM4/DASS/
├── task0_data_creation.ipynb ✓
├── data/
│   ├── raw/
│   │   ├── dickens_great_expectations.txt
│   │   └── austen_pride_prejudice.txt
│   ├── processed/
│   │   ├── dickens_cleaned.txt
│   │   └── austen_cleaned.txt
│   └── dataset/
│       ├── class1_human.jsonl          (500 samples)
│       ├── class2_ai_vanilla.jsonl     (500 samples)
│       ├── class3_ai_styled.jsonl      (500 samples)
│       ├── final_dataset.jsonl         (1500 samples)
│       ├── train.jsonl                 (1050 samples)
│       ├── val.jsonl                   (225 samples)
│       └── test.jsonl                  (225 samples)
└── results/
    └── visualizations/
        └── dataset_overview.png
```

---

## 📊 VALIDATION CHECKLIST

Before moving to Task 1, verify:

- [ ] `class1_human.jsonl` exists with ~500 entries
- [ ] `class2_ai_vanilla.jsonl` exists with ~500 entries  
- [ ] `class3_ai_styled.jsonl` exists with ~500 entries
- [ ] `final_dataset.jsonl` has exactly 1500 entries
- [ ] `train.jsonl` has ~1050 entries (70%)
- [ ] `val.jsonl` has ~225 entries (15%)
- [ ] `test.jsonl` has ~225 entries (15%)
- [ ] Visualization PNG was generated
- [ ] All word counts are between 100-200

**Quick validation script:**
```python
import json

for filename in ['class1_human', 'class2_ai_vanilla', 'class3_ai_styled']:
    with open(f'data/dataset/{filename}.jsonl', 'r') as f:
        count = sum(1 for _ in f)
        print(f"{filename}: {count} samples")
```

Expected output:
```
class1_human: 500 samples
class2_ai_vanilla: 500 samples
class3_ai_styled: 500 samples
```

---

## 🎯 SUCCESS CRITERIA

You're done with Task 0 when:

1. ✅ All 1500 paragraphs generated
2. ✅ Word counts in valid range (100-200)
3. ✅ All three classes represented equally
4. ✅ Train/val/test splits created
5. ✅ Visualization shows clear distributions
6. ✅ Example paragraphs look reasonable

---

## 🔜 WHAT'S NEXT?

After Task 0 completion, you'll move to **Task 1: The Fingerprint**

You'll analyze:
- **Lexical Richness** - Do humans use more varied vocabulary?
- **Syntactic Complexity** - Are AI sentences simpler?
- **Punctuation Patterns** - Does AI avoid semicolons?
- **Readability** - Which class is easier to read?

**Goal:** Mathematically PROVE the three classes are different before building ML models.

---

## 💡 PRO TIPS

### Tip 1: Save Often
After each major cell execution, the code auto-saves checkpoints. But also manually save your notebook: `Ctrl+S`

### Tip 2: Monitor Progress
The progress bars show:
- Current item being processed
- Time elapsed
- Estimated time remaining

### Tip 3: Read Generated Text
Don't just generate blindly! Read some examples to ensure quality. The last cell shows sample paragraphs.

### Tip 4: Document Decisions
Add markdown cells explaining:
- Why you chose certain authors
- Any issues you encountered
- Interesting observations about AI vs human text

### Tip 5: Keep API Key Secret
- Never commit to GitHub
- Use `.env` file or GitHub Secrets
- In notebook, you can use:
  ```python
  from getpass import getpass
  GEMINI_API_KEY = getpass('Enter API key: ')
  ```

---

## 📞 HELP & RESOURCES

**If stuck:**
1. Re-read this guide
2. Check the comprehensive breakdown I provided earlier
3. Review notebook comments
4. Google specific errors
5. Check Gemini API documentation: https://ai.google.dev/docs

**Estimated Total Time:**
- First-time setup: 30 minutes
- Actual generation: 20-30 minutes
- Validation & analysis: 10 minutes
- **Total: 1-1.5 hours**

---

## 🎉 YOU GOT THIS!

Task 0 is the foundation. Take your time, document everything, and make sure the data quality is high. The rest of the tasks depend on this!

**Start now. Run Cell 1. Then Cell 2. Then Cell 3...**

You'll be done before you know it! 🚀
