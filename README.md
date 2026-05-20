<h1 align="center">
  <img width="96" align="absmiddle" alt="SPIKE logo" src="assets/logo.png">&nbsp;<strong><big><big>SPIKE</big></big></strong>&emsp;&emsp;&emsp;
</h1>

<h2 align="center">
  An Adaptive Dual Controller Framework for Cost-Efficient Long-Horizon Game Agents
</h2>

<p align="center">
  <a href="https://openreview.net/profile?id=~Wencan_Jiang1"><strong>Wencan Jiang <sup>1</sup></strong></a>
  &middot;
  <a href="https://zhangzjn.github.io/"><strong>Jiangning Zhang <sup>1,✉</sup></strong></a>
  &middot;
  <a href="https://jianbiaomei.github.io/"><strong>Jianbiao Mei <sup>1</sup></strong></a>
  &middot;
  <a href="https://github.com/Eddie0521"><strong>Jinzhuo Liu <sup>1</sup></strong></a>
  &middot;
  <a href="https://yuyang-cloud.github.io/"><strong>Yu Yang <sup>1</sup></strong></a>
  <br>
  <a href="https://scholar.google.com/citations?user=3lMuodUAAAAJ"><strong>Xiaobin Hu <sup>2</sup></strong></a>
  &middot;
  <a href="https://scholar.google.com/citations?hl=zh-CN&user=m3KDreEAAAAJ"><strong>Zhucun Xue <sup>1</sup></strong></a>
  &middot;
  <a href="https://person.zju.edu.cn/en/yongliu"><strong>Yong Liu <sup>1</sup></strong></a>
  &middot;
  <a href="https://dr.ntu.edu.sg/cris/rp/rp02343"><strong>Dacheng Tao <sup>3</sup></strong></a>
</p>

<p align="center">
  <strong><sup>1</sup>Zhejiang University</strong> &nbsp;&nbsp;&nbsp;
  <strong><sup>2</sup>National University of Singapore</strong> &nbsp;&nbsp;&nbsp;
  <strong><sup>3</sup>Nanyang Technological University</strong>
</p>

<p align="center">
  <a href="https://arxiv.org/abs/2605.18636">
    <img src="https://img.shields.io/badge/arXiv-2605.18636-red?style=flat&logo=arXiv&logoColor=red" alt="arXiv">
  </a>
  <a href="https://wencanjiang.github.io/projects/SPIKE/">
    <img src="https://img.shields.io/badge/Website-Online-green?style=flat&logo=googlechrome&logoColor=green" alt="Website">
  </a>
  <a href="https://arxiv.org/pdf/2605.18636">
    <img src="https://img.shields.io/badge/Paper-PDF-blue?style=flat" alt="Paper">
  </a>
</p>

<a name="introduction"></a>

# 📷 Introduction

SPIKE is an adaptive dual-controller framework for long-horizon game agents in Stardew Valley. It combines strategic replanning, fast reactive execution, and controller-specific memory to improve task success while reducing unnecessary LLM calls.

<a name="highlight"></a>

# ✨ Highlight!!!

<img src="assets/teaser.png" width="760">

SPIKE is designed for long-horizon multimodal agents that must remain goal-directed over many low-level interactions under token and latency constraints.

1. **Event-triggered amortized deliberation:** Strategic reasoning is reused across stable local segments and reinvoked at visual, progress, repetition, or failure boundaries.
2. **Adaptive dual-controller execution:** A Strategic Controller handles planning and recovery, while a bounded Reactive Controller performs fast local execution and local override.
3. **Controller-specific hierarchical memory:** SPIKE separates State-Action Memory Bank retrieval for routine execution from State-Action Knowledge Graph evidence for replanning.
4. **Better success-cost trade-off:** On StarDojo Lite-100, SPIKE improves success over the strongest baseline while reducing tokens and latency.

<a name="contents"></a>

# 🧭 Summary of Contents

- [📷 Introduction](#introduction)
- [✨ Highlight!!!](#highlight)
- [🧭 Summary of Contents](#contents)
- [🧠 Method Overview](#method-overview)
- [🛠️ Installation](#installation)
    - [1. Prepare the game environment](#1-prepare-the-game-environment)
    - [2. Install the Python runtime](#2-install-the-python-runtime)
    - [3. Configure local environment](#3-configure-local-environment)
- [⚙️ Configuration](#configuration)
- [📦 Resources](#resources)
- [🧩 Components](#components)
- [🚀 Usage](#usage)
    - [Useful scripts](#useful-scripts)
- [✅ Verification](#verification)
- [📚 Citation](#citation)
- [📬 Contact](#contact)

<a name="method-overview"></a>

# 🧠 Method Overview

<img src="assets/architecture.png" width="760">

**SPIKE framework.** SPIKE uses event-triggered switching to reserve strategic deliberation for discontinuities while reactive execution handles stable local progress.

<a name="installation"></a>

# 🛠️ Installation

### 1. Prepare the game environment

For Stardew Valley, SMAPI, StarDojoMod, and other game-side setup details, follow the official [StarDojo](https://github.com/StarDojo2025/stardojo) instructions.

### 2. Install the Python runtime

```powershell
git clone https://github.com/Rinke02/SPIKE.git
cd SPIKE
conda create -n Spike python=3.10.9
conda activate Spike
python -m pip install -r requirements.txt
python -m pip install -e ./agent
```

### 3. Configure local environment

Create your local environment file:

```powershell
Copy-Item env/.env.example env/.env
```

Fill in `env/.env` with your local `STARDEW_APP_PATH` and the API key variables referenced by the config you use. Do not commit `env/.env`; it is ignored by Git.

Set project paths for the current Windows PowerShell session:

```powershell
.\setup.ps1
```

<a name="configuration"></a>

# ⚙️ Configuration

This source release includes public templates for Qwen, OpenAI-compatible, and Gemini runs:

```text
agent/conf/qwen_config.json
agent/conf/openai_config.json
agent/conf/gemini_config.json
agent/conf/env_config_stardew.json
```

The public config files keep model names and embedding settings, but do not contain private keys or service URLs. Fill in `base_url`, `emb_base_url`, `api_key`, `emb_api_key`, or the corresponding environment variables for your own provider.

<a name="resources"></a>

# 📦 Resources

The required Stardew task save folders are included under:

```text
env/tasks/saves/save_new
env/tasks/saves/save_farming
env/tasks/saves/save_quests
```

The default text embedding model is [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5). When this model is used through `sentence-transformers`, it can be downloaded automatically on first use or prepared in your local Hugging Face cache in advance. Model files and local cache directories are not included in this repository.

The default visual embedding is local-image-embedding-v1, a deterministic local image feature extractor implemented in agent/cradle/runner/image_embedder.py.

<a name="components"></a>

# 🧩 Components

- `agent/`: core SPIKE agent package, including controller logic, memory modules, Stardew integration, resources, and model configuration templates.
- `StardojoMod/`: public SMAPI mod source used by the StarDojo-based Stardew Valley runtime.
- `env/`: Stardew runner code, task suites, save folders, game data, and environment utilities.
- `run_lite100_parallel.py`: standard Lite-100 parallel benchmark entry point.
- `summarize_run_results.py`: helper script for summarizing run outputs.
- `tests/`: minimal checks for the Lite-100 runner and local embedding defaults.

<a name="usage"></a>

# 🚀 Usage

Run from the repository root with `Spike` activated.

```powershell
python run_lite100_parallel.py --dry_run --llm_config agent/conf/qwen_config.json --embed_config agent/conf/qwen_config.json
```

Replace the config paths with the provider templates under `agent/conf/` as needed. Remove `--dry_run` after completing the game-side setup in [StarDojo](https://github.com/StarDojo2025/stardojo), preparing `env/.env`, and filling in the required model provider settings.

### Useful scripts

- `run_lite100_parallel.py`: standard Lite-100 parallel benchmark.
- `summarize_run_results.py`: summarize benchmark outputs.
- `verify_qwen_no_key.py`: check Qwen config behavior without publishing keys.

Runtime output is written under `runs/`, which is ignored by Git.

<a name="verification"></a>

# ✅ Verification

You can also check the Lite-100 command generation without launching Stardew Valley:

```powershell
python run_lite100_parallel.py --dry_run --llm_config agent/conf/qwen_config.json --embed_config agent/conf/qwen_config.json
```

For an actual game-side smoke test, use a configured `env/.env` and your own model provider settings, then run a small Lite-100 subset or the full standard Lite-100 entry point.

<a name="citation"></a>

# 📚 Citation

If you find our work useful, please consider citing us!

```bibtex
@misc{jiang2026spike,
  title={SPIKE: An Adaptive Dual Controller Framework for Cost-Efficient Long-Horizon Game Agents},
  author={Wencan Jiang and Jiangning Zhang and Jianbiao Mei and Jinzhuo Liu and Yu Yang and Xiaobin Hu and Zhucun Xue and Yong Liu and Dacheng Tao},
  year={2026},
  eprint={2605.18636},
  archivePrefix={arXiv},
  primaryClass={cs.CV},
  url={https://arxiv.org/abs/2605.18636}
}
```

<a name="contact"></a>

# 📬 Contact

For issues or contributions, feel free to open an issue or pull request.
