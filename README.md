# Llama2 Guide

This repository contains information related to understanding and running the Llama2 large language model.

## Table of Contents

- [Running Llama2 on Apple Silicon](#running-llama2-on-apple-silicon)
- [Additional Resources](#additional-resources)

---

## Running Llama2 on Apple Silicon

To run Llama2 on Apple's M1 Pro (or similar architectures) using [llama.cpp](https://github.com/ggerganov/llama.cpp), follow these steps:

1. **Setup Directories and Clone Repositories:**

```bash
mkdir llama-on-apple
git clone https://github.com/facebookresearch/llama
git clone https://github.com/ggerganov/llama.cpp
```

2. **Get official llama2 weights from Meta**

```bash
mkdir meta_models
place downloaded files in this directory
```

```
llama-on-apple/
├── llama/
├── llama.cpp/
└── meta_models/
    ├── llama-2-7b/
    └── llama-2-7b-chat/

```

3. **Build Llama.cpp:**

```bash
cd llama.cpp
LLAMA_METAL=1 make
```

4. **Create a Conda Environment and Install Dependencies:**

```bash
conda create -n llama2 python=3.11
conda activate llama2
python3 -m pip install -r requirements.txt
```

5. **Convert Model from Meta Format to Llama.cpp Format:**

```bash
python3 convert.py --outfile models/7B/ggml-model-f16.bin --outtype f16 ../../llama-on-apple/meta_models/llama-2-7b-chat
```

6. **Quantize Model to Reduce Size:**

```bash
./quantize ./models/7B/ggml-model-f16.bin ./models/7B/ggml-model-q4_0.bin q4_0
```

This step quantizes the model, reducing its size from 13GB to 3GB.

7. **Chat with the Model:**

```bash
./main -m ./models/7B/ggml-model-q4_0.bin -n 1024 -ngl 1 --repeat_penalty 1.0 --color -i -r "User:" -f ./prompts/chat-with-bob.txt
```

Steps taken from video on [Youtube](https://www.youtube.com/watch?v=TsVZJbnnaSs)

---

## Additional Resources

---

For any issues or further questions, please raise an issue in the repository.
