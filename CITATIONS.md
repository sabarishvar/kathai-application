# Citations and Resources

## Use of an LLM

I used Claude (Anthropic) throughout this application as a tutor and pair programmer. I used it to:
- explain concepts in simple terms (chunking, embeddings, signal processing, spectral gating, translation models, evaluation metrics),
- suggest experiment designs and help write and debug code,
- help structure and phrase the written analysis.

I ran every experiment myself on my own machine, and all results in the notebooks are my own outputs. I read the outputs, judged them, and edited the write-ups. For 2.3, the Tamil test sentences were drafted with Claude's help, and I checked them and judged every translation myself as a Tamil speaker. Several directions came from discussing unexpected results, for example adding a pause-based measure when SNR could not detect reverb, repeating the Whisper test with five noise samples, and moving the topic out of the embedding into metadata.

## Part 1
- Wikipedia, "Toda language": https://en.wikipedia.org/wiki/Toda_language
- Wikipedia, "Bhili language": https://en.wikipedia.org/wiki/Bhili_language
- Press Information Bureau, "Documenting India's Endangered Languages" (SPPEL): https://static.pib.gov.in/WriteReadData/specificdocs/documents/2025/aug/doc2025812605201.pdf
- Lewis et al. (2020), *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*

## 2.1 The Damaged Recording
- SciPy (`scipy.signal`: `butter`, `sosfiltfilt`, `iirnotch`, `filtfilt`, `stft`/`istft`, `spectrogram`, `fftconvolve`, `resample_poly`) and the SciPy Signal Processing Tutorial
- NumPy, Matplotlib
- FFmpeg with libopus (low-bitrate compression)
- `noisereduce` (Tim Sainburg): the spectral-gating idea my implementation follows
- `faster-whisper` (SYSTRAN), based on OpenAI Whisper: Radford et al. (2022), *Robust Speech Recognition via Large-Scale Weak Supervision*

## 2.2 The Chunk That Cut the Story in Half
- Pinecone, "Chunking Strategies for LLM Applications"
- LangChain Text Splitters, including `SemanticChunker` (percentile breakpoint idea)
- `tiktoken` (OpenAI): https://github.com/openai/tiktoken
- `sentence-transformers`, model `all-MiniLM-L6-v2`; Reimers & Gurevych (2019), *Sentence-BERT*
- Hearst (1997), *TextTiling: Segmenting Text into Multi-Paragraph Subtopic Passages*, Computational Linguistics 23(1)
- Beeferman et al. (1999) and Pevzner & Hearst (2002), for the Pk and WindowDiff segmentation metrics mentioned in my evaluation design
- `yt-dlp` and `faster-whisper` (transcription)
- Lectures used:
  - Andrej Karpathy, "[1hr Talk] Intro to Large Language Models": https://www.youtube.com/watch?v=zjkBMFhNj_g
  - NPTEL, "Mod-01 Lec-07 Pourbaix diagram" (Dr. Kallol Mondal, IIT Kanpur): https://www.youtube.com/watch?v=3PDqGO39F8o

## 2.3 When the English Sounds Right but the Meaning Is Wrong
- NLLB Team et al. (2022), *No Language Left Behind: Scaling Human-Centered Machine Translation*; model `facebook/nllb-200-distilled-600M` (Meta AI)
- Hugging Face `transformers` and the Hugging Face Translation Task Guide
- `sacrebleu` (chrF score); Popović (2015), *chrF: character n-gram F-score for automatic MT evaluation*

convertion link: https://claude.ai/share/2c406717-6627-4526-8822-37eb6d9e50c3  