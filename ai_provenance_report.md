# PHASE VI — HUMAN–AI ACCOUNTABILITY
## AI Provenance and Research Integrity Report

**Author:** Ayan Amir
**Project:** Context Length Thresholding in Small Language Models (TinyLlama-1.1B)

### 1. Overview of Human-AI Collaboration
This project was executed using an iterative human-AI collaboration model. The AI (Google Gemini) acted as a technical co-pilot, generating baseline code, formatting LaTeX documents, and executing data evaluation scripts. The human (Ayan Amir) maintained architectural control, verified the outputs, provided the execution environment (Google Colab T4), and made all final methodological decisions.

### 2. Verified Human Decisions & AI Limitations (Real Examples)

**Example 1: The LaTeX Formatting and Column Imbalance**
* **Task:** Formatting the final manuscript (Prompt 15 & 16).
* **AI Proposal:** The AI initially generated the IEEE/ACM standard `twocolumn` LaTeX layout. 
* **AI Limitation/Issue:** Because the paper was concise, the text and the main degradation plot (Figure 1) were squeezed into the left column, leaving the right side awkwardly blank and the image too small.
* **Human Decision & Correction:** The human identified the visual flaw and instructed the AI to fix the alignment. The AI was directed to rewrite the LaTeX source using a single-column, centered format (`\documentclass[11pt]{article}`) which successfully balanced the whitespace and enlarged the figure for readability.

**Example 2: Methodological Choice for Contribution (Prompt 12)**
* **Task:** Selecting a defensible research contribution pathway.
* **AI Proposal:** The AI evaluated three pathways (A. Reproduction under new conditions, B. Comparative study, C. Targeted modification).
* **Human Decision & Verification:** The human reviewed the compute constraints (T4 GPU limits) and the synthetic nature of the dataset, approving the AI's recommendation to select **Pathway B (Positional Robustness)**. The human verified that changing the fact injection depth (from 50% to 10% or 90%) was a computationally feasible and scientifically valid extension without altering the baseline weights.

**Example 3: Managing Synthetic Data to Prevent Leakage (Prompt 13 & 17)**
* **Task:** Ensuring no pre-training data leakage in the evaluation dataset.
* **AI Proposal/Action:** The AI suggested and wrote code to generate the "Needle-in-a-Haystack" text dynamically in Python using a repeating filler sentence, rather than downloading a public dataset like WikiText.
* **Human Decision & Verification:** The human approved this approach as it guaranteed TinyLlama had never seen the exact string configuration during its pre-training. The human verified the CSV output (`context_degradation_results.csv`) to ensure the exact substring matching logic worked correctly.

### 3. Conclusion on Vibe Coding and Risks
* **Dependence on Human Reasoning:** While the AI flawlessly wrote the PyTorch inference loops, the human had to ensure the constraints of the assignment (like avoiding external datasets and fixing formatting issues) were met. 
* **Remaining Risks:** The primary risk of this "vibe coding" approach is trusting the AI's metric calculation blindly. To mitigate this, the human actively audited the `evaluation_script.py` to ensure missing values and data leakage were properly handled before trusting the final accuracy cliff at 2363 tokens.
