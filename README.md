# Seeing Eye to AI: Gaze-Based Response Rewards for LLMs

This repository contains the official implementation for the paper "Seeing Eye to AI: Human Alignment via Gaze-Based Response Rewards for Large Language Models", presented at ICLR.

## Overview

This project introduces a novel approach to aligning Large Language Models (LLMs) with human preferences by leveraging eye-tracking data. By analyzing human gaze patterns during the reading of model responses, we develop a reward mechanism that captures implicit human feedback, enabling more effective alignment than traditional methods. Our approach demonstrates significant improvements in generating responses that better match human attention patterns and preferences.
<p align="center">
  <img src="assets/pipeline.png" alt="Overview of the system" width="80%">
</p>

## Key Features

- Comprehensive eye-tracking data collection pipeline
- Novel gaze-based reward computation framework
- Fine-tuning methodology for LLMs using gaze-derived rewards
- Robust evaluation framework for measuring alignment improvements
- Support for multiple model architectures (Mistral, Llama, etc.)

## Installation

To install and run the project:

1. Clone the repository
2. Create a virtual environment with python 3.11.8 and activate it  
3. Install the required dependencies from requirements.txt
4. Install the tokenizer aligner package:

```bash
pip install git+https://github.com/angelalopezcardona/tokenizer_aligner.git@v1.0.0
```
5. Install the eyetrackpy package:
```bash
pip install git+https://github.com/angelalopezcardona/eyetrackpy.git@v1.0.0
```
6. Navigate to the rlhf_rw directory


## Usage

The main training scripts are located in the `rlhf_rw` directory. Below are example commands for training models with different configurations:

### Example 1: Training with HelpSteer2 dataset using Mistral-7B

```bash
python rlhf_rw/main.py \
  -d nvidia/HelpSteer2 \
  -m mistralai/Mistral-7B-v0.3 \
  --concat True \
  --use_softprompt True \
  --learning_rate 5e-5 \
  --logging_steps 64 \
  --lr_scheduler_type cosine_with_min_lr \
  --min_lr_ratio 0.7 \
  --batch_size 8 \
  --fp_dropout 0.1,0.3 \
  -fmv 1 \
  --seed 44
`` 
### Example 2: Training with OpenAssistant dataset using Llama-3-8B

```bash
python rlhf_rw/main.py \
  -d OpenAssistant/oasst1 \
  -m meta-llama/Meta-Llama-3-8B \
  --concat True \
  --use_softprompt True \
  --learning_rate 5e-5 \
  --logging_steps 64 \
  --lr_scheduler_type cosine_with_min_lr \
  --min_lr_ratio 0.7 \
  --batch_size 8 \
  --fp_dropout 0.1,0.3 \
  -fmv 2 \
  --features_used 0,1,0,1,0 \
  --seed 44
```

### Key Parameters

- `-d, --dataset`: Dataset to use for training. Currently, only nvidia/HelpSteer2 and OpenAssistant/oasst1 are supported.
- `-m, --model`: Base model to fine-tune. Currently, only mistralai/Mistral-7B-v0.3 and meta-llama/Meta-Llama-3-8B, meta-llama/Llama-3-8B-Instruct are supported.
- `--concat`: Whether to concatenate prompt and response. True is of GazeConcat and False is of GazeAdd.
- `--use_softprompt`: When using GazeConcat, whether to use add gaze features to the model. False if for baseline and True if for GazeConcat method.
- `--learning_rate`: Learning rate for optimization
- `--fp_dropout`: Feature projection dropout rates
- `--fmv`: Feature model version. 1 is for the first model and 2 is for the second model.
- `--features_used`: Binary flags for which gaze features to use (fixation count, duration, etc.)

## Citation

If you find this work useful for your research, please cite our paper:

```bibtex
@inproceedings{Lopez-Cardona2025Seeing,
  title     = {Seeing Eye to AI: Human Alignment via Gaze-Based Response Rewards for Large Language Models},
  author    = {Lopez-Cardona, Angela and Segura, Carlos and Karatzoglou, Alexandros and Abadal, Sergi and Arapakis, Ioannis},
  booktitle = {International Conference on Learning Representations (ICLR)},
  year      = {2025},
  url       = {https://openreview.net/forum?id=uZgK0tcPqd}
}
```
## License

© 2025 Telefónica Innovación Digital.  
Released under the GNU Lesser General Public License v3.0 (LGPLv3).  
See [LICENSE](./LICENSE) for details.

