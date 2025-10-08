# Human‑in‑the‑Loop Evaluation & Preference Models  
*Evaluating LLMs in Practice — Part 6 of 7*

In [Part 5](./part5_ab_testing_medium_post.md) we compared two language models using an A/B test.  But A/B tests still rely on aggregate metrics like click‑through or satisfaction.  Sometimes you need **direct human feedback** on the content itself: which answer is better?  How helpful is this summary?  Do users prefer one response over another?  Collecting such feedback and aggregating it into signals for evaluation and training is the subject of this part.

## 👩‍⚖️ Why Human‑in‑the‑Loop?

Automatic metrics (BLEU, ROUGE, perplexity, retrieval metrics) only approximate quality.  Humans, however, can judge **correctness, helpfulness, tone, bias, and context**.  Gathering human judgments has become central to modern language‑model development:

- **Ratings:** Assign a scalar score (e.g. 1–5 stars) to a single output.  Useful for assessing overall quality or fluency.
- **Pairwise comparisons:** Given two outputs for the same prompt, decide which one is better.  This produces relative preferences that are well‑suited to training **reward models** in *reinforcement learning from human feedback (RLHF)*.

Both approaches require careful annotation guidelines, multiple raters per example, and mechanisms to ensure data quality.

## 🧪 Simulating Pairwise Preferences

To illustrate how pairwise preference data is aggregated, we simulate a small experiment with 30 prompts and three human raters per prompt:

- **True best model:** For 60 % of the prompts, Model A’s response is truly better; for the remaining 40 %, Model B’s is better.
- **Rater behaviour:** If Model A is truly better, a rater picks A with 0.8 probability; otherwise they pick B with 0.8 probability.

Each rater expresses a preference (1 for A, −1 for B).  We sum the preferences across raters and take the sign to determine the winner for that prompt (ties are possible if raters disagree evenly).

Running this simulation yields the following aggregate:

| Statistic             | Value |
|----------------------:|------:|
| Prompts               | 30    |
| A wins                | 17    |
| B wins                | 13    |
| Ties                  | 0     |
| A preferred (overall) | 56.7 %|

In this run, Model A is favoured on slightly more than half the prompts.  Because we simulated a noisy process, the outcome varies run to run even though A is the true best on 60 % of the prompts.

## 💬 Discussion & Best Practices

- **Multiple raters matter:** Aggregating three or more judgments per prompt reduces noise and allows you to detect when raters disagree.
- **Inter‑rater reliability:** Beyond simple majority vote, metrics like Fleiss’ κ can quantify agreement among annotators.
- **Annotation guidelines:** Clear instructions and examples help raters provide consistent and fair evaluations.
- **Reward models:** In RLHF, a neural network (reward model) is trained on pairwise data to predict human preferences.  This reward model then guides policy optimisation.
- **Bias and fairness:** Human raters may have biases; sampling diverse raters and prompts can mitigate this.  Always monitor for harmful or discriminatory outputs.

➡️ **Next Part:** We’ll take a step back and discuss **holistic evaluation** — combining multiple metrics, including safety and fairness, to judge language models across tasks.

📓 *Full notebook and code*: [part‑6‑human_eval/human_eval.ipynb](./part-6-human_eval/human_eval.ipynb)
