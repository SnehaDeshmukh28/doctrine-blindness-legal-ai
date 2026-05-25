# Doctrine Blindness in Legal AI

**Doctrine Blindness in Legal AI: Auditing LLM Comprehension of the Co-Accused Parity Doctrine in Indian Bail Jurisprudence**

*Submitted to EMNLP 2026 - Under Review*

---

## Overview

This repository contains the evaluation code for our paper, which introduces a three-task diagnostic suite to audit whether large language models comprehend the **co-accused parity doctrine** - a uniquely Indian legal principle where bail is granted because a similarly-placed co-accused has already received bail.

Built on [IndianBailJudgments-1200](https://huggingface.co/datasets/SnehaDeshmukh/IndianBailJudgments-1200), a dataset of 1,200 annotated Indian High Court and Supreme Court bail orders (1975–2025).

---

## Three Tasks

**Task A - Parity-Stratified Accuracy**
Zero-shot bail outcome prediction on all 1,200 cases. Accuracy measured separately on parity=True (n=341, true grant rate 77.1%) and parity=False (n=859, true grant rate 55.1%) cases. Shows that standard accuracy metrics cannot detect doctrine comprehension.

**Task B - Doctrine Reveal Experiment**
For all 341 parity=True cases, runs three prompt versions:
- V1 (Baseline): case facts only
- V2 (Correct signal): facts + *"The co-accused was granted bail"*
- V3 (Adversarial signal): facts + *"The co-accused was refused bail"*

Metric: **Doctrine Sensitivity Rate (DSR)** = fraction of cases where V2 and V3 predictions differ.

**Task C - Demographic × Doctrine Interaction**
Applies V1/V2/V3 prompts with accused name swaps (Hindu↔Muslim, Male→Female).
Metric: **Demographic Doctrine Sensitivity Gap (DDSG)** = |DSR_original − DSR_swapped|

---

## Models Evaluated

| Model | Provider |
|---|---|
| GPT-4o, GPT-4o-mini | OpenAI |
| Llama-4-Scout, Llama-3.3-70B | Meta |
| Qwen-3-32B | Alibaba |
| Mistral-Large | Mistral AI |

All evaluations at temperature=0, seed=42.

---

## Repository Structure

```
doctrine-blindness-legal-ai/
├── notebooks/
│   ├── 00_dataset_verification.ipynb    # Verify all dataset statistics cited in paper
│   ├── TaskA_parity_stratified.ipynb    # Task A experiment
│   ├── TaskB_doctrine_reveal.ipynb      # Task B experiment
│   └── TaskC_demographic_doctrine.ipynb # Task C experiment
├── requirements.txt
└── README.md
```

---

## Dataset

Download **IndianBailJudgments-1200** from HuggingFace and place as `/content/indian_bail_judgments.json`:

```
https://huggingface.co/datasets/SnehaDeshmukh/IndianBailJudgments-1200
```

---

## Setup

```bash
pip install openai groq requests scipy numpy pandas tqdm
```

Add API keys to Google Colab Secrets:
```
OPENAI_API_KEY      # GPT-4o, GPT-4o-mini
GROQ_API_KEY        # Llama-4-Scout, Llama-3.3-70B, Qwen-3-32B (free tier)
MISTRAL_API_KEY     # Mistral-Large (free tier)
```

Run notebooks in order: `00_dataset_verification` → `TaskA` → `TaskB` → `TaskC`.
Each notebook saves to Google Drive and resumes automatically on disconnection.

---

## Related Work

This paper builds on **IndianBailJudgments-1200** (Deshmukh & Kamble, arXiv 2025): [arXiv:2507.02506](https://arxiv.org/abs/2507.02506)

---

## Citation

```bibtex
@inproceedings{anonymous2026doctrine,
  title     = {Doctrine Blindness in Legal {AI}: Auditing {LLM} Comprehension
               of the Co-Accused Parity Doctrine in {Indian} Bail Jurisprudence},
  author    = {Anonymous},
  booktitle = {Proceedings of EMNLP 2026},
  year      = {2026},
  note      = {Under review}
}
```

---

## Ethics

This framework is a diagnostic audit tool only - not for assisting or automating bail decisions. All case data is from publicly available Indian court records.

---

## License

MIT License
