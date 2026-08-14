# Introduction
The goal of this document is to provide a basic background for using local large language models (LLMs). This will introduce terminology, tools, and techniques to aid you in exploring and utilizing LLMs.

# Model Principles
People commonly experience LLMs through *frontier models*, via web-based chatbots. One of the simplest indicators of model "knowledge" is their parameter count: more parameters means more representation power. Frontier models may have parameter counts in the trillions, requiring them to run on massive distributed computational clusters.

Local models are, instead, running on your local computer. This means you get smaller models (parameter counts in the billions), and to get the most from your local model, you will probably need some awareness of their surrounding infrastructure which is otherwise hidden by frontier models.

## Why use a local model?
If using a local model necessarily implies a less-powerful model, why would we willingly make that choice? First and foremost is the consideration of *accessibility*: local models using open weights are as transparent as you can get in the LLM field. You are not bound by user agreements, you will not run out of tokens midway through the month, and you pay for usage with wear and tear on your local machine instead of subscription models.

There is also a matter of *reproducibility*. It's reasonable to expect that models will grow more powerful with time, but if you are attempting to generate scientific results directly with an LLM, a local model that you control will be your best chance at guaranteeing availability in the future.

Local model use also presents opportunities for configuration and tuning. There is a belief that LLM use will move towards smaller, targeted models, as use-cases are often focused in scope. Using local models will naturally increase your understanding and confidence in LLM concepts, which will support you in building useful skills for the future.

## Role of memory
Fundamentally, an LLM is a program running in your computer's memory: it is a statisical construct that takes input (text, images, audio), broken down into "tokens", and reports back what is the most likely output given that particular token series.

For local LLMs, **it is essential to understand that not all memory is created equal**. Video memory (VRAM, from the GPU) is dramatically faster than your standard computer RAM (used by the CPU). LLMs are almost universally optimized to run on VRAM, and a general goal for local LLM use is to fit as much as possible (ideally, all) of our model into our system's available VRAM.

## Open Weight Formats
When we download a model, we are downloading a binary representation of the weights (and occasionally additional supplementary structures). There are currently two common weight formats you may encounter:

* **GGUF** - **G**PT-**G**enerated **U**nified **F**ormat is the most commonly encountered open weight format and is designed to run on consumer hardware.
* **MLX** - A weight format designed specifically for Apple Silicon macs (M1, M2, etc...). This takes advantage of the Unified Memory Architecture (UMA) which combines CPU and GPU RAM.

## Weight Quantization
Each weight in a model is represented by a number; in addition to the count of weights, the numerical format of the weights significantly influences the memory requirements of that model. By default, weights are typically stored in 16-bit floating point numbers. Using smaller bit counts per weight is called *quantization*, and is a form of compression so that larger models can be adapted to available hardware.

While quantization does produce "dumber" models, the smaller weights actually improve performance, as a smaller total volume of data must be processed during inference. Local model use almost universally implies quantization, so it is helpful to be familiar with the implications of various quantization algorithms and nomenclature:
- `Q` is basic quantization applied equally to all weights. `Q8` means weights are stored with 8 bits of precision (half the size), while `Q4` uses 4 bits
- `K` has largely succeeded `Q` quantization, grouping weights into blocks and providing higher precision to blocks that are critical to output generation.
- `IQ` is a newer experimental quantization aimed at extremely small bitrates. It uses a corresponding importance matrix to identify which weights impact output quality the most. 
- `UD`, or "ultra dynamic," is a technique unique to [Unsloth](https://unsloth.ai/) models which dynamically quantizes each layer of a model.
- `QAT`, or quantization-aware training, uses simulated quantizations during the training itself instead of compressing weights after training. This mitigates many errors of quantized models, as they have "learned" associations using quantized weights.

## Context
Context is the heart of LLM performance, often directly determining the usability of a model's output. Essentially, the context is a collection of tokens related to a query. At its most basic, this includes the conversation history itself; but in agentic LLM use, this can include portions of source code, documentation, web searches, and so forth.

The maximum context size is model-dependent. With local models you will almost never approach this maximum because, after the model weights, the context has the next largest impact on LLM memory footprint. It is proportionate to the parameter count, but the context cache defaults to using 16-bit floating point numbers, regardless of weight quantization, which can quickly escalate to demanding 10's of GB of memory. In any case, a large context can degrade performance as it fills.

While models do not have a strict minimum context size, if the context is too small, key system instructions may be lost. This exact value will depend on the harness you're using (as different toolsets will have different footprints). If you notice odd failures, such as random crashes or the model not returning, try increasing your context size.

A reasonable starting point is to aim for a 16-32k token context. Finding the "perfect" context size can be challenging; LM Studio can be helpful here to find the largest context that will fit in VRAM, and just leave it at that.

### Context Quantization
The context is a type of key:value cache - it is used constantly during inference so, just like the model weights, we want to try and fit this cache fully in VRAM.

Because of the 16-bit key and value format, using cache quantization can facilitate a dramatic drop in cache size. This doesn't necessarily lead to performance benefits directly, rather larger caches than we may be able to load otherwise. However, it may also cause massive points of failure and strange behavior as the cache starts missing lookups, or interpreting data oddly.

As a first pass selecting an appropriate un-quantized context for your hardware is preferred. If that proves problematic, try quantizing cache values more aggressively than keys. Experiment with Q8 and see how the model does - if it seems confused, you may need to reduce your context size or move to a smaller model.

## Mixture of Experts (MoE)
One trick to improve small model performance is a "mixture of experts": dividing the parameters into sub-networks (the "experts") and limiting how many experts are active for any given token. You can identify these models by an `A#B` notation conveying how many parameters are active. For example, a `26B A4B` model has 26 billion parameters, of which 4 billion are active for any given token.

MoE models have an extra routing step which determines where to send each piece of input, thus these models can behave oddly when heavily quantized as experts begin to collide. Instead, overall performance may be better with some experts offloaded to CPU.

## Multi-token prediction (MTP)
Another architecture for improving model performance is MTP. Traditionally, tokens are decoded one at a time in sequence. With MTP, multiple tokens are attempted to decode simultaneously. This adds a validation step where the quality of the decoding is estimated, and may result in token rejection. For example, if you try decoding 2 tokens at a time but only accept 50% of them, you have not actually changed performance (and probably made things worse by adding the validation check). But a 75% acceptance rate may represent a significant speedup.

In general, if you find your local LLM to be too slow, you might experiment with MTP settings. Start with a small number of simultaneous predictions, and monitor your acceptance rate closely. This may be ideal for simple, repetitive tasks (coding boilerplate), but may be avoided for architectural or natural language tasks.

## Speculative decoding
Speculative decoding works very similarly to MTP, but is not an integrated part of a given model's training. Instead, with speculative decoding, a small secondary "draft model" is loaded concurrently. The draft model proposes tokens for the main model to accept or reject.

## Sampling parameters
While model weights are fixed during the training process, there are several parameters that can be considered during model runtime. Whether or not these parameters are actually exposed depends on how the model is loaded and the [harness](#harnesses) used.

- **Temperature**: General probability distribution. Lower values are more deterministic; higher values are more creative/random.
- **Min-P**: Token probability threshold, relative to the most likely token. This minimizes outlier selection when the model is confident, but encourages exploration
- **Top-P**: Cumulative probability threshold (`0.0` to `1.0`) when predicting the next token. Lower values narrow choice to most likely tokens; higher values allow for broader selection.
- **Top-K**: Hard cap limiting considerations for the next token to the `K` most probable options. Cuts off low-probability tail tokens.
- **Frequency/presence penalty**: Penalize tokens for their presence in output so far. Positive values can reduce repetitive phrases.

# Model Management
To start using an LLM locally, it is not enough to simply download a `GGUF` file and go: we need a software layer on top to manage loading the model weights into memory, to pass tokens for inference, and to relay resulting output. This is almost unversally `llama.cpp`—a local model management core—but several tools are available that abstract `llama.cpp` to varying degrees, and provide additional light functionality on top (such as chat). However, these applications should be viewed first and foremost as *model managers*: making models available for use elsewhere.

## LM Studio
[LM Studio](https://lmstudio.ai/) is one of the top when it comes to exploring and fine-tuning model parameters. This means it also carries a high level of complexity. There are several key areas of the GUI:

- Model Search allows you to find and install local models from a variety of sources, including curated LM staff picks, and [HuggingFace](https://huggingface.co/) - one of the best sources for open models.
- The Settings ⚙️ gear contains some key considerations, in particular the Runtime section allows you to update your `llama.cpp` versions, while Hardware will show your available and used system resources.
- Selecting a model to load will bring up a dialog of locally installed models. A manual parameter toggle allows you to fine tune a large number of parameters, such as context length, and see an estimate of how much memory will be used.
- The Chat window is where you can have local conversations with your active models, as well as configure runtime parameters like reasoning and temperature. Through integrations and extensions, this chat can be customized to meet many use cases - but its greatest value is in experimenting with model configurations for use in purpose-built harnesses.
- Finally, a Developer panel allows you to expose your active models to other services via a local REST API.

## Ollama
[Ollama](https://ollama.com/) is a "friendly," lightweight entry point open models. While a GUI app is provided, it is *very* limited to simple chats. Key functionality will, instead, be found on the web or command-line utility:

- First, find a model of interest [via the model search](https://ollama.com/search). The presentation of models here is limited, which makes searching fairly straightforward.
- For more options, and complexity, use [HuggingFace](https://huggingface.co/) to look for models. Any HF model can be referred to in Ollama via the `hf.co/[model_maker]/[model_name]:[version_tag]` syntax - for example, `hf.co/google/gemma-4-12B-it-qat-q4_0-gguf:latest`
- Once you find a model of interest, use `ollama pull <model_ref>` to download it.
- `ollama run <model_ref>` will start a local server with a specified model, going right into a chat interface.
- Alternatively, `ollama launch <service>`  can be used to connect a (more useful) dedicated harness to a local Ollama model.
- `ollama ps` is a helpful tool to check the actual size and processor (RAM/GPU) division of running model(s).

Note that for model parameter configuration, one must turn to the [Ollama Modelfile format](https://docs.ollama.com/modelfile). This format feels surprisingly limited given its complexity. For exploring parameter tuning, LM Studio is preferred.

Additionally, GUI updates may lag behind. Use the command-line instructions for updates (`irm https://ollama.com/install.ps1 | iex` on Windows, or `curl -fsSL https://ollama.com/install.sh | sh` on Mac/Linux) to keep your underlying `llama.cpp` up-to-date.

# Harnesses
A **harness** is a tool for interacting with an LLM: this is your interface for connecting to the model. The harness you use will define *how* you interact. For example, in web-based LLM use, [https://gemini.google.com/](https://gemini.google.com/) is a simple harness that allows you to chat with a few Gemini models, [https://chatgpt.com/](https://chatgpt.com/) is the equivalent for OpenAI models, and [https://claude.ai/](https://claude.ai/) the same for Anthropic/Claude.

At their core, what differentiates harness from each other are their available **tools**, exposure of **sampling parameters**, and **user experience**. If you want to use a LLM for coding, you want a harness that can easily navigate source code, with tools for reading and editing source files.

## Visual Studio Code
[Visual Studio Code](https://code.visualstudio.com/) (VS Code) is an exceptional integrated development environment (IDE) for coding with LLM support. It has been designed with LLM use at the forefront, and many commercial products (i.e. Cursor) are derived from it. As a Microsoft product, it also has first-party integration with [GitHub Copilot](https://github.com/copilot), making it a natural choice for those with academic/student access.

### GitHub Copilot Chat
This is the default AI chat that comes pre-installed with VS Code. While it is aimed at linking to your GitHub account and corresponding Copilot subscription, you can also connect local models which will be able to use the full complement of editing tools.

As of v 1.133.0, clicking on the currently active model in chat brings up a selection dialog; the gear ⚙️ next to `Other models` allows you to add models, which was recently expanded via [model providers](https://code.visualstudio.com/api/extension-guides/ai/language-model-chat-provider), a new class of IDE plugin. [Ollama](#ollama) integration has first-party support out of the box. Unfortunately, most other first-party offerings are aimed at a "bring your own key" (BYOK) model, assuming you have a paid account with a remote LLM provider. Use of [LM Studio](#lm-studio) models require setting a custom endpoint, or using a community model provider.

### Continue.dev
[Continue](https://docs.continue.dev/) is an open source AI harness plugin for VS Code. Installing the plugin will add a new `Continue` chat option. You can use this as a direct alternative to GitHub Copilot Chat. The functionality is very similar, although you may find a need to update your keybindings, as so much comes pre-configured for Copilot. The main benefit of using `Continue` is that model integration is very straightforward and permissive, even from [LM Studio](#lm-studio). Additionally, more customization is exposed - such as tuning separate models for chat, autocomplete, and code editing.

# Getting the most from your model
If you read this entire article, it is likely clear that there is a very high ceiling for learning about LLMs and using local models effectively. You certainly do not need master or even understand every parameter. The hope is that you now feel some confidence in where to start.

The following is a quick reference of considerations when using local LLMs:

1. VRAM is your top constraint. Ideally, all your model components (layers, KV cache) will be on GPU.
1. Prioritize model size over precision. A heavily quantized large model (e.g., `16B` at `Q4`) will almost always outperform a smaller model with less quantization (e.g., `8B` at `Q8`).
1. Keep total VRAM allocation around 90-93% of capacity to avoid crash risks.
1. If you see odd model behavior, try increasing the context size.
1. Use low Temperature (`0.0` - `0.2`) for data extraction, bio-informatic scripts, and structured JSON outputs to maximize determinism.

## Hardware considerations
- Buy computers with bigger GPUs than you think you'd need. Look for GPUs tuned for running LLMs, and strongly consider Apple Silicon chips for their UMA.
- On Mac, MLX models may have better performance, but also have limited configuration options.

## LM Studio notes
- These settings may need trial-and-error to assess impact: **Physical batch**, **Eval batch**, **Concurrent predictions**
- Keep these settings as-is: **Unified KV Cache**, **RoPE frequency**, **Keep model in memory**, **try mmap()**, **Flash Attention**