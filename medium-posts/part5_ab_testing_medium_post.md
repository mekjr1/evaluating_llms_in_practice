# A/B Testing Language Models: From Metrics to Real Users  
*Evaluating LLMs in Practice — Part 5 of 7*

In [Part 4](./part4_rag_eval_medium_post.md) we measured how well retrieval systems surface relevant documents.  But retrieval metrics don’t tell the whole story: even if a model retrieves the right context, does the **generated answer** delight users?  To answer that, you need to run experiments with real users.  This is where **A/B testing** comes in.

## 🧪 What Is A/B Testing?

An A/B test is a controlled experiment where you compare two variants — here, two different versions of a language model — by randomly assigning users to one of them and measuring their behaviour or satisfaction.  Proper randomisation ensures that the only systematic difference between groups is the model version itself.  Statistical tests then tell you whether any observed difference is due to the model or just random fluctuation.

In production, A/B tests are complex: they account for user segmentation, duration, ethical considerations, and more.  In this tutorial we build a **toy simulation** to illustrate the mechanics and interpret the results.

## 🏗️ Simulating User Feedback

We simulate 1,000 user interactions with two model variants:

- **Model A** has a true satisfaction rate of 60 %.
- **Model B** has a true satisfaction rate of 55 %.

Each user either likes the model’s response (1) or doesn’t (0).  We randomly assign users to the two models and record the outcomes.  The simulation proceeds as follows:

1. **Random assignment:** choose at random which users get Model A versus Model B.
2. **Generate outcomes:** for users assigned to Model A, draw successes from a 60 % Bernoulli distribution; for Model B, draw from a 55 % Bernoulli distribution.
3. **Compute observed rates:** count successes and compute the fraction of satisfied users in each group.

See the notebook for the full Python code.

## 📊 Results & Significance Test

After running the simulation, we obtained the following summary (your random seed may produce slightly different numbers):

| Group      | Users | Successes | Observed Rate |
|-----------:|------:|----------:|--------------:|
| **Model A** |   490 |       296 |       60.4 % |
| **Model B** |   510 |       282 |       55.3 % |

The **difference in observed satisfaction** is **5.1 percentage points** (60.4 % vs 55.3 %).  But is this gap statistically meaningful?  To decide, we perform a **two‑proportion z‑test**:

1. Compute the pooled proportion \(p_{pool} = (x_A + x_B)/(n_A + n_B)\).
2. Compute the standard error \(\text{SE} = \sqrt{p_{pool}(1 - p_{pool})(1/n_A + 1/n_B)}\).
3. Compute the z‑statistic \(z = (\hat{p}_A - \hat{p}_B)/\text{SE}\).
4. Convert z into a two‑sided p‑value using the normal distribution.

In our sample the z‑statistic is **1.64**, giving a **p‑value of 0.102**.  Because this p‑value is above the common 0.05 threshold, we *cannot* confidently say Model A is better than Model B — the observed difference could plausibly be due to random variation.

## 💡 Takeaways

- **Random assignment** is crucial: it controls for confounding factors so that differences can be attributed to the model.
- **Sample size matters**: with only 1,000 users, a 5 percentage‑point difference doesn’t reach significance.  Larger samples would reduce the standard error and potentially reveal smaller effects.
- **Don’t stop at p‑values**: also consider effect sizes and practical significance.  A tiny p‑value on a 0.1 % difference might not matter in the real world.
- **Ethics & fairness**: be transparent about experiments, respect user consent, and monitor for disparate impact across demographics.

➡️ **Next Part:** We’ll explore **human‑in‑the‑loop evaluation**: collecting direct human ratings and pairwise preferences to train and evaluate language models.

📓 *Full notebook and code*: [part‑5‑ab_testing/ab_testing.ipynb](./part-5-ab_testing/ab_testing.ipynb)
