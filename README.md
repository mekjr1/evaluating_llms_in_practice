# Evaluating LLMs in Practice
## A Comprehensive Tutorial Series on LLM Evaluation Metrics

[![GitHub Stars](https://img.shields.io/github/stars/mekjr1/evaluating_llms_in_practice?style=social)](https://github.com/mekjr1/evaluating_llms_in_practice)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains a comprehensive tutorial series on evaluating Large Language Models in practical applications. Through hands-on Jupyter notebooks, mathematical foundations, and real-world experiments, learn how to properly assess LLM performance.

## 📚 Tutorial Series Overview

This is a **7-part series** covering everything from basic metrics to advanced evaluation pipelines:

1. **Part 1**: Introduction to LLM Evaluation Metrics
* 
* **Part 2**: BLEU and ROUGE Deep Dive — Currently Available
* **Part 3**: Perplexity and Language Model Evaluation — Now Available
* **Part 4**: Evaluating Retrieval-Augmented LLMs (RAG) — Now Available
* **Part 5**: A/B Testing for LLM Applications — Now Available
* **Part 6**: Human Evaluation & Preference Modeling — Now Available
* **Part 7**: Holistic LLM Evaluation — Now Available

## 🚀 Quick Start

### Part 1: BLEU and ROUGE Introduction
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mekjr1/evaluating_llms_in_practice/blob/master/part-1-bleu_and_rouge/bleu_and_rouge.ipynb?hl=en#runtime_type=gpu)

A gentle introduction to BLEU and ROUGE metrics with:
- Basic concepts and intuitions
- Simple code examples
- Model comparisons
- Visual analysis

**📁 Location**: `part-1-bleu_and_rouge/bleu_and_rouge.ipynb`

### Part 2: BLEU and ROUGE Deep Dive
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mekjr1/evaluating_llms_in_practice/blob/master/part-2-bleu_rouge_deep_dive/bleu_rouge_deep_dive.ipynb?hl=en#runtime_type=gpu)

**Medium-ready comprehensive tutorial** featuring:
- **Mathematical foundations** with LaTeX formulas
- **Step-by-step manual calculations** showing metric internals
- **Multiple experiments**: toy examples → real models → failure cases
- **Rich visualizations** with 4-panel analysis charts
- **Critical evaluation** of strengths and weaknesses
- **Best practices** for practical applications

**📁 Location**: `part-2-bleu_rouge_deep_dive/bleu_rouge_deep_dive.ipynb`

## 🎯 What You'll Learn

### Core Concepts
- Why accuracy/F1 don't work for generative models
- N-gram overlap metrics and their limitations
- Precision vs recall in text evaluation
- Length bias and how to handle it

### Practical Skills
- Implementing BLEU and ROUGE from scratch
- Comparing multiple LLM models systematically
- Creating meaningful visualizations of metric results
- Understanding when metrics fail and why

### Advanced Topics
- Mathematical foundations of evaluation metrics
- Correlation analysis between different metrics
- Failure case analysis and edge cases
- Modern alternatives to traditional metrics

## 🛠️ Installation & Setup

### Local Setup
```bash
git clone https://github.com/mekjr1/evaluating_llms_in_practice.git
cd evaluating_llms_in_practice
pip install transformers datasets evaluate rouge-score nltk matplotlib pandas seaborn
```

### Google Colab (Recommended)
Simply click the **"Open In Colab"** badges above! Each notebook is configured with:
- ✅ GPU runtime for faster model inference
- ✅ Automatic dependency installation
- ✅ Pre-configured environment

## 📊 Featured Experiments

### Model Comparisons
- **DistilBART**: Specialized summarization model
- **BART-Base**: General-purpose transformer
- **T5-Small**: Text-to-text transfer transformer

### Length Impact Analysis
- **Short summaries** (7 words) vs **Long summaries** (21+ words)
- How length affects BLEU vs ROUGE scores
- Precision vs recall trade-offs

### Failure Case Studies
- Perfect paraphrases scoring poorly
- Factually incorrect text scoring highly  
- Gibberish with high word overlap
- When metrics mislead practitioners

## 📈 Visualizations

Each notebook includes rich visualizations:
- **Bar charts**: Model performance comparisons
- **Correlation plots**: BLEU vs ROUGE relationships
- **Length impact charts**: How summary length affects scores
- **Distribution plots**: Score breakdowns across metrics

## 🎓 Learning Path

### Beginner (Start Here)
1. **Part 1 Notebook**: Get familiar with basic concepts
2. Run simple experiments with toy examples
3. Understand metric outputs and interpretations

### Intermediate 
1. **Part 2 Notebook**: Deep dive into mathematical foundations
2. Implement manual calculations
3. Analyze real model comparisons

### Advanced (Coming Soon)
1. **Part 3+**: Explore perplexity, semantic metrics, and custom pipelines
2. Build production evaluation systems
3. Conduct rigorous A/B testing

## 🤝 Contributing

Contributions are welcome! Please feel free to:
- Report bugs or issues
- Suggest new experiments or visualizations
- Add support for additional models
- Improve documentation and explanations

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Connect

- **GitHub**: [@mekjr1](https://github.com/mekjr1)
- **Repository**: [evaluating_llms_in_practice](https://github.com/mekjr1/evaluating_llms_in_practice)

---

⭐ **Star this repo** if you find it helpful for your LLM evaluation journey!
