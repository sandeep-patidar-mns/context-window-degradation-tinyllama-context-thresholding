# Context Length Thresholding in SLMs: TinyLlama-1.1B

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Python](https://img.shields.io/badge/Python-3.13.16-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.11.0-EE4C2C)

## 📌 Problem Statement
Small Language Models (SLMs) are often deployed in memory-constrained environments, but their exact behavior when pushed past their theoretical context limits is poorly documented. This repository contains a rigorously controlled Needle-in-a-Haystack (NIAH) experiment to identify the exact context length thresholding of the `TinyLlama-1.1B-Chat-v1.0` model. We empirically map whether the model gracefully degrades or catastrophically fails when exceeding its 2048-token training limit.

## 📂 Repository Structure
.
├── notebook.ipynb                   # Main experimental code and execution logs
├── context_degradation_results.csv  # Raw output data from the experiment
├── manuscript.pdf                   # Final peer-reviewed IEEE/ACM style paper
├── author_response.pdf              # Peer review author response and checklist
└── README.md                        # This documentation file

## 💻 Compute Requirements & Environment
The experiments in this repository were genuinely executed on a **Google Colab Tesla T4 GPU (14.56 GB VRAM)**. 
* **Hardware:** 1x NVIDIA T4 (or equivalent 16GB+ VRAM GPU)
* **Python:** 3.13.16
* **Dependencies:**
  * `torch==2.11.0+cu130`
  * `transformers==5.20.2`
  * `pandas==2.2.3`

## 📊 Dataset Preparation
**No external dataset downloads are required.** 
To prevent pre-training data leakage and ensure precise token control, the dataset is purely synthetic and generated on-the-fly during execution. The "haystack" consists of repeating filler text, and the "needle" is a specific target fact injected exactly at the 50% token depth. 

## 🔬 Running the Experiment
The primary baseline and experimental scaling loop (1500 to 2500 tokens) is contained within the Jupyter Notebook.

**Execute via Jupyter/Colab (Verified):**
Open `notebook.ipynb` and run all cells sequentially. The global random seed is locked to `42`, and generation is set to `temperature=0.1` and `do_sample=False` to ensure exact deterministic reproduction.

## ⚠️ Limitations
* **Synthetic Bias:** The repeating nature of the filler text may induce repetition loops in the attention mechanism differing from natural language contexts.
* **Metric Brittleness:** The evaluation uses binary exact-substring matching. Semantic variations (e.g., spelling out numbers) are graded as failures.
* **Single Position:** This baseline injects the fact solely at the 50% depth.

## ✅ Minimum Reproduction Checklist (For Peer Students)
- [ ] Provision a T4 GPU (e.g., via Google Colab).
- [ ] Verify PyTorch and Transformers versions match the environment section.
- [ ] Run `notebook.ipynb` top-to-bottom.
- [ ] Verify that `context_degradation_results.csv` shows `Success = True` at 2250 tokens and `Success = False` (with hallucinated text) at 2500 tokens.

## 📝 Credits, License, and Citation
**Author:** Sandeep Patidar
**License:** MIT License. Feel free to use and modify the code for your own research.
