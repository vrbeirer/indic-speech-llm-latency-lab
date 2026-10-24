# SIGNAL // INDIC

**Speech intelligence, latency, and turn-taking research for Indic languages.**

[Open the Google Colab notebook](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing) · [GitHub profile](https://github.com/vrbeirer)

> Two speech-LLM architectures. Two Indic languages. One question: how can conversational AI respond naturally without unnecessary delay?

## Research question

**What architectural limitations prevent Speech LLMs from achieving low-latency, natural, real-time conversations?**

This project has two parts: a comparative study of **Moshi** and **SALMONN-omni**, and a practical speech-recognition experiment for **Hindi and Kannada**.

## Architecture comparison

| System | Approach | Main trade-off |
|---|---|---|
| **Moshi** | Uses the Mimi neural audio codec and a hierarchical Temporal/Depth Transformer to model speech and text streams jointly. | Tightly integrates audio and text, but requires substantial training data and per-frame generation. |
| **SALMONN-omni** | Connects a Mamba streaming speech encoder and a Llama-3-8B-Instruct LLM to a separate streaming speech synthesiser. | Reuses a pretrained LLM, but must coordinate language generation and speech synthesis. |
| **Proposed controller** | Combines acoustic context, semantic-completeness confidence, and a latency budget to select whether to respond, keep listening, continue speaking, or yield. | A research proposal/prototype; not a validated end-to-end conversational system. |

**Latency note:** Moshi's reported ~200 ms latency and SALMONN-omni's reported ~405 ms time-to-first-audio come from different evaluation setups, so they should not be treated as a direct speed ranking.

## Indic speech-recognition experiment

The notebook evaluates **100 FLEURS test utterances**: 50 Hindi and 50 Kannada. It compares a Whisper-small baseline with language-specialized Whisper models.

## 📊 Results dashboard

These charts summarise the **saved notebook run** on 100 examples. Lower WER/CER and lower latency are better. Bars are visual guides; exact values are listed below.

### Recognition errors · WER ↓

```text
HINDI  (each bar scaled to the highest WER in this language)
Whisper-small baseline     0.5298  ████████████████████████
Language-specialized       0.0396  ██

KANNADA  (each bar scaled to the highest WER in this language)
Whisper-small baseline     2.0571  ████████████████████████
Language-specialized       0.0662  █
```

### Inference time · mean seconds per audio example ↓

```text
Scale: █ ≈ 1 second (bar lengths rounded for readability)

HINDI
Whisper-small baseline      2.50 s  ███
Language-specialized       21.07 s  █████████████████████

KANNADA
Whisper-small baseline      7.63 s  ████████
Language-specialized       23.07 s  ███████████████████████
```

### Saved metrics (exact values)

| Language | Model condition | WER ↓ | CER ↓ | Mean latency ↓ | Mean RTF ↓ |
|---|---|---:|---:|---:|---:|
| Hindi | Whisper-small baseline | 0.5298 | 0.3603 | 2.50 s | 0.241 |
| Hindi | Language-specialized Whisper | 0.0396 | 0.0199 | 21.07 s | 1.965 |
| Kannada | Whisper-small baseline | 2.0571 | 1.3348 | 7.63 s | 0.689 |
| Kannada | Language-specialized Whisper | 0.0662 | 0.0371 | 23.07 s | 1.860 |

**What the results suggest:** the specialist models show lower transcription error on this saved run, but take substantially longer per audio example. Their mean real-time factor (RTF) is above 1, so inference is slower than the duration of the audio under this measurement setup. The Kannada baseline also produced repetitive transcription outputs, increasing its error rates.

These figures describe one saved run; results can vary with model versions, sample selection, decoding settings, hardware, and runtime conditions.

## Reproduce the experiment

1. Open the [Google Colab notebook](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing).
2. Select a GPU runtime, such as a T4, if available. CPU execution may be considerably slower.
3. Run the notebook cells in order. The notebook downloads FLEURS, runs the transcription conditions, calculates evaluation metrics, and exports results.
4. Download the generated outputs from `/content/task2_results_specialist/`.

## Limitations

- FLEURS evaluates read speech; spontaneous conversation, accents, and noisy environments may produce different results.
- Accuracy and latency should be considered together. The specialist models in this run were not real-time under the measured setup.
- The latency figures for Moshi and SALMONN-omni are reported by their respective papers under different conditions.
- The proposed turn-transition controller passed seven synthetic unit tests. This does not demonstrate an improvement in full conversational interaction.

## References

- Défossez et al. (2024). [*Moshi: A speech-text foundation model for real-time dialogue*](https://arxiv.org/abs/2410.00037).
- Yu et al. (NeurIPS 2025). [*SALMONN-omni: A standalone speech LLM without codec injection for full-duplex conversation*](https://proceedings.neurips.cc/paper_files/paper/2025/hash/233aee920dab065709145371b5900b8f-Abstract-Conference.html).
- [Google FLEURS dataset](https://huggingface.co/datasets/google/fleurs).

---

**SIGNAL LOST? RECONNECT TO THE EXPERIMENT.**

Created by [@vrbeirer](https://github.com/vrbeirer) · [Open notebook in Colab](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing)
