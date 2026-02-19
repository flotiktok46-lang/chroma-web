# chroma-web

> Browser-based WebAssembly port of [ChromaFlow](https://github.com/synergy-global/chromaFlow) — real-time neural DSP and feature extraction for Web Audio API.

A collaboration between Florent and [Leon McKenzie / Synergy DSP](https://www.synergy-dsp.com/).

---

## Goal

Bring ChromaFlow's RT-aware neural DSP architecture to the browser via WebAssembly, enabling real-time adaptive audio intelligence in Web Audio applications — no server, no model download, no latency.

The target use case is HaloSound: a live performance visual system that needs to distinguish a singer's voice from the backing mix in real time, and adapt to the specific performer on stage.

---

## Roadmap — 3 POCs

### POC 1 — Compile chromaLib → WASM
Compile `chromaLib` (header-only C++17) to WebAssembly via Emscripten.  
Wire a `processBlock()` call inside a Web Audio `AudioWorkletProcessor`.  
**Success criteria:** allocation-free audio path running in browser, latency < 5ms.

### POC 2 — Feature Extraction Benchmark
Run `ChromaFeatureExtractor` on a live mic signal via WASM.  
Compare output (MFCC + spectral features) against the existing pure JS DSP pipeline side by side.  
**Success criteria:** feature parity measured, latency of both approaches logged at 60fps.

### POC 3 — Live Online Learning
A small Dense network (8→4→1) taking extracted features as input.  
Trained in real-time using ChromaFlow's IIR autograd — no dataset, no offline training.  
User supervises the first ~30 seconds ("voice" / "not voice"), then the network adapts autonomously.  
**Success criteria:** replaces the current static voice scoring formula with a model that calibrates to the individual performer on stage.

---

## Stack

| Layer | Technology |
|---|---|
| DSP library | [ChromaFlow](https://github.com/synergy-global/chromaFlow) (C++17, MIT) |
| Compilation | Emscripten (C++ → WASM) |
| Browser audio | Web Audio API — AudioWorklet |
| Reference app | HaloSound (MoveNet + Three.js + Web Audio) |

---

## Background

ChromaFlow is a tiny, RT-aware neural DSP and feature extraction library in C++17.  
It provides minimal differentiable building blocks (Dense, Conv, RNN, Attention), simple IIR-style online learning, and a real-time safe base class for audio processing.

The existing HaloSound voice detection pipeline uses pure JS DSP (formant analysis, F0 autocorrelation, spectral flatness) running at 60fps in < 3ms. This project explores whether ChromaFlow's adaptive ML layer can improve accuracy and personalization beyond what static DSP formulas can achieve.

---

## Status

🚧 Work in progress — POC 1 in progress.

---

## License

MIT — see [LICENSE](LICENSE)
