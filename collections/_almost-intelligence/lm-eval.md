---
title: Language Models Evaluation
created: "2023-02-18"
modified: "2023-02-18"
hidden: true
---

This post collects the benchmarks for evaluating language models. Many datasets contain the evidence or additional corpora, which we omitted  as our main focus is on the evaluation. The listed datasets are not exhaustive, and we will keep updating the list. Here are some quick links:

| Dataset |  Links |
| --- | --- --- |
| BoolQ | [Homepage](https://github.com/google-research-datasets/boolean-questions){: .btn } |
| ARC | [Homepage](https://allenai.org/data/arc){: .btn }  [Leaderboard (Challenge)](https://leaderboard.allenai.org/arc/submissions/public){: .btn } [Leaderboard (Easy)](https://leaderboard.allenai.org/arc_easy/submissions/public){: .btn } |
| RACE | [Homepage](http://www.cs.cmu.edu/~glai1/data/race){: .btn } [Leaderboard](https://www.qizhexie.com/data/RACE_leaderboard.html){: .btn } |

---

## 1. Perplexity 

The perplexity is defined as the inverse probability of the held-out dataset. The lower the perplexity, the better the model is. Let $$p_\theta$$ be the language model parameterized by $$\theta$$, the perplexity is calculated by the following formula:

$$\begin{align}\text{PPL} = \exp \left( \frac{1}{N} \sum_{i=1}^N \frac{1}{|\bm{x}^{(i)}|} \sum_{j=1}^{|\bm{x}^{(i)}|} \log p_\theta(x_j^{(i)} | \bm{x}_{<j}^{(i)}) \right),\end{align}$$

where $$N$$ is the number of sentences in the held-out dataset, $$\bm{x}^{(i)}$$ is the $$i$$-th sentence, $$\bm{x}^{(i)}_{<j}$$, $$x_j^{(i)}$$ are the prefix and the $$j$$-th token of $$\bm{x}^{(i)}$$, respectively.

The popular datasets for evaluating the perplexity are:

* [Penn Treebank](https://catalog.ldc.upenn.edu/LDC99T42)
* [WikiText-2](https://blog.einstein.ai/the-wikitext-long-term-dependency-language-modeling-dataset/)
* [WikiText-103](https://blog.einstein.ai/the-wikitext-long-term-dependency-language-modeling-dataset/)
* [BookCorpus](http://yknzhu.wixsite.com/mbweb)
* [OpenWebText](https://skylion007.github.io/OpenWebTextCorpus/)
* [Text8](http://mattmahoney.net/dc/textdata.html)


## 2. Yes/No questions

### 2.1 [BoolQ: A Large-scale Question Answering Dataset with Long-range Interactions](https://arxiv.org/abs/1905.10044)

| Split | # Questions |
| --- | --- |
| Train | 9,427 |
| Dev | 3,270 |
| Test | 3,245 |
| **Total** | 15,942 |

## 3. Multiple choice


### 3.1 [ARC (AI2 Reasoning Challenge)](https://allenai.org/data/arc)

The dataset has two subsets: Challenge and Easy. The Challenge subset contain the questions that are not solved by two baselines models.

| # Questions | Challenge | Easy | Total |
| --- | --- | --- | --- |
| Train | 1,119 | 2,251 | 3,370 |
| Dev | 299 | 570 | 869 |
| Test | 1,172 | 2,376 | 3,548 |
| **Total** | 2,590 | 5,197 | 7,787 |

### 3.2 [RACE: Large-scale ReAding Comprehension Dataset From Examinations](http://www.cs.cmu.edu/~glai1/data/race)

The dataset has two partitions: Middle and High, corresponding to the middle school and high school level.

| # Questions | Middle | High | Total |
| --- | --- | --- | --- |
| Train | 25,421 | 62,445 | 87,866 |
| Dev | 1,436 | 3,451 | 4,887 |
| Test | 1,436 | 3,498 | 4,934 |
| **Total** | 28,293 | 69,394 | 97,687 |

### 3.3 [HellaSwag: A Diagnostic Dataset for Grounded Commonsense Inference](https://arxiv.org/abs/1902.01007)

The dataset 
