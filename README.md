# Swahili ASR — Offline, On-Device (Zindi Challenge Starter)

Colab‑ready baseline + repo skeleton for building a **Kiswahili ASR** system that runs **fully offline** and fits the challenge constraints (**single NVIDIA T4 ≤16 GB**, strong WER, good Real‑Time Factor).

> ⚠️ **Follow Zindi rules.** Use only the datasets provided by the challenge for training and evaluation. Pretrained **open** models are allowed unless the challenge page states otherwise. Do not use paid APIs or private data/services. Always set random seeds.

---

## Quickstart (Colab)
1. Open the notebook: `notebooks/Swahili_ASR_Baseline.ipynb` in Google Colab.
2. Upload/clone this repo into Colab (or mount Google Drive).
3. Place the Zindi audio & metadata into `data/` as instructed in the notebook.
4. Run all cells to:
   - Install deps (open‑source only)
   - Transcribe with a **faster‑whisper** baseline (int8_float16 on T4)
   - Produce `artifacts/submission.csv` with columns: `filename,text`
   - Measure **RTFx** (Real‑Time Factor) and peak GPU memory
   - (Optional) Evaluate WER on a validation split you create from the provided training set

---

## Repo Layout
```
.
├── config/
│   └── config.yaml                # model + decoding defaults
├── data/                          # put Zindi-provided data here (not tracked)
│   └── README.md
├── notebooks/
│   └── Swahili_ASR_Baseline.ipynb # Colab-ready notebook
├── scripts/
│   ├── infer_fasterwhisper.py     # CLI baseline inference -> submission.csv
│   ├── compute_wer.py             # WER scorer (filename-aligned)
│   ├── train_wav2vec2_ctc.py      # (skeleton) HF CTC fine-tune (rules-compliant)
│   └── measure_rtf_memory.py      # log RTFx and peak GPU usage
├── swahili_asr/
│   ├── __init__.py
│   ├── data.py                    # dataset loaders (wav paths + labels)
│   ├── text.py                    # normalization for WER/submission
│   ├── decoding.py                # faster-whisper decoder helpers
│   ├── metrics.py                 # WER compute utils
│   └── utils.py                   # timing/memory helpers
├── artifacts/                     # outputs (submission.csv, logs)
├── .colab/                        # tiny helpers for Colab UX
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## Phase‑Two Checklist (keep this in your README)
- ✅ Reproducible training/inference (fixed seeds, one‑click scripts)
- ✅ WER eval script using the same text normalization as submissions
- ✅ RTFx measurement on a single **T4**, plus **peak GPU memory** logs
- ✅ ≤4‑page report with ablations (beam sizes, VAD on/off, LM on/off), error analysis
- ✅ Clean, documented code; no manual relabelling; no private services

---

## Notes on Data & Rules
- Place **only Zindi‑provided** audio/transcripts in `data/`.
- If you create a local **validation split** from training data, keep the mapping CSVs in `data/splits/` for reproducibility.
- If you optionally test a public mirror for convenience, do **not** use any extra text/audio beyond what the challenge allows.

---

## Baselines & Upgrades
- **Baseline**: `faster-whisper` multilingual (small/base) with `int8_float16`, VAD on, `beam_size=5–8`.
- **Fine‑tune path A**: LoRA on Whisper‑Small, then export to CTranslate2 for fast offline decoding.
- **Fine‑tune path B**: Wav2Vec2 XLS‑R (encoder‑CTC) + *character LM* trained **only** on challenge transcripts with `pyctcdecode` (optional).

Good luck, and karibu! :)