# Technical Report (≤ 4 pages)

**Challenge:** Offline Swahili ASR (Zindi)  
**Team / Author:** <Your Name / Team>  
**Date:** <YYYY-MM-DD>  
**Seed:** 42

## 1. Overview
- Objective, constraints (single T4 ≤16 GB), offline privacy.
- High-level system: frontend (audio), ASR engine (model + decoding), post-processing.

## 2. Data & Preprocessing
- Only Zindi-provided dataset used for training/validation.
- Sampling rate (16 kHz), mono; trims, VAD settings if applied.
- Train/val split strategy and counts.

## 3. Model & Training
- Baseline: faster-whisper <size> for inference.
- Fine-tune option: Wav2Vec2 XLS‑R CTC (or Whisper LoRA). 
- Hyperparameters: epochs, batch, LR, fp16, beam size, patience.
- Ablations you ran (list clearly).

## 4. Decoding & Normalization
- Beam sizes, temperature, condition_on_previous_text.
- Text normalization rules (must match WER calc & submission).

## 5. Performance
- **WER (validation)**: <value>, evaluation method.
- **RTFx (T4)**: mean/std over validation/test; measurement method.
- **Peak GPU memory**: <MiB> via nvidia-smi; ensure < 16 GB.

## 6. Error Analysis
- Typical mistakes (names, numbers, code-switching).
- Ideas tried & results (LM/no LM, VAD on/off).

## 7. Reproducibility
- Exact commands to train/infer.
- Python and package versions (requirements.txt).
- Random seeds and any nondeterminism caveats.

## 8. Limitations & Future Work
- Model drift, domain robustness, accents, background noise.
- Mobile deployment path (int8, on-device VAD), potential TTS extension.