# KathAI Project Member Application — Sabarishvar S S (MM24B046)

My application for the KathAI project (AI Club × Eons × LC Lab, CFI, IIT Madras).

## What's where

kathai-application/
├── part1_general/
│ └── answers.md Part 1: General Questionnaire
├── q2_1_audio/ 2.1 The Damaged Recording
│ ├── damaged_recording.ipynb
│ ├── reference.wav my reference recording (trimmed, +12 dB)
│ └── reference_raw.wav the original untrimmed recording
├── q2_2_chunking/ 2.2 The Chunk That Cut the Story in Half
│ ├── 01_fixed_chunking_demo.ipynb Q1: why fixed-size chunking breaks
│ ├── 02_chunking_strategies.ipynb Q2: sentence vs semantic chunking
│ ├── 03_pipeline_and_evaluation.ipynb Q3 + Q4: real lectures, evaluation, queryable demo
│ └── lectures/ Whisper transcripts and chapter metadata
├── q2_3_translation/ 2.3 When the English Sounds Right but the Meaning Is Wrong
│ ├── 01_translation.ipynb
│ └── results.md plain summary of the 2.3 results
├── CITATIONS.md all sources and how I used an LLM
└── requirements.txt


I attempted all three technical questions. Each notebook contains the code, its outputs, and my analysis.

## How to run

1. Python 3.10+ and `pip install -r requirements.txt`
2. Install **ffmpeg** (needed for 2.1 compression and 2.2 audio download)
3. Everything runs on CPU. No GPU needed.

Notes:
- **2.1:** the audio players were removed from the saved notebook to keep it small enough for GitHub to display. Running the notebook brings them back. The degraded audio files are not included because the notebook regenerates them from `reference.wav`.
- **2.2:** the lecture audio is not included. The notebook re-downloads it with `yt-dlp`, but the Whisper transcripts are already in `lectures/`, so transcription is skipped.
- **2.3:** the first run downloads the NLLB-200 model (~2.5 GB).