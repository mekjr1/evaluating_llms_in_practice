# Perplexity: How Surprised Is Your Model?  
*Evaluating LLMs in Practice — Part 3 of 7*

## 📌 Recap
In the previous articles we explored why simple classification metrics (accuracy, F₁‑score) fail for generative AI, and we performed a deep dive on BLEU and ROUGE.  Those metrics, while useful for measuring word‑overlap, still don’t tell us whether a model is *fluent* or how *surprised* it is by the text it generates or reads.  

This part introduces **perplexity**, the classic measure of language model fluency, and demonstrates how to compute it with open models.  A companion Jupyter notebook with code and examples is available in the [project repository](https://github.com/mekjr1/evaluating_llms_in_practice/tree/main/part-3-perplexity).

## 🤔 What Is Perplexity?
Perplexity (PPL) is a probabilistic metric that quantifies how well a language model predicts the next token in a sequence.  

- **Definition:** For a sequence of tokens \(x_1,x_2,\dots,x_N\) with model probabilities \(p(x_i)\), perplexity is the exponential of the average negative log‑likelihood:

\[\text{Perplexity}(X) = e^{-\frac{1}{N}\sum_{i=1}^N \log p(x_i)}.\]

- **Intuition:** A lower perplexity means the model assigns higher probability to the observed text — it is *less surprised*.  A higher perplexity means the model struggles to predict the next word and is therefore more surprised.

- **Why it matters:** Perplexity correlates with **fluency** and **quality**.  Models with low perplexity on held‑out text are generally better at producing coherent language.  Perplexity is also widely reported in academic papers to benchmark language models.

## 🧾 Computing Perplexity with Code
There are two practical ways to compute perplexity:

1. **N‑gram models** — build a simple model from your corpus and calculate perplexity directly.  This requires no external libraries and illustrates how perplexity changes when the model sees unfamiliar phrases.
2. **Pretrained language models** — use modern models (e.g. GPT‑2) and read off their negative log‑likelihood loss.  Exponentiating this loss yields perplexity.

Below is a minimal example of the n‑gram approach used in the companion notebook.  It builds a bigram (2‑gram) model from a training corpus and computes perplexity on both the training text and a separate test text.

```python
import math
from collections import defaultdict

def build_ngram_model(text, n=2):
    """Build an n‑gram model from the provided text."""
    model = defaultdict(lambda: defaultdict(int))
    words = text.split()
    for i in range(n, len(words)):
        context = tuple(words[i-n:i])
        word = words[i]
        model[context][word] += 1
    # convert counts to probabilities
    for context, counts in model.items():
        total = float(sum(counts.values()))
        for word in list(counts.keys()):
            counts[word] /= total
    return model

def perplexity(model, text, n=2):
    """Compute perplexity of the model on the provided text."""
    words = text.split()
    log_prob = 0.0
    count = 0
    for i in range(n, len(words)):
        context = tuple(words[i-n:i])
        word = words[i]
        prob = model.get(context, {}).get(word, 1e-6)  # smoothing for unseen words
        log_prob += -math.log(prob, 2)
        count += 1
    return math.pow(2, log_prob / max(count, 1))

train_text = "hello world it is a nice day hello world it is another beautiful day"
test_text = "hello world it is a sunny day hello world it is a nice day"
model = build_ngram_model(train_text, n=2)
train_ppl = perplexity(model, train_text)
test_ppl = perplexity(model, test_text)
print(f"Train perplexity: {train_ppl:.2f}\nTest perplexity: {test_ppl:.2f}")
```

Running this code prints something like:

```
Train perplexity: 1.12
Test perplexity: 35.50
```

The bigram model is very confident (low perplexity) on the text it was trained on, but much less so on unseen sentences.  This demonstrates how perplexity rises when a model encounters novel word sequences.

If you have the `transformers` library installed, you can also compute perplexity directly from a pretrained model’s loss.  The notebook includes an example of this approach (using DistilGPT‑2) when the environment supports it.

### Manual Perplexity via Hugging Face
For completeness, here is how you would compute perplexity using a Hugging Face model:

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch, math

model_id = "distilgpt2"  # small model for demonstration
tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForCausalLM.from_pretrained(model_id)
model.eval()

text = "Perplexity measures how well a language model predicts text."
inputs = tokenizer(text, return_tensors="pt")
with torch.no_grad():
    outputs = model(**inputs, labels=inputs["input_ids"])
    loss = outputs.loss.item()

ppl = math.exp(loss)
print(f"Perplexity: {ppl:.2f}")
```

This calculates the negative log‑likelihood of the sequence and exponentiates it to obtain perplexity.  The method works for any causal LM, though model downloads can be large and might require internet access.

## 📊 Insights from the Notebook
When you run the notebook you’ll notice several key patterns:

- **Train vs Test:** Our simple bigram model achieves a **train perplexity of around 1.12** but a **test perplexity of roughly 35.50** on unseen sentences.  This highlights how sensitive perplexity is to unfamiliar word sequences.
- **Clean vs Jumbled Text:** Well‑formed, grammatical sentences lead to lower perplexity.  If you scramble word order or add random characters, perplexity spikes because the model is far more “surprised.”
- **Model Capacity Matters:** In more advanced examples (when `transformers` is available), larger pretrained models tend to yield lower perplexity on the same text.  However, a fluent hallucination can still have low perplexity — fluency is not the same as truth.

For tables of results and charts comparing training and test perplexity values, refer to the full notebook in the repository.

## ✅ Key Takeaways
- **Perplexity = Surprise:** It quantifies how well a model predicts the next token; lower is better.
- **Fluency Indicator:** Low perplexity correlates with coherent and fluent text, but not necessarily with factual accuracy.
- **Model & Data Matters:** Better models and cleaner input both reduce perplexity.  Noisy or out‑of‑distribution text inflates perplexity.
- **Complementary Metric:** Use perplexity alongside BLEU, ROUGE, and human evaluation to gain a more holistic view of model quality.

## 🔗 Further Reading & Notebook
The complete notebook with code and instructions lives here:  
[**part-3-perplexity/perplexity.ipynb**](https://github.com/mekjr1/evaluating_llms_in_practice/tree/main/part-3-perplexity).  It mirrors the structure of the earlier BLEU/ROUGE notebooks and is ready for you to run or modify.

Up next in **Part 4**, we’ll turn to *retrieval metrics* such as MRR, nDCG, Precision@k, and Recall@k — critical for retrieval‑augmented generation (RAG) systems.  Follow along to see how search quality affects the answers your LLM produces.

