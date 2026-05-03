# Characterizing-Long-Context-Failure-Modes-in-Large-Language-Models-using-LongBench-v2

This repository contains three iterative notebook versions for evaluating long-context language models on the LongBench benchmark. Each notebook builds on the previous version and reflects the evolution of the evaluation pipeline, from a simple single-model baseline to a more robust and reusable multi-model framework with long-context optimization.

---

## Notebook Overview

### `eval_v1.ipynb`
Baseline evaluation pipeline.

This notebook serves as the initial prototype for long-context evaluation. It implements a simple baseline setup for testing the LongBench dataset on a single model:

- Evaluates LongBench with a baseline pipeline
- Uses **Qwen2-7B-Instruct** as the only target model
- Runs single-model inference and accuracy evaluation
- Designed for initial validation of the evaluation workflow

This version is primarily used to establish the baseline and verify the correctness of dataset loading, prompt construction, model inference, and answer extraction.

---

### `eval_v2.ipynb`
Extended multi-model evaluation pipeline.

This notebook is built on top of `eval_v1.ipynb` and expands the framework into a more complete experimental pipeline.

Main improvements:

- Extends evaluation from one model to **three target models**
- Supports:
  - **Qwen2-7B-Instruct**
  - **Qwen2.5-7B-Instruct**
  - **LLaMA-3.1-8B-Instruct**
- Records full experimental outputs and evaluation results
- Improves result tracking for systematic comparison across models
- Enables structured benchmarking and experiment logging

This version is used for full-scale experiments and comparative evaluation across multiple models.

---

### `eval_v3.ipynb`
Optimized long-context evaluation pipeline.

This notebook is built on top of `eval_v2.ipynb` and introduces major engineering improvements for long-context efficiency and stability.

Main improvements:

- Adds **pre-tokenized reusable input design** to reduce repeated tokenization overhead
- Reuses pre-generated tokenized contexts across runs for more efficient evaluation
- Refactors the evaluation code for cleaner and more stable execution
- Improves implementation based on **official Qwen recommendations**
- Adds support for **YaRN** (Yet another RoPE extension) for long-context scaling
- Improves handling of long-context inference under various truncation strategies

This version is designed for more efficient large-scale long-context experiments, especially when evaluating extended context windows under YaRN.

---

## Progression Summary

| Notebook | Purpose | Main Focus |
|---|---|---|
| `eval_v1.ipynb` | Baseline prototype | Single-model baseline on Qwen2-7B |
| `eval_v2.ipynb` | Full experiment pipeline | Multi-model evaluation and experiment logging |
| `eval_v3.ipynb` | Optimized long-context pipeline | Reusable tokenization, cleaner code, YaRN support |

---

## Runtime Environment & Requirements

- Google Colab
- System RAM: 167.1 GB
- GPU: NVIDIA A100 (80GB)

---

## Result Generation Workflow (`eval_v3.ipynb`)

1. **Pre-tokenize LongBench v2 contexts**  
   Tokenize the full context of LongBench v2 contexts in advance and save the tokenized outputs as `.pkl` files for reuse in later experiments.

2. **Run evaluation pipeline**  
   Using the predefined prompt format, run the evaluation pipeline across different experimental settings, including:
   - target models
   - truncation strategies
   - context window sizes

   The evaluation results are then saved into a unified `.csv` file.

   **Recorded CSV columns:**
   - `max_tokens`
   - `overall_accuracy`
   - `answer_only_accuracy`
   - `response_rate`
   - `response_count`
   - `truncated_rate`
   - `mean_preserved_ratio`
   - `mean_original_context_tokens`
   - `mean_kept_context_tokens`
   - `cache_hit_rate`
   - `model`
   - `difficulty`
   - `length`
   - `truncation_strategy`

3. **Visualize experimental results**  
   Load the generated `.csv` files and visualize experimental results using `matplotlib` for comparison and analysis.

---
## Notes

- All notebooks evaluate models on the **LongBench** benchmark.
- Each version represents an iteration of the same evaluation framework.
- Later versions improve reproducibility, efficiency, and long-context support.
- `eval_v3.ipynb` is the recommended version for long-context experiments.
- To run the notebooks properly, local setup is required, including 
    - Updating all file paths to match your own storage environment 
    - Providing a valid Hugging Face access token (with LLaMA model access enabled) for model loading and evaluation.