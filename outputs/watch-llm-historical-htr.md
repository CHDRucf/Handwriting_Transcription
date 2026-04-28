# Watch: LLMs & OCR for Historical Handwriting Transcription

**Created:** 2026-04-21  
**Last checked:** 2026-04-21  
**Schedule:** Weekly (every Monday)

## Scope

Peer-reviewed articles in reputable journals **or** high-quality preprints/technical reports from established research groups on:
- Using LLMs (multimodal or text-only) to transcribe handwritten historical documents
- VLM-based HTR (handwritten text recognition) for archival/historical materials
- LLM post-correction of OCR/HTR output on historical texts
- Prompt engineering or fine-tuning strategies for historical transcription
- Benchmarks comparing LLM-based vs. traditional HTR on historical manuscripts

**Exclusions:** Modern handwriting recognition (non-historical), purely traditional OCR/HTR without LLM involvement, general OCR benchmarks on printed modern text.

---

## Baseline Papers (as of 2026-04-21)

### Core — Directly on LLM-based Historical HTR

| # | Paper | ID / DOI | Date | Venue | Key Finding |
|---|-------|----------|------|-------|-------------|
| 1 | Humphries et al., "Unlocking the Archives: Using LLMs to Transcribe Handwritten Historical Documents" | arXiv:2411.03340 / DOI:10.1080/01615440.2025.2500309 | Nov 2024 (preprint); May 2025 (journal) | *Historical Methods* 58(3) | Claude Sonnet 3.5 CER 5.7% beats Transkribus Titan 6.6% on 18th–19th c. English; LLM-corrected HTR reaches CER 1.8% |
| 2 | Kim et al., "Early evidence of how LLMs outperform traditional systems on OCR/HTR tasks for historical records" | arXiv:2501.11623 | Jan 2025 | Preprint (VUB/ULB) | Two-shot Claude Sonnet 3.5 scores 4.06/5.0 human eval on Belgian records, best overall |
| 3 | Greif et al., "Multimodal LLMs for OCR, OCR Post-Correction, and NER in Historical Documents" | arXiv:2504.00414 | Apr 2025 | Preprint (Oxford/Mannheim) | Gemini 2.0 Flash CER 1.27% on German city directories; multimodal post-correction reaches 0.84% |
| 4 | Li, "Handwriting Recognition in Historical Documents with Multimodal LLM" | arXiv:2410.24034 | Oct 2024 | CHR 2024 workshop | Gemini on historical handwriting; identifies hallucination as primary failure mode |
| 5 | Colavizza et al., "Benchmarking Large Language Models for Handwritten Text Recognition" | arXiv:2503.15195 | Mar 2025 | Preprint (Bologna/Harvard) | Systematic benchmark of MLLMs for HTR; strong on English, weaker on non-Latin scripts |
| 6 | CHURRO: "Making History Readable with an Open-Weight Large VLM for High-Accuracy, Low-Cost Historical Text Recognition" | arXiv:2509.19768 | Sep 2025 | Preprint (Stanford) | Open-weight VLM specifically fine-tuned for historical text recognition |
| 7 | Levchenko, "LLMs for Historical Document OCR: A Methodological Framework for Digital Humanities" | arXiv:2510.06743 | Oct 2025 | Preprint | Evaluation methodology for LLM-based historical OCR; 12 models tested; Gemini and Qwen best |
| 8 | "A Benchmark of State-Space Models vs. Transformers and BiLSTM-based Models for Historical Newspaper OCR" | arXiv:2604.00725 | Apr 2026 | Preprint | SSMs vs Transformers for historical newspaper OCR |

### Adjacent — Post-correction, Fine-tuning, Specialized HTR

| # | Paper | ID / DOI | Date | Venue | Key Finding |
|---|-------|----------|------|-------|-------------|
| 9 | "Postcorrection of Weak Transcriptions by LLMs in the Iterative Process of HTR" | DOI:10.3103/S0005105525701511 | Apr 2026 | *Automatic Documentation and Mathematical Linguistics* 59 (Springer) | LLM postcorrection integrated into iterative HTR pipeline |
| 10 | "Reference-Based Post-OCR Processing with LLM for Precise Diacritic Text in Historical Document Recognition" | arXiv:2410.13305 | Oct 2024 | Preprint | LLM post-correction for diacritic languages |
| 11 | "Finetuning Vision-Language Models as OCR Systems for Low-Resource Languages: A Case Study of Manchu" | arXiv:2507.06761 | Jul 2025 | Preprint (HKUST) | VLM fine-tuning for historical Manchu documents |
| 12 | Torres Aguilar, "HTR for Historical Documents using Visual Language Models and GANs" | HAL:hal-04716654 | 2024 | Preprint (U. Luxembourg) | VLMs + GANs for medieval/early modern HTR |
| 13 | "Handwritten Text Recognition of Historical Manuscripts Using Transformer-Based Models" | arXiv:2508.11499 | Aug 2025 | Preprint | Transformer-based HTR for historical manuscripts with scarce training data |
| 14 | "CTC Transcription Alignment of the Bullinger Letters" | arXiv:2508.07904 | Aug 2025 | Preprint (Fribourg) | Improving annotation quality for historical HTR |
| 15 | Zou & Tekgurl, "Multi-Modal LLMs for Historical Handwritten Text Recognition and Data Augmentation" | Stanford CS231n project | 2025 | Course paper (Stanford) | Gemini 2.0 on Ottoman Turkish manuscripts |

### Non-peer-reviewed but notable

| # | Source | URL | Note |
|---|--------|-----|------|
| 16 | OmniAI OCR Benchmark | https://getomni.ai/ocr-benchmark | 1,000-doc benchmark of VLMs vs OCR providers |
| 17 | CodeSOTA: Claude vs GPT-4o for OCR (2026) | https://www.codesota.com/ocr/claude-vs-gpt4o-ocr | Practical comparison including handwritten notes |

---

## Update Log

### 2026-04-21 — Baseline established
- 15 papers catalogued (4 from prior research in Apr 2025, 11 newly discovered)
- Notable new additions since prior research: CHURRO (Stanford open-weight VLM), Levchenko methodological framework (12 models), Colavizza benchmark (Bologna/Harvard), Springer journal article on LLM postcorrection
- Field is active and accelerating; expect 2-4 new relevant papers per month
