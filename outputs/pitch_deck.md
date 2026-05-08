
🎓 Slide Deck: Decoding the Past — An HTR Senior Project Pitch

Slide 1 — Title Slide
Visual: Full-bleed background image of a beautiful, aged handwritten manuscript page (e.g. a letter or ledger in elegant cursive). A dark semi-transparent overlay sits over it. White title text on top.
Title: Decoding the Past
Subtitle: Building an AI-Powered Handwriting Transcription System
Bottom line: [Your name / department / semester]
Speaker notes:

"Welcome everyone. I want to show you a project that sits at the intersection of AI, history, and real-world impact — and pitch it to you as a senior capstone worth your time."


Slide 2 — Hook: What Is This?
Visual: Side-by-side split. Left: a photo of a crinkled, faded 18th-century letter in looping cursive. Right: clean, readable typed text. A bold arrow between them.
Heading: Millions of documents. Completely unreadable by computers.
Bullet points (large font, 3 max):

Letters, ledgers, court records, diaries — mostly handwritten
Unindexed, unsearchable, locked away
Historians can't analyze what they can't read

Speaker notes:

"Here's the problem. The overwhelming majority of historical documents have never been digitized in any useful way. A computer can't search a handwritten letter. It can't run statistics on a 19th century ledger. Historians who want to study these materials either read them by hand — slowly — or just don't. This project is about fixing that."


Slide 3 — Why It Matters
Visual: World map with dots highlighting archival collections in Europe, Latin America, Africa, South Asia. Or: a montage of 3-4 different types of historical documents (a plantation ledger, an immigrant ship manifest, a medieval manuscript, a WWI field diary).
Heading: History from the ground up
3 short callout boxes:

📜 Distant reading & topic modeling at scale
🕵️ Network analysis of historical actors
🗣️ Amplifying underrepresented voices

Speaker notes:

"This isn't just about efficiency. When you make handwritten documents machine-readable, you open up entire new research questions. Who was trading with whom in 1780? What diseases were spreading through a village in 1850? Whose names keep appearing in court records? These are questions that can now be answered computationally — but only if someone builds the tools."


Slide 4 — The Technical Challenge
Visual: Annotated image of a difficult handwritten document with callout arrows pointing to: faded ink, crossed-out words, abbreviations, margin notes, a water stain.
Heading: This is harder than it looks
Callout labels (matching arrows):

Faded or damaged paper
Abbreviations & non-standard spelling
Marginalia & unusual layouts
Cursive styles that vary by writer, era, language

Speaker notes:

"Modern OCR — the technology that reads printed text on your phone — falls apart here. Historical handwriting is messy, degraded, inconsistent, and often in languages or scripts that AI hasn't been trained on well. The field has been working on this for years, and the state of the art is still far from solved."


Slide 5 — The Landscape: What Already Exists
Visual: A clean 2x3 icon grid showing logos or icon representations of existing tools: Transkribus, Tesseract, eScriptorium, GPT-4o, Claude, Gemini. Below them, a spectrum bar labeled "Specialized ←→ General."
Heading: There are tools — but no complete solution
3 bullet points:

Specialized OCR models: great for one corpus, brittle elsewhere
General LLMs (GPT-4o, Claude, Gemini): flexible, but expensive and inconsistent
No system that ties them together intelligently

Speaker notes:

"Tools like Transkribus have been around for a while and they work — but they require a lot of setup, they're not flexible, and they don't leverage the latest AI. Meanwhile, frontier language models like GPT-4 and Claude are surprisingly good at reading handwriting from images, but they're not designed for this. Nobody has built the glue layer — a smart pipeline that lets you combine these, evaluate them, and improve over time. That's what this project is."


Slide 6 — The Project Vision
Visual: A clean pipeline diagram (left to right): 📷 Image Input → 🤖 Model Selection → 📝 Transcription Output → 🔥 Confidence Map → ✏️ Human Edit → 🔁 Feedback Loop. Each step is a rounded box with an icon.
Heading: A full HTR pipeline — built by you
Speaker notes:

"Here's the vision at a high level. You upload a manuscript image. The system picks the best model for the job — or lets you compare several. It returns a transcription, and critically, it tells you where it's uncertain. A human editor can correct those spots. And those corrections feed back in as training examples for the next run. The system gets smarter as it's used."


Slide 7 — Core Technical Components
Visual: Same pipeline diagram from Slide 6, but now each box expands into a small bullet list below it. Clean, airy layout.
Heading: What you'd actually build
ComponentWhat it doesModel RouterChoose between LLM APIs, local models, or OCRPrompt BuilderFew-shot examples, anti-hallucination promptsAccuracy EvaluatorCER, WER, BLEU scores per runConfidence Heat MapVisualize uncertainty at character levelEditing InterfaceHuman-in-the-loop correction UIFeedback SystemCorrections become few-shot training examples
Speaker notes:

"Let me break down the components. This is a system with distinct modules — which is actually great for a team project, because different people can own different pieces. You've got an ML component, a backend, a frontend, and an evaluation framework all wrapped into one."


Slide 8 — The Interesting CS Problems
Visual: Four quadrant layout, each quadrant with a bold icon and short description. Dark background, bright accent colors.
Quadrant 1 — 🧠 Prompt Engineering
Few-shot learning, anti-hallucination, temperature control
Quadrant 2 — 📊 Evaluation
How do you measure transcription quality? CER, WER, BLEU
Quadrant 3 — 🗺️ Confidence Mapping
Character-level uncertainty visualization
Quadrant 4 — 🔁 Feedback Loops
How do human corrections improve model performance over time?
Speaker notes:

"I want to flag some of the genuinely interesting CS problems in here, because this isn't just glue code. Figuring out how to prompt a language model to not hallucinate archaic characters is a research problem. Building a heat map that shows character-level confidence requires working with model logits or sampling strategies. Designing a feedback loop that improves few-shot performance over time is a systems design challenge. There's real depth here."


Slide 9 — What the Research Says
Visual: A simple 2-column comparison table with a clean design. Left column: "Specialized Models." Right column: "General LLMs." Checkmarks and X marks for different criteria (flexibility, accuracy on English, accuracy on other languages, cost, setup time).
Heading: LLMs vs. specialized OCR: it depends
Below the table, one highlighted callout:

"Two-shot Claude Sonnet 3.5 on whole-page scans outperformed all tested OCR engines on Belgian historical records (Kim et al., 2025)"

Speaker notes:

"The research is actually pretty interesting here. Specialized models like Transkribus are great when you have a lot of data to train on. But recent papers show that a well-prompted LLM — given just two example transcriptions — can outperform purpose-built OCR systems on handwritten records. The tradeoff is cost and consistency. Your system would let researchers make that tradeoff intelligently based on their needs."


Slide 10 — Sub-Projects (Pick Your Adventure)
Visual: Three cards side by side, each with a distinct color, icon, and title. Like a choose-your-own-adventure layout.
Card 1 — 🖥️ The Pipeline
Model routing, API integration, batch processing
Card 2 — 🎨 The Interface
Editing UI, confidence heat map, UX design
Card 3 — 🧪 The Evaluator
Benchmark framework, accuracy metrics, model comparisons
Heading: This project has natural sub-teams
Speaker notes:

"One reason this works well as a capstone is that it naturally breaks into parallel workstreams. You could have a team focused on the ML pipeline and model integration, another building the web interface and heat map visualization, and another designing the evaluation and benchmarking framework. These are loosely coupled — you can make real progress independently and then integrate."


Slide 11 — Stretch Goals (The Exciting Stuff)
Visual: Rocket ship icon or "Level Up" aesthetic. Bold, energetic. 3 items with upward-pointing arrows.
Heading: If you want to go further...

🔬 Fine-tune a local model on a growing corpus of corrected transcriptions
🌍 Multilingual support — most tools are English-first; can you do better?
🤝 Integrate with archival databases — connect to real collections

Speaker notes:

"The baseline project is already meaty. But if your team is ambitious, there's a clear path to something publication-worthy. Fine-tuning a small open-source model on a domain-specific corpus, or demonstrating improved accuracy on non-English documents, would be a genuine research contribution."


Slide 12 — The Stack
Visual: A clean tech stack diagram — layered boxes like a layer cake. Each layer labeled.
From bottom to top:

Data: HTR-United datasets, corrected transcription corpus
Models: HuggingFace local models + Claude / GPT-4o / Gemini APIs
Backend: Python, FastAPI or similar
Frontend: Web UI for upload, viewing, editing
Evaluation: Custom benchmark harness

Speaker notes:

"In terms of what you'd actually use: the dataset layer is covered — there are open benchmark datasets for historical handwriting. The model layer is a mix of API calls to frontier models and optional local open-source models. The rest is a fairly standard web stack. Python backend, web frontend. Nothing exotic — but the integration is interesting."


Slide 13 — Why This Project, Why Now
Visual: Minimalist slide. Dark background. Three large bold statements, stacked, centered.

The tools exist.
The need is real.
Nobody has built the system.

Speaker notes:

"I want to leave you with this. The individual pieces — the models, the datasets, the evaluation metrics — they're all out there. The research community has been documenting exactly what works and what doesn't. What doesn't exist yet is a coherent, usable system that ties them together. That's the gap this project fills. And it's exactly the kind of thing a motivated senior team can build."


Slide 14 — What You'd Walk Away With
Visual: Clean icon list, two columns.
Left — Technical skills:

Multimodal LLM integration
Evaluation framework design
Human-in-the-loop ML systems
Full-stack web development

Right — Portfolio value:

A working tool used by real researchers
Benchmarking study (potentially publishable)
Open source contribution
Interdisciplinary collaboration

Speaker notes:

"And here's what's in it for you. This isn't a toy project. Historians actually need this. If you build it well, it will be used. You'll have hands-on experience with multimodal AI, evaluation methodology, and system design — and a project you can actually talk about in interviews. The interdisciplinary angle is a plus too; it shows you can work on real-world problems outside of pure CS."


Slide 15 — Questions & Discussion
Visual: Return to the manuscript image from Slide 1 — same full-bleed photo. Overlay text:
"What would you build first?"
Below: your contact info / office hours / course logistics
Speaker notes:

"That's the pitch. I'm happy to go deeper on any piece of this — the technical components, the research background, or how teams might be structured. What questions do you have?"