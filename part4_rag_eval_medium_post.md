# Evaluating Retrieval‑Augmented LLMs: Precision@k, Recall@k, Hit@k, MRR & nDCG  
*Evaluating LLMs in Practice — Part 4 of 7*

In [Part 3](./part3_perplexity_medium_post.md) we explored perplexity as a measure of language model fluency.  In this instalment we move beyond generation metrics and focus on the **retrieval** component of retrieval‑augmented generation (RAG) systems.  Before a language model can answer a question or summarise a document, it often relies on a retriever to fetch relevant context.  Evaluating that retriever is therefore crucial to the overall performance of the system.

This article walks through a small, worked example demonstrating five common **information‑retrieval metrics**:

- **Precision@k** – the fraction of the top‑*k* retrieved items that are relevant.  High precision means fewer irrelevant documents clutter the top of the ranking.
- **Recall@k** – the fraction of the relevant documents that appear in the top‑*k*.  High recall means the system doesn’t miss too many relevant items.
- **Hit@k** – a simple binary measure: did at least one relevant item appear in the top‑*k*?
- **MRR** (Mean Reciprocal Rank) – rewards the system for placing the first relevant document high in the ranking.  A first hit at rank 1 gets 1, at rank 2 gets 1/2, at rank 3 gets 1/3, etc.
- **nDCG** (normalised Discounted Cumulative Gain) – takes into account all relevant documents and discounts them logarithmically by position, then normalises by the ideal ranking.

### 🔧 The Experiment

To illustrate these metrics we created a tiny simulated dataset of three queries and two hypothetical retrieval models.  Each query has a set of **relevant document IDs**, and each model returns a ranked list of candidate IDs.  We then compute metrics on the **top 3** results.

Here’s a concise version of the setup code (see the notebook for full definitions of the metric functions):

```python
# Queries and ground‑truth relevant documents
queries = ['q1', 'q2', 'q3']
relevant_docs = {
    'q1': {'doc1', 'doc3', 'doc4'},
    'q2': {'doc2', 'doc5'},
    'q3': {'doc3', 'doc6', 'doc7', 'doc9'}
}

# Retrieval results for two models
predictions_A = {
    'q1': ['doc1', 'doc2', 'doc3', 'doc4', 'doc5'],
    'q2': ['doc2', 'doc5', 'doc6', 'doc1', 'doc3'],
    'q3': ['doc4', 'doc3', 'doc7', 'doc2', 'doc8', 'doc6', 'doc9']
}
predictions_B = {
    'q1': ['doc2', 'doc4', 'doc5', 'doc1', 'doc3'],
    'q2': ['doc5', 'doc6', 'doc2', 'doc3', 'doc1'],
    'q3': ['doc9', 'doc6', 'doc7', 'doc3', 'doc8', 'doc2']
}

# Compute metrics for k = 3 (see notebook for functions)
metrics = []
for model_name, preds in {'Model A': predictions_A, 'Model B': predictions_B}.items():
    # accumulate precision, recall, hit, mrr, ndcg across queries…
    metrics.append({
        'Model': model_name,
        'Precision@3': …,
        'Recall@3': …,
        'Hit@3': …,
        'MRR': …,
        'nDCG': …
    })
```

### 📊 Results

| Model    | Precision@3 | Recall@3 | Hit@3 | MRR  | nDCG |
|---------:|:-----------:|:--------:|:-----:|:----:|:----:|
| **Model A** |     0.667    |   0.722   |  1.00 | 0.833 | 0.872 |
| **Model B** |     0.667    |   0.694   |  1.00 | 0.833 | 0.866 |

Both models manage to include at least one relevant document in their top three results for every query (**Hit@3 = 1.0**).  However, **Model A** has a slightly higher recall and nDCG, meaning it tends to cover more relevant documents and positions them a bit higher in the list.  Precision@3 and MRR are identical for the two models in this tiny example.

### 💡 Takeaways

- **Precision@k** and **recall@k** give complementary views: one penalises false positives, the other penalises missed relevant items.  You often trade one for the other.
- **Hit@k** is useful when even a single relevant hit is valuable (e.g. retrieving at least one supporting passage for a generated answer).
- **MRR** focuses on *how soon* you see the first relevant result, which matters if users rarely scroll through rankings.
- **nDCG** considers the whole ranked list and discounts lower‑ranked hits; it’s a good holistic measure for ranking quality.
- Small examples can hide important differences—always test on larger, realistic data.  Nevertheless, this exercise shows how the metrics behave and how to interpret them.

➡️ **Next Part:** We’ll dive into how to design and analyse **A/B tests** for language model evaluation, simulating user feedback and significance testing.

📓 *Full notebook with code and further explanations:* [part‑4‑rag_eval/rag_eval.ipynb](./part-4-rag_eval/rag_eval.ipynb).
