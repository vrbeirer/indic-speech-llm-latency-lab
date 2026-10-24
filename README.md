![SIGNAL // INDIC — animated speech intelligence banner](assets/signal-hero.gif)

# SIGNAL // INDIC

### A speech-AI latency lab for Hindi, Kannada, and real-time conversation research.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Languages](https://img.shields.io/badge/Languages-Hindi%20%2F%20Kannada-7C3AED)](#results)
[![Status](https://img.shields.io/badge/Status-Experimental-0F766E)](#limitations)

[**Launch the notebook**](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing) · [**Research question**](#the-question) · [**Results**](#results) · [**Reproduce**](#run-the-lab)

---

## The Question

**What architectural limitations prevent Speech LLMs from achieving low-latency, natural, real-time conversations?**

This project explores the question from two angles: a comparative architecture study of **Moshi** and **SALMONN-omni**, plus a practical **Hindi/Kannada speech-recognition evaluation**.

| 🔬 Research | 🎙️ Practical experiment | 🧪 Proposed idea |
|:--|:--|:--|
| Compare two full-duplex Speech LLM architectures | Evaluate a baseline against language-specialized Whisper models | Explore an adaptive turn-transition controller |

## The Two Architectures

| Model | How it works | Main trade-off |
|:--|:--|:--|
| **Moshi** | Mimi codec + hierarchical Temporal/Depth Transformer jointly models speech and text streams. | Tight audio-text integration, with substantial data and per-frame generation demands. |
| **SALMONN-omni** | Mamba streaming encoder + Llama-3-8B-Instruct + adapter + separate streaming synthesiser. | Reuses a pretrained LLM, while needing careful synchronisation between language generation and synthesis. |
| **Proposed controller** | Combines acoustic context, semantic-completeness confidence, and latency budget to choose whether to respond, listen, continue speaking, or yield. | A research proposal/prototype—not a validated end-to-end conversational system. |

![System pipeline showing the ASR experiment and proposed adaptive turn-transition controller](assets/system-pipeline.svg)

> **Latency note:** Moshi's reported ~200 ms and SALMONN-omni's reported ~405 ms TTFA come from different evaluation setups. Treat them as paper-reported figures, not a direct speed ranking.

## Results

The notebook evaluates **100 FLEURS test utterances**: 50 Hindi and 50 Kannada. Lower WER/CER is better; lower latency is faster.

### WER · Word error rate

![WER comparison between baseline and specialist models for Hindi and Kannada](assets/wer-comparison.png)

### Latency · Mean time per example

![Mean inference latency comparison](assets/latency-comparison.png)

### CER · Character error rate

![CER comparison between baseline and specialist models](assets/cer-comparison.png)

| Language | Condition | WER ↓ | CER ↓ | Mean latency | RTF |
|:--|:--|--:|--:|--:|--:|
| Hindi | Whisper-small baseline | 0.530 | 0.360 | 2.5 s | 0.24 |
| Hindi | Specialist (large-v2) | **0.040** | **0.020** | 21.1 s | 1.97 |
| Kannada | Whisper-small baseline | 2.057 | 1.335 | 7.6 s | 0.69 |
| Kannada | Specialist (medium) | **0.066** | **0.037** | 23.1 s | 1.86 |

**Reading the results:** the specialist models substantially reduce transcription error on this test set, but they take longer per example. Their RTF values exceed 1, meaning inference takes longer than the corresponding audio duration under the measured setup. Kannada baseline outputs also included repetitive transcription loops, which inflate the error rates.

*These figures reflect the experiment reported in the notebook; results may vary with model versions, runtime, hardware, and dataset selection.*

## Run the Lab

1. **Open the notebook:** [Launch SIGNAL // INDIC in Google Colab](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing).
2. **Choose a runtime:** use a GPU such as a T4 where available. CPU execution may be substantially slower.
3. **Run the cells in order:** the notebook downloads FLEURS, runs the baseline and specialist conditions, calculates metrics, and exports result files.

The notebook writes outputs under `/content/task2_results_specialist/`.

## Limitations

- FLEURS contains read speech in relatively clean conditions; spontaneous and noisy speech may behave differently.
- Read transcription accuracy together with latency; the specialist models in this run were not real-time under the measured setup.
- The Moshi and SALMONN-omni latency figures are taken from their respective papers and are not directly comparable.
- The turn-transition controller passed **7 synthetic unit tests**; this does not establish end-to-end conversational improvement.

---

**SIGNAL LOST? RECONNECT TO THE EXPERIMENT.**

[GitHub · @vrbeirer](https://github.com/vrbeirer) · [Open the Colab notebook](https://colab.research.google.com/drive/1GqasiGGlLbPUYyKf6NDzyu1l1kQ9VLwr?usp=sharing)

_Built for exploration. Measured with care. Experimental by design._
