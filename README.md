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

### Reported results

Lower WER and CER indicate fewer transcription errors. Lower latency is faster.

| Language | Condition | WER ↓ | CER ↓ | Mean latency | Real-time factor |
|---|---|---:|---:|---:|---:|
| Hindi | Whisper-small baseline | 0.530 | 0.360 | 2.5 s | 0.24 |
| Hindi | Specialist model (large-v2) | **0.040** | **0.020** | 21.1 s | 1.97 |
| Kannada | Whisper-small baseline | 2.057 | 1.335 | 7.6 s | 0.69 |
| Kannada | Specialist model (medium) | **0.066** | **0.037** | 23.1 s | 1.86 |

On this test set, the specialist models report lower transcription error but higher inference latency. Their real-time factor (RTF) is above 1, meaning processing took longer than the corresponding audio duration under the measured setup. The Kannada baseline also showed repetitive transcription outputs, which increased its error rates.

These figures describe the notebook's reported run. Results can vary with model versions, dataset selection, decoding settings, hardware, and runtime conditions.

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
