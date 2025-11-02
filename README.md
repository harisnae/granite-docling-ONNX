# Granite‑Docling ONNX Demo  

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/harisnae/granite-docling-ONNX/blob/main/granite-docling-ONNX.ipynb)

## Overview  

This repository contains a **Jupyter notebook** (`granite-docling-ONNX.ipynb`) that demonstrates how to run the **ONNX‑converted** version of IBM’s Granite‑Docling model for image‑conditioned captioning.

- **ONNX model files** (vision encoder, token embedder, decoder) are hosted at:  
   https://huggingface.co/onnx-community/granite-docling-258M-ONNX  

- The **original PyTorch model** (used by the official demo) is available at:  
   https://huggingface.co/ibm-granite/granite-docling-258M  

- The **official Hugging Face Space demos** (working captions) are:  
  - https://huggingface.co/spaces/ibm-granite/granite-docling-258m-demo  
  - https://huggingface.co/spaces/ibm-granite/granite-docling-258M-WebGPU  

The notebook in this repo **does not yet produce the correct caption** for the sample image used in the official Hugging Face Space demo. The purpose of publishing this repository is to **share the current implementation with the ONNX community** and ask for help in identifying why the generated caption differs from the reference output.

---

## What the Notebook Does  

1. Installs the required packages (`optimum[onnxruntime]`, `transformers`, `torch`, `onnxruntime`, etc.).  
2. Downloads the three ONNX model files (`vision_encoder.onnx`, `embed_tokens.onnx`, `decoder_model_merged.onnx`) together with their `.onnx_data` weight files.  
3. Builds three separate ONNX Runtime `InferenceSession`s (vision encoder, token embedder, decoder).  
4. Provides several caption‑generation functions (greedy, top‑k / top‑p sampling, no‑cache, BOS/EOS‑corrected).  
5. Runs the functions on a sample image (Lake Zurich) and prints the generated token IDs and decoded captions.  
---

## How to Run the Notebook  

1. Click the **Open in Colab** button above (or open the notebook in a local Jupyter environment).  
2. Execute the cells **in order**.  
3. When prompted, optionally provide a Hugging Face access token (only needed for private repositories).  
4. The notebook will download the ONNX files, create the sessions, and attempt caption generation.  

**Current behavior:** The caption generated for the sample image does **not** match the one shown in the official Hugging Face Space demo (`A view of Lake Zurich in Switzerland with mountains in the background.`).  

---

## How You Can Help  

- Identify any missing or incorrect preprocessing steps (e.g., token IDs, image‑token placeholder, BOS/EOS handling).  
- Suggest modifications to the input construction for the decoder so that it mirrors the exact input pipeline used by the original Space demo.  
- Provide a minimal patch or a revised notebook that yields the correct caption.  

Any feedback, pull request, or issue discussion is welcome!

---

## Future Plans  

- **Browser‑only inference** – integrate the model with **@huggingface/transformers** (Web‑GPU / Web‑CPU) so the entire pipeline runs client‑side in the browser.  
- **One‑time load & IndexedDB caching** – the model files will be downloaded the first time a user opens the page, then stored in **IndexedDB** for instant reuse, enabling true **offline‑first** usage.  
- **No server or API keys** – all inference happens locally; no data ever leaves the user’s device and no secret keys are required.  
- **Static‑site friendly** – the demo can be built as a pure static site (HTML + JS + CSS) and hosted on any static host such as **GitHub Pages**, **Netlify**, or **Cloudflare Pages**.  
- **Responsive, mobile‑first design** – UI styled with mobile‑first CSS so the demo works smoothly on screens as small as **320 px**, providing a consistent experience across desktop and mobile devices.
---

**License**  

The code in this repository is released under the **MIT License**.  
The underlying model weights are subject to the licenses of the original IBM Granite‑Docling model and the ONNX conversion provided by the Hugging  Face community.  
