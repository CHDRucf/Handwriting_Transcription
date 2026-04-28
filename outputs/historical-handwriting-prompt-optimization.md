# Optimizing LLM Prompts for Late 17th–Early 18th Century English Handwriting Transcription
susan testing 123
## Executive Summary

Your current prompt is well-structured and already follows several best practices: it uses a clear rule hierarchy, preserves original text faithfully, annotates rather than modifies, and handles several period-specific conventions (thorn-derived abbreviations, archaic pronouns, superscript text). However, research across academic papers, practitioner tools, and paleographic guides reveals six high-impact areas where the prompt can be materially improved:

1. **Role prompting and period context** — Telling the model *what it is* and *what it's looking at* measurably improves accuracy on historical handwriting tasks.
2. **Period-specific paleographic knowledge** — The prompt is missing several letterform and abbreviation conventions critical to the 1680–1720 period (long s, u/v and i/j interchange, P-prefix abbreviations, macron contractions, additional superscript abbreviations).
3. **Uncertainty handling** — The current prompt has no mechanism for the model to flag uncertain readings, which research shows leads to silent hallucination.
4. **Anti-hallucination and anti-modernization safeguards** — LLMs have a strong tendency to silently "correct" archaic spelling, the single biggest error category in benchmark studies.
5. **Multi-pass / self-verification** — A two-pass strategy (read, then verify) catches errors that single-pass transcription misses.
6. **Few-shot examples** — Two-shot prompting consistently outperforms zero-shot in every benchmark study reviewed.

Three suggested prompts are provided below: (A) a **Paleographic Context Supplement** to prepend to the existing prompt, (B) a **replacement prompt with integrated best practices**, and (C) a **verification/correction prompt** for a second-pass quality check.

---

## 1. What the Research Shows

### 1.1 LLMs Outperform Traditional HTR Out-of-the-Box

Multiple recent studies confirm that frontier multimodal LLMs (GPT-4o, Claude Sonnet 3.5, Gemini) significantly outperform specialized HTR software on historical handwriting without any fine-tuning:

- **Humphries et al. (2024)** tested on 50 pages of 18th–19th century English documents (33 different hands). Claude Sonnet-3.5 achieved CER 5.7% and WER 8.9% (modified metrics, excluding capitalization/punctuation differences), vs. Transkribus Titan at CER 6.6% / WER 13.2%. When LLMs corrected Transkribus outputs, accuracy reached CER 1.8% / WER 3.5% — near-human levels.

- **Kim et al. (2025)** compared GPT-4o and Claude Sonnet 3.5 against traditional OCR/HTR systems on historical Belgian records. **Two-shot prompting** with Claude Sonnet 3.5 received the highest human evaluation scores (4.06/5.0), dramatically outperforming zero-shot approaches.

- **Greif et al. (2025)** tested Gemini 2.0 Flash and GPT-4o on German city directories (1754–1870). Gemini 2.0 Flash achieved normalized CER 1.27% off-the-shelf, outperforming even corpus-fine-tuned traditional models. **Multimodal post-correction** (feeding the image + noisy transcription) achieved CER 0.84%.

### 1.2 Key Prompt Engineering Techniques for Historical Transcription

The following techniques are supported by multiple independent sources:

#### Role Prompting and Context Setting
Providing the model with an expert persona ("You are an expert paleographer") and period-specific context ("This is a late 17th century English handwritten document") improves accuracy. The Greif et al. (2025) study found that their prompts included highly specific instructions, even using emphatic phrasing like all-caps warnings to enforce rule compliance. Humphries et al. used explicit instructions to "work character by character, word by word, line by line."

#### Few-Shot Examples
**This is the single highest-impact technique consistently identified across all studies.** Kim et al. (2025) found two-shot prompting was the best strategy across both whole-scan and line-by-line experiments. Greif et al. (2025) used one-shot prompting to achieve their best results. Humphries et al. (2024) explicitly flagged few-shot prompting as the most promising avenue for future improvement of their baseline. If you can provide even 1–2 example image+transcription pairs that match the hand and period of your documents, accuracy should improve substantially.

#### Temperature = 0
Both Humphries et al. (2024) and Greif et al. (2025) set temperature to 0.0 for transcription tasks. This minimizes creativity and maximizes deterministic, faithful output. The OCR prompt engineering guide confirms that lower temperature performs best on non-creative transcription tasks.

#### Uncertainty Policy
The OCR prompt engineering guide identifies three confidence levels as best practice: high (clearly readable, output as-is), medium (predictable from context but uncertain — output with candidates), and low (indiscernible — output null with reason). The current prompt has no uncertainty mechanism, which means the model will silently guess when it can't read something.

#### Multi-Pass / Self-Verification
The OCR prompt guide recommends a two-pass approach: first read the whole page marking uncertain parts with [?], then re-read only those uncertain regions with domain-specific hints. Humphries et al. (2024) demonstrated that using a different LLM to correct an initial transcription improves accuracy by 15–25%. Greif et al. (2025) showed that multimodal post-correction (image + draft transcription) achieves the best results of any approach tested.

#### Anti-Hallucination Safeguards
Multiple sources identify common LLM failure modes in transcription:
- **Modernization bias** — Silently updating archaic spelling to modern equivalents (e.g., "employ'd" → "employed"). Humphries et al. found that roughly 33-44% of "errors" in LLM transcriptions were actually capitalization, punctuation, or spelling modernizations.
- **Text fabrication** — When text is illegible, models sometimes generate plausible but invented text. This was the primary failure mode identified in the Gemini study by the CHR 2024 paper.
- **Line skipping** — Models occasionally omit entire lines, especially in dense text.
- **Number misreading** — Digits are consistently harder than letters for LLMs. Kim et al. (2025) specifically noted that "LLMs have a hard time reading digits."

### 1.3 Paleographic Knowledge Gaps in the Current Prompt

Your current prompt covers thorn-derived abbreviations (ye/yt/wch), archaic pronouns (thou/thee/ye), and superscript text. However, several critical conventions of the late 17th–early 18th century period are missing:

#### The Long S (ſ)
In use from the late 8th century through the early 19th century. Rules: long s (ſ) at the beginning and middle of words, short s at the end. In handwriting, long s looks very similar to lowercase f (the difference is that f has a full crossbar while long s has only a half-bar or none). This is one of the most common sources of confusion for both humans and OCR systems. Without guidance, the model may output "f" for long s or silently modernize all long s to short s.

#### u/v and i/j Interchange
In the period 1680–1720, these conventions were transitioning. The traditional rule: "v" at the start of words and "u" in the middle (so "vnder" for "under", "haue" for "have"); "i" at the start/middle and "j" at the end. By the early 18th century this was becoming more standardized, but many writers still used the older convention. The model needs to know whether to preserve or modernize these.

#### Additional Abbreviation Systems
The BYU Script Tutorial and Folger Shakespeare Library identify several abbreviation types beyond what's in the current prompt:

- **P-prefix abbreviations**: Special strokes on the letter P indicate per/par (horizontal crossback), pre (hook or "2" shape above), pro (descending loop). Examples: pish = parish, pson = person, pmisses = premisses, pfitts = profitts.
- **Macron/tilde contractions**: A line or tilde above a letter indicates omitted letters, typically m or n. Examples: cōmon = common, tēple = temple, cōmand = command.
- **Additional superscript abbreviations** beyond the current prompt's coverage: wth = with, yor = your, Maty = Majesty, Esqr = Esquire, gent = gentleman, mchant = merchant, deced = deceased, executrs = executors, Admrs = administrators.
- **Ampersand variations**: Including &c for "et cetera", which appears frequently in legal and administrative documents.

#### Capitalization and Punctuation Conventions
Period writers used capitalization irregularly by modern standards — nouns, important words, and sentence-initial words might all be capitalized. Punctuation was also highly variable: virgules (/) for commas, colons used where we'd use periods, minimal use of apostrophes. The model should be told to preserve these exactly rather than standardizing.

#### Double Letters
Period writers often used double consonants where modern English uses single (e.g., "furrs" for "furs", "profitts" for "profits", "publick" for "public"). Without explicit instruction, the model will "correct" these.

---

## 2. Analysis of Current Prompt Strengths and Gaps

### Strengths
| Feature | Assessment |
|---|---|
| Rule hierarchy (Preservation > Annotation > Exclusion) | Excellent — clear priority ordering |
| Preserve original text exactly | Correct and well-stated |
| Misspelling annotation with [brackets] | Good practice |
| Archaic pronoun handling | Good, covers main cases |
| Scribal abbreviation handling (yt, wch, ye) | Good start, needs expansion |
| Superscript text with carats | Clear convention |
| Crossed-out text convention | Good |
| Bleed-through exclusion | Important and well-stated |
| Line break preservation | Important for scholars |

### Gaps
| Missing Element | Impact | Priority |
|---|---|---|
| No role/persona for the model | Medium — reduces domain expertise activation | High |
| No period/document context | Medium — model doesn't know what it's reading | High |
| No uncertainty marking mechanism | High — leads to silent hallucination | Critical |
| No anti-modernization safeguards | High — biggest error category | Critical |
| Missing long s (ſ) handling | High — very common in this period | High |
| Missing u/v, i/j convention handling | Medium — transitional in this period | Medium |
| Missing P-prefix abbreviations | Medium — common in legal/admin docs | Medium |
| Missing macron/tilde contractions | Medium — common across document types | Medium |
| Incomplete superscript abbreviation list | Medium — many more exist than listed | Medium |
| No few-shot example guidance | High — biggest accuracy improvement | High |
| No multi-pass / verification strategy | Medium — catches systematic errors | Medium |
| No digit-specific caution | Medium — numbers are error-prone | Medium |

---

## 3. Three Suggested Prompts

### Prompt A: Paleographic Context Supplement (Prepend to Current Prompt)

This supplement is designed to be placed *before* the existing prompt.txt, adding role context, period knowledge, and anti-hallucination safeguards without changing any existing rules.

```
You are an expert paleographer specializing in late 17th and early 18th century English handwriting (approximately 1680–1730). You have deep knowledge of the secretary-to-round hand transition, period abbreviation systems, and archaic letterforms.

You are about to transcribe a handwritten document from this period. Before applying the transcription rules below, keep these critical guidelines in mind:

DOCUMENT CONTEXT:
- These documents are written in English, dating from approximately 1680–1730.
- Handwriting in this period ranges from late secretary hand to early round/italic hand, and many writers mixed elements of both.
- Spelling was not yet standardized. Do NOT "correct" any spelling to modern forms. Words like "publick," "furrs," "employ'd," "compleat," "shew," "hath," "chuse," and "connexion" are period-correct and must be preserved exactly.
- Capitalization was inconsistent and often applied to nouns and important words. Preserve exactly as written.
- Punctuation was variable: virgules (/) may serve as commas, colons may appear where modern writers use periods. Preserve exactly as written.

CRITICAL ANTI-MODERNIZATION RULE:
- Do NOT silently update archaic spelling, capitalization, or punctuation to modern conventions.
- Do NOT change "hath" to "has," "doth" to "does," "shew" to "show," or similar.
- Do NOT remove or add apostrophes.
- Do NOT regularize double consonants (e.g., keep "profitts" not "profits," "furrs" not "furs").
- If a word looks misspelled but could be a period spelling variant, preserve it exactly.

ADDITIONAL LETTERFORM AND ABBREVIATION KNOWLEDGE:

Long S (ſ):
- The long s looks like an "f" but without a full crossbar (or with only a left-side bar).
- It appears at the beginning and middle of words; short s appears at the end.
- Transcribe the long s as a regular "s" — do NOT transcribe it as "f."
- Examples: what looks like "fhall" is "shall," "fatisfied" is "satisfied," "finfulnefs" is "sinfulness."

u/v and i/j interchange:
- Writers of this period often used "v" at the start of words where we use "u" (e.g., "vnder" = under, "vpon" = upon) and "u" in the middle where we use "v" (e.g., "haue" = have, "giue" = give).
- Similarly, "i" may appear where we use "j" (e.g., "iustice" = justice, "iudgement" = judgement).
- Preserve these EXACTLY as written. Do not modernize.

P-prefix abbreviations:
- A horizontal stroke through or across the descender of "p" indicates "per" or "par": pish = parish, pson = person, pcell = parcell, pticulers = particulers.
- A hook or mark above "p" indicates "pre": pmisses = premisses, prsente = presente.
- A descending loop from the head of "p" indicates "pro": pfitts = profitts, ppositions = propositions.
- Retain the abbreviation as written AND add the expansion in square brackets: pish [parish].

Macron/tilde contractions:
- A line or tilde (~) above a letter indicates omitted letters, typically "m" or "n."
- Examples: cōmon = common, cōmand = command, tēple = temple.
- Retain the word as written AND add the expansion in square brackets: cōmon [common].

Additional superscript abbreviations (beyond those already listed):
- wth = with, yor = your, Maty = Majesty, Esqr = Esquire, gent = gentleman
- mchant = merchant, deced = deceased, executrs = executors, Admrs = administrators
- sd = said, aforesd = aforesaid, rect = receipt, acct = account, Lds = Lords
- Retain and expand in square brackets as per Rule 5.

Ampersand and &c:
- The ampersand (&) means "and" — preserve as written.
- "&c" or "&c." means "et cetera" — preserve as written; add [et cetera] in brackets.

UNCERTAINTY MARKING:
- If you cannot confidently read a word, write your best guess followed by [?].
  Example: Barrington [?]
- If a word is completely illegible, write [illegible].
- If you can read some letters but not others, write what you can see with underscores for missing letters and add [?].
  Example: B_rr_ngton [?]
- Do NOT invent or fabricate text to fill gaps. An honest [illegible] is always preferred over a plausible guess presented as fact.

READING APPROACH:
- Read the document line by line, left to right, top to bottom.
- Pay special attention to numbers and digits — these are easily misread.
- Do not skip any lines. If a line is very faint, still attempt to read it and mark uncertain portions.
- If text continues in the margin or between lines, indicate its insertion point using ^carats^ as per the transcription rules below.

Now apply the following transcription rules:

```

---

### Prompt B: Integrated Full Replacement Prompt

This is a standalone replacement for the current prompt.txt, incorporating all evidence-based improvements.

```
ROLE: You are an expert paleographer specializing in English handwriting from approximately 1680–1730 — the transition period from secretary hand to round hand. You have extensive experience reading documents from this era, including letters, legal instruments, administrative records, and personal correspondence.

TASK: Transcribe each image separately. Label them exactly as:
[Page 1]
[Page 2]
...

---------------------------------------
DOCUMENT CONTEXT
---------------------------------------
- Language: English, late 17th to early 18th century.
- Handwriting: Varies from late secretary hand to early round/italic hand. Many writers mix both styles.
- Spelling: NOT yet standardized. Preserve all period spellings exactly (e.g., "publick," "compleat," "shew," "chuse," "connexion," "furrs," "profitts," "employ'd").
- Capitalization: Irregular by modern standards. Preserve exactly.
- Punctuation: Variable — virgules (/), inconsistent commas, colons for periods. Preserve exactly.

---------------------------------------
RULE HIERARCHY (Most important → least)
---------------------------------------

A. PRESERVATION RULES (Inviolable)
1. Preserve ALL original text exactly as written, including spelling, punctuation, line breaks, spacing, and capitalization.
2. Do NOT combine content from different images.
3. Do NOT modernize any aspect of the text:
   - Do NOT change archaic spellings to modern ones.
   - Do NOT regularize capitalization.
   - Do NOT add, remove, or change punctuation.
   - Do NOT regularize double consonants ("profitts" stays "profitts").
   - Do NOT change "hath" to "has," "shew" to "show," etc.

B. LETTERFORM RULES
4. Long S (ſ):
   The long s appears at the beginning and middle of words and resembles a lowercase "f" (but without a full crossbar). Transcribe it as a regular "s."
   - "fhall" → transcribe as "shall"
   - "fatisfied" → transcribe as "satisfied"
   - "finfulnefs" → transcribe as "sinfulness"
   - Do NOT transcribe the long s as "f."

5. u/v and i/j interchange:
   Period writers often used "v" at the start of words for "u" and "u" in the middle for "v." Similarly, "i" for "j" and vice versa.
   Preserve these EXACTLY as written:
   - "vnder" stays "vnder" (do NOT change to "under")
   - "haue" stays "haue" (do NOT change to "have")
   - "iustice" stays "iustice" (do NOT change to "justice")

C. ANNOTATION RULES (Add information without changing original text)

6. Misspellings:
   Keep the misspelled word exactly as written AND immediately follow it with the correct spelling inside square brackets.
   Treat any non-standard, incomplete, phonetic, or obviously incorrect spelling as a misspelling UNLESS it is a known period spelling variant (see Document Context above).
   If the correct spelling is reasonably inferable, the correction MUST be provided.
   Examples: sorre [sorry], secrit [secret], ther [there], agayn [again]

7. Archaic pronouns:
   Preserve the original word AND add the modern equivalent in square brackets:
   thou → thou [you]        thee → thee [you]
   ye (pronoun) → ye [you]  thy → thy [your]
   thine → thine [yours]    hath → hath [has]
   doth → doth [does]       hither → hither [here]
   thither → thither [there] whence → whence [from where]
   wherefore → wherefore [why]

   NOTE: If "ye" is a scribal abbreviation for "the," treat it under Rule 8.

8. Scribal abbreviations:
   Retain the abbreviation AND immediately follow it with the expanded word in square brackets.

   Thorn-derived abbreviations:
   yt [that], ye [the], ym [them], yn [then], yr [their/there], ys [this]

   P-prefix abbreviations:
   pish [parish], pson [person], pcell [parcell], pmisses [premisses], pfitts [profitts]

   Superscript abbreviations:
   wch [which], wth [with], yor [your], Maty [Majesty], Esqr [Esquire]
   Sr [Sir], gent [gentleman], mchant [merchant], sd [said], aforesd [aforesaid]
   acct [account], rect [receipt], executrs [executors], Admrs [administrators]

   Macron/tilde contractions (line above a letter indicating omitted m or n):
   Retain and expand: e.g., cōmon [common], cōmand [command]

   Ampersand: Preserve "&" as written. "&c" → &c [et cetera]

   If the expansion is uncertain, KEEP the abbreviation EXACTLY as written and DO NOT add brackets.

9. Words written above the line:
   Surround the word with carats: ^word^

10. Crossed-out text:
    Replace the crossed-out content with: [crossed out]

11. Text written vertically or across the page:
    Transcribe as: [across] followed by the text.

D. UNCERTAINTY RULES
12. If a word is uncertain, write your best guess followed by [?]:
    Barrington [?]
13. If a word is completely illegible, write: [illegible]
14. If you can read partial letters, use underscores: B_rr_ngton [?]
15. Do NOT fabricate text. An honest [illegible] is always better than an invented reading.

E. EXCLUSION RULES
16. Ignore bleed-through text, background impressions, ghost text, smudges, or ink blotting.
17. Begin a new line in the transcription whenever the line ends in the image.
18. Do NOT add commentary, explanations, interpretations, or summaries. Output ONLY the transcribed text with annotations.
19. Pay special attention to numbers and digits — verify each digit against the image before outputting.
```

---

### Prompt C: Second-Pass Verification/Correction Prompt

This prompt is used *after* an initial transcription has been generated (by any LLM or HTR tool). It implements the evidence-backed multi-pass correction strategy.

```
ROLE: You are an expert paleographer and editor reviewing a draft transcription of a late 17th to early 18th century English handwritten document (approximately 1680–1730).

TASK: Compare the handwritten page image with the draft transcription below. Your job is to correct any errors in the transcription while preserving the historical authenticity of the text.

DRAFT TRANSCRIPTION:
<draft>
{INSERT DRAFT TRANSCRIPTION HERE}
</draft>

---------------------------------------
VERIFICATION CHECKLIST — Work through these systematically:
---------------------------------------

1. COMPLETENESS CHECK:
   - Does the transcription begin and end at the same point as the handwritten page?
   - Are any lines skipped? Compare line-by-line against the image.
   - Are any words omitted within lines?
   - Are there any words in the transcription that do NOT appear in the image (fabricated text)?

2. FIDELITY CHECK:
   - Has any archaic spelling been silently modernized? (e.g., "publick"→"public", "compleat"→"complete", "shew"→"show")
   - Has capitalization been regularized?
   - Has punctuation been added, removed, or changed?
   - Have any double consonants been reduced? (e.g., "profitts"→"profits")
   - Have contractions been silently expanded without brackets?

3. LETTERFORM CHECK:
   - Has the long s (ſ) been correctly transcribed as "s" (not "f")?
   - Have u/v and i/j interchanges been preserved as written?

4. ANNOTATION CHECK:
   - Are scribal abbreviations retained with expansions in [brackets]?
   - Are archaic pronouns annotated with modern equivalents in [brackets]?
   - Are genuine misspellings (not period variants) annotated?
   - Are superscript words marked with ^carats^?
   - Is crossed-out text marked as [crossed out]?

5. NUMBER CHECK:
   - Verify every digit against the image. Numbers are frequently misread.
   - Check dates, amounts, quantities, and page numbers.

6. UNCERTAINTY CHECK:
   - Are there any passages marked [?] or [illegible] in the draft that you can now read more clearly?
   - Are there any passages that should be marked [?] or [illegible] that are currently presented with false confidence?

---------------------------------------
OUTPUT:
---------------------------------------
Provide the corrected transcription. In your response, write "Corrected Transcription:" followed ONLY by the corrected text.

If you found no errors, write "Corrected Transcription: [NO CHANGES]" followed by the original text.
```

---

## 4. Implementation Recommendations

### Priority Order
1. **Immediate, high-impact**: Add Prompt A (the context supplement) before your existing prompt. This requires no changes to your current rules and addresses the biggest gaps.
2. **When ready for a fuller overhaul**: Switch to Prompt B, which integrates everything into a single coherent prompt.
3. **For maximum accuracy**: Implement a two-pass pipeline using Prompt B for initial transcription and Prompt C for correction, ideally using a *different* LLM for each pass (e.g., Gemini for transcription, Claude for correction, or vice versa).

### Few-Shot Examples
The single most impactful improvement beyond prompt text is providing 1–2 example image+transcription pairs. If you have any pages from your corpus where you've manually verified the transcription, include these as examples in your API calls. Both Kim et al. (2025) and Greif et al. (2025) found this was the most effective strategy.

### Temperature Setting
Set temperature to 0.0 (or as close to 0 as the API allows) for all transcription tasks.

### Cross-Model Correction
Humphries et al. (2024) found that LLMs cannot effectively self-correct but can correct outputs from *different* models. The best pipeline they found was:
- Gemini for initial transcription → Claude for correction (modified CER 4.1%, WER 7.0%)
- OR: Transkribus for initial transcription → Claude for correction (modified CER 1.8%, WER 3.5%)

### Digit Verification
Consider adding a specific instruction for numbers: "After transcribing, re-examine every number in the text against the image. Digits 1/7, 0/6, 3/8, and 5/6 are commonly confused."

---

## 5. Open Questions

1. **Few-shot example selection**: How many examples is optimal? Kim et al. found 2-shot best, but more examples haven't been systematically tested for historical English handwriting specifically.
2. **Long s handling**: Should the long s be transcribed as modern "s" (as recommended here) or preserved as "ſ" with a Unicode character? This depends on your downstream use case.
3. **u/v and i/j modernization**: The current recommendation is to preserve original forms, but some editorial traditions modernize these. This is a project-level decision.
4. **Chain-of-thought**: No study has tested explicit chain-of-thought prompting for historical transcription. It might help with difficult passages but could also slow processing.
5. **Model evolution**: These findings are based on GPT-4o, Claude Sonnet 3.5, and Gemini 1.5/2.0. Newer models may shift the optimal prompting strategy.

---

## Sources

1. Humphries, M. et al. "Unlocking the Archives: Using Large Language Models to Transcribe Handwritten Historical Documents." arXiv:2411.03340, Nov 2024. https://arxiv.org/abs/2411.03340
2. Kim, S. et al. "Early evidence of how LLMs outperform traditional systems on OCR/HTR tasks for historical records." arXiv:2501.11623, Jan 2025. https://arxiv.org/abs/2501.11623
3. Greif, G. et al. "Multimodal LLMs for OCR, OCR Post-Correction, and Named Entity Recognition in Historical Documents." arXiv:2504.00414, Apr 2025. https://arxiv.org/abs/2504.00414
4. Anonymous. "Handwriting Recognition in Historical Documents with Multimodal LLM." CHR 2024, arXiv:2410.24034. https://arxiv.org/abs/2410.24034
5. "LLM OCR Prompt Engineering Guide Q1 2026." Zenn.dev. https://zenn.dev/coffin299/articles/60ba24446c0c27?locale=en
6. "Introducing Transcription Pearl." Generative History (Substack), Mark Humphries. https://generativehistory.substack.com/p/introducing-transcription-pearl
7. BYU Script Tutorial: "English Handwriting Abbreviations." https://script.byu.edu/english-handwriting/tools/abbreviations
8. Folger Shakespeare Library: "u/v, i/j, and transcribing other early modern textual oddities." https://www.folger.edu/blogs/collation/uv-ij-and-transcribing-other-early-modern-textual-oddities/
9. Wolfe, Heather. "The Alphabet Book: A guide to early modern English secretary hand." Folger Shakespeare Library, 2020. https://folgerpedia.folger.edu/mediawiki/media/images_pedia_folgerpedia_mw/7/79/AlphabetBook2020.pdf
10. UK National Archives. "Palaeography Tutorial." https://www.nationalarchives.gov.uk/palaeography/
11. US National Archives. "The Long S." Pieces of History, Dec 2021. https://prologue.blogs.archives.gov/2021/12/14/the-long-s/
12. Harvard. "Reading Colonial North America." https://projects.iq.harvard.edu/files/transcription/files/reading_colonial_north_america_final_22.pdf
13. BYU. "Paleography 1500–1800." https://handwritinghistory.org/paleography-1800-1500/
14. AIMultiple. "Handwriting Recognition Benchmark: LLMs vs OCRs." https://aimultiple.com/handwriting-recognition
