# Research Plan: Historical Handwriting Transcription Prompt Optimization

## Context
The user has a working pipeline using Gemini (and other LLMs) to transcribe late 17th / early 18th century English handwritten documents. The current prompt (prompt.txt) is already well-structured with a rule hierarchy (Preservation → Annotation → Exclusion), handling misspellings, archaic pronouns, scribal abbreviations, crossed-out text, and layout conventions. The goal is to investigate best practices for improving this prompt and deliver 3 suggested new/supplementary prompts.

## Questions
1. **What are proven prompt engineering techniques for multimodal OCR/HTR tasks with LLMs?** (few-shot examples, chain-of-thought, structured output, role prompting, temperature/sampling)
2. **What paleography-specific knowledge improves transcription of late 17th–early 18th century English hands?** (secretary hand vs. italic hand, common letter confusions, abbreviation systems, dating conventions)
3. **What does the academic literature say about using LLMs/VLMs for historical document transcription?** (accuracy, error patterns, comparison to traditional HTR)
4. **What are best practices for handling ambiguity and uncertainty in transcription prompts?** (confidence markers, multiple readings, flagging uncertain characters)
5. **Are there Gemini-specific or vision-model-specific prompting techniques that improve handwriting recognition?** (image preprocessing instructions, spatial reasoning prompts, multi-pass strategies)
6. **What common failure modes exist in LLM-based handwriting transcription, and how can prompts mitigate them?** (hallucination, text fabrication, skipped lines, merged words, modernization bias)

## Strategy
- **3 parallel researchers** covering disjoint dimensions:
  1. **Web sources:** Prompt engineering best practices for multimodal/OCR tasks, Gemini vision docs, practitioner guides, community experiences (Q1, Q5, Q6)
  2. **Academic papers:** HTR with LLMs, historical document transcription ML, paleography + AI (Q3, Q4)
  3. **Paleography + domain knowledge:** 17th–18th c. English hands, scribal conventions, common confusions, practitioner transcription guidelines (Q2, Q6)
- Expected rounds: 1–2

## Acceptance Criteria
- [x] All 6 questions answered with ≥2 independent sources
- [x] Contradictions identified and addressed
- [x] No single-source claims on critical findings
- [x] 3 concrete prompt suggestions grounded in evidence
- [x] Prompt suggestions are tailored to the specific period (late 17th–early 18th c. English)

## Task Ledger
| ID | Owner | Task | Status | Output |
|---|---|---|---|---|
| T1 | researcher-web | Prompt engineering best practices for multimodal OCR/HTR, Gemini vision tips, failure modes | done | historical-handwriting-prompt-optimization-research-web.md |
| T2 | researcher-papers | Academic lit on LLM/VLM-based historical document transcription | done | historical-handwriting-prompt-optimization-research-papers.md |
| T3 | researcher-paleo | Paleography of late 17th–18th c. English, scribal conventions, transcription guidelines | done | historical-handwriting-prompt-optimization-research-paleo.md |
| T4 | lead | Synthesize findings and write report + 3 prompt suggestions | done | outputs/.drafts/historical-handwriting-prompt-optimization-draft.md |
| T5 | verifier | Add inline citations and verify URLs | done | historical-handwriting-prompt-optimization-brief.md |
| T6 | reviewer | Verification pass | done | historical-handwriting-prompt-optimization-verification.md |

## Verification Log
| Item | Method | Status | Evidence |
|---|---|---|---|
| Few-shot improves HTR accuracy | cross-source check | pending | |
| Gemini vision prompting best practices | official docs | pending | |
| Paleographic conventions for the period | multiple paleo sources | pending | |
| Failure modes (hallucination, skipping) | practitioner reports + papers | pending | |

## Decision Log
- 2025-04-15: Plan created. 3 researchers covering web, papers, paleography.
