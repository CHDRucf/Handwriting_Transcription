# Historical Handwriting Transcription System

## General Goal: 
A historical handwriting transcription pipeline and platform that allows few shot sampling, permits testing and evaluating different models, certainty heat mapping of text and a transcription editing interface that feeds back into the few shot sampling, and potentially allows for finetuning of local models on a growing corpus of data.

## Stakes: 
Historians are deeply interested in transcribing a variety of documents into machine-readable text, which facilitates data-driven practices such as distant reading, network analysis, topic modelling and thematic analysis, and the like. Handwritten Text Recognition (HTR) is technically challenging, but handwritten documents contain crucial data for many subfields and periods. Making handwritten historical documents machine-readable also plays a key role in producing history 'from the ground up' by facilitating the inclusion of underesourced voices.

## Challenges
Kim et. al. 2025: "Historical records are more challenging to transcribe than modern documents due to cursive handwriting styles, the degraded quality of the texts (e.g., faded inks or damaged paper), language changes, and document layouts."

## Known Existing Products:
* Transkribus (https://www.transkribus.org/): model choice, drop images, output transcriptions
* Tesseract (https://github.com/tesseract-ocr/tesseract): open source OCR
* eScriptorium (https://escriptorium.rich.ru.nl/): open source, both automatic and manual transcription, model training
* Transcription Pearl (https://github.com/mhumphries2323/Transcription_Pearl): 
* HTRflow (https://ai-riksarkivet.github.io/htrflow/latest/)
* Others: Ocelus/Teklia (https://www.teklia.com/en), Konfuzio (https://konfuzio.com/en/document-ocr/), DOCSUMO (https://www.docsumo.com/resources/pdf-to-text-converter)

## Key Metrics

### CER, WER, BLEU, and Human Evaluation
Character Error Rate and Word Error Rate are calculated based on how many edits would be necessary to correct the transcription text. Current state of the art CER and WER rates: [insert table]
BLEU is a more sophisticated calculation that focuses on the precision of recalling individual words and sequences of words, and it correlates a bit better to human judgment.
Kim et. al. also use human evaluations of the outputs, scored on a scale of 0 - 5 for overall quality of transcriptions.

### Speed

### Cost
Price per image transcribed. Current comparable costs for different models: [insert table]

### English vs. Other Languages
Most LLM training data is written in English. As a result, HTR on English language documents has low overall error rates than HTR on some other languages. Many languages with a relatively small number of native speakers, or from non-Western nations are not well-served by available contemporary LLMs.

## State of the Field:
What follows are brief summaries of key findings on pertinent topics in the field.

### LLMs vs. OCR
Is the field mixed on whether one approach is superior to the other? Note Greif et. al.'s literature review: "Li [48] found that for handwritten texts in French, Italian, Spanish, and Dutch published between the sixteenth and nineteenth centuries, fine-tuned TrOCR and CNN-BiLSTM models drastically outperform an unspecified Gemini model. Kim et al. [49] found that for (mostly) handwritten probation records from 1921 Belgium, Claude (prompted with two few-shot examples) produced a more accurate transcription than other OCR engines (EasyOCR, TrOCR, KerasOCR, Tesseract) and outperformed fine-tuned TrOCR versions. Humphries et al. [50] found that for their corpus of eighteenth and nineteenth century English handwriting, Gemini-1.5-pro, GPT-4o, and Claude-Sonnet-3.5 all achieved transcription accuracies comparable to and sometimes better than conventional state-of-the-art OCR algorithms. Ghiriti et al. [51] tested the transcription capabilities of GPT-4 Vision-Preview and its response to various artificially introduced distortions and degradations for a corpus of early twentieth century German-language Fraktur prints and found that it outperformed Tesseract, except for those documents with complex layouts."

Several current papers suggest methods for LLMs to correct OCR transcriptions (Greif et. al. 2025).

### Local, finetuned models vs. general models
Greif et. al.: "Although TrOCR models with corpus-specific fine-tuning have been shown to yield very accurate results for handwritten texts [20], the limited existing evidence suggests that for Latin script prints, Transkribus’ Text Titan I outperforms a corpus-fine-tuned TrOCR model [72]."

### Zero shot, few shot learning
Kim et. al. 2025: "two-shot GPT-4o for line-by-line images and two-shot Claude Sonnet 3.5 for whole-scan images yield the transcriptions of the historical records most similar to the ground truth."

### Key Prompt Enginnering Techniques

#### Role Prompting and Context Setting

#### Temperature setting
Most researchers report settings the temperature to 0.0 to prevent the bastardization of text in transcription.

#### Anti-hallucination Safeguards
Anti-error prompts.

#### Paleographic and Linguistic Contexts

#### Disorganized writing

#### Image and manuscript issues
##### DPI
##### Damaged documents
##### Rotation, folding

### Multi-pass, Self-verification, Multiple model verification, 'Council' approaches

## Proposed Stack
1. Dedicated GPU workstation for local models
2. ACCESS resources to finetune local models?
3. API calls to frontier models
4. Any scanning resources? (Titan scanner, e.g.)
5. HTR datasets (https://htr-united.github.io/, Alkendi et al. section 3, table 6)
6. Models (huggingface.co, https://zenodo.org/communities/ocr_models/records?q=&l=list&p=1&s=10&sort=newest, HTRflow, OpenAI, Anthropic, Google Gemini)

## Proposed Algorithmic and Pipeline Components:

1.	Model Chooser (local or cloud): LLM and OCR system?
2.	Accuracy scores per run evaluator
3.	Prompt builder/chooser
    1.	Second pass verification prompts?
    2. Using a different model?
4.	‘Few shot’ learning chooser
5.	Model training techniques:
    1. Document layout and line identification function (with second pass and human correction?)
    2. Image normalization
    3. Word segmentation, character recognition
    4. Layer Normalization, Beam Search, Focal Loss, 
6.	Certainty heat mapping
7.	Editing output to gold standard
8.	Use of gold standard examples in few shot learning
9.	Finetuning on a growing corpus
10.	Batch handling features

## Selected Bibliography and Works Cited
AlKendi W, Gechter F, Heyberger L, Guyeux C. Advancements and Challenges in Handwritten Text Recognition: A Comprehensive Survey. J Imaging. 2024 Jan 8;10(1):18. doi: 10.3390/jimaging10010018. PMID: 38249003; PMCID: PMC10817575.

Greif, G., Griesshaber, N., & Greif, R. (2025). Multimodal LLMs for OCR, OCR post-correction, and named entity recognition in historical documents. arXiv preprint arXiv:2504.00414.

Kim, S., Baudru, J., Ryckbosch, W., Bersini, H., & Ginis, V. (2025). Early evidence of how LLMs outperform traditional systems on OCR/HTR tasks for historical records. arXiv preprint arXiv:2501.11623.



