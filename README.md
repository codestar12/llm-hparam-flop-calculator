# 🚀 LLM Hyperparameter & FLOP Calculator

### *"A tool for the innumerate FLOPhead"*

*A tiny standalone web app for choosing pre-training hyperparameters the
way DeepSeek intended.*

👉 **Live calculator (GitHub Pages):**\
https://codestar12.github.io/llm-hparam-flop-calculator/

------------------------------------------------------------------------

## ⭐️ Overview

Selecting hyperparameters for LLM pretraining is... annoying.\
Learning rate, batch size, grad accumulation, sequence length, node
count, GPU count---it's all friction between you and the research you
actually care about.

This tool exists to remove that friction.

It implements the empirical scaling laws from the **DeepSeek V1**
technical report (Chen et al., 2024), which show that:

> **Optimal batch size and learning rate can be predicted directly from
> total compute budget.**

From just a few inputs---**model size**, **hidden dimension**, and
**training tokens**---the tool:

-   Approximates **non-embedding parameters**
-   Calculates **training FLOPs**
-   Computes **optimal LR & batch size**
-   Suggests **highly divisible global batch sizes** at 4k/8k
-   Estimates **alternative model shapes** under a fixed compute budget
-   Converts **FLOPs → parameters** for experiment planning

All in a single self-contained HTML file.\
No dependencies. No build step. No backend. Just vibes and math.

This README incorporates the explanation and motivation from my Substack
post, *Acceptable Hyperparameters and How to Find Them.*

------------------------------------------------------------------------

## 🧠 Background & Motivation

If you are a normal, sane researcher, this lets you be a FLOPhead too---without memorizing
constants or living inside Excel.

------------------------------------------------------------------------

## 📐 How It Works (DeepSeek Scaling Laws)

DeepSeek V1 found strong empirical relationships between total compute
**C** and extremal hyperparameters.

### Compute Formula

    C = 6 × (non-embedding parameters) × (training tokens)

### Optimal Learning Rate

    η_opt = 0.3118 × C^(-0.1250)

### Optimal Batch Size (Tokens per Step)

    B_opt = 0.2920 × C^(0.3271)

### Required Optimizer Settings

To match DeepSeek's regime:

    AdamW
    β1 = 0.9
    β2 = 0.95
    weight_decay = 0.1

------------------------------------------------------------------------

## 🛠 Features

### 1. **Parameters → FLOPs Mode**

Given model parameters + hidden size + tokens, the tool computes:

-   ✔️ Non-embedding parameters\
-   ✔️ Total training FLOPs\
-   ✔️ Optimal LR & optimal batch size\
-   ✔️ Suggested configs for 4k & 8k context\
-   ✔️ Alternative model sizes (e.g., 3B / 5B / 8B) under the same
    compute budget


  ## ⚠️ Assumptions & Limitations

Coefficients come from DeepSeek V1 and assume a dataset similar to theirs.

Architecture assumptions are roughly Llama-3-like (dense).

For MoE models, scaling laws still work but require constraints (DeepSeek MoE paper).

Embedding parameter estimation assumes vocab = 128,256 (Llama 3).

Not optimized for extremely exotic architectures.

Advanced users may wish to refit the scaling curves using the DeepSeek procedure:

Sweep from 1e17–2e19 FLOPs,

Exclude runs >0.25% above minimum loss,

Fit LR and batch-size curves.

(Procedure described in detail in your blog post.) 

8d2f86b9-e12a-4f34-aa62-c54e7dd…

## 🛣️ Roadmap (lol yeah right)

Add presets for Llama, DeepSeek, Qwen, Mixtral

Support dynamic vocab sizes

Add MoE-aware FLOP formulas

Export suggested configs as YAML/JSON

Dark mode switch

🙌 Contributions

PRs welcome.
Bug reports even more welcome.
Spreadsheet-enjoyers welcome out of pity.
