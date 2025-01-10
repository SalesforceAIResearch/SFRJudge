# SFRJudge

This is the official repository for [Direct Judgement Preference Optimization](https://arxiv.org/abs/2409.14664). This repo currently contains code to run evaluation on 12 benchmarks featured in the paper. To run evaluation on RewardBench, please see the [RewardBench repo](https://github.com/allenai/reward-bench).

The contents of this repository are intended for research purposes only.

> Users need to make their own assessment regarding any obligations or responsibilities under the corresponding licenses or terms and conditions pertaining to the original datasets and data.


## Setup
This code was tested with Python 3.10.8 and PyTorch 2.3.0. 

```
conda create --name judge_eval python=3.10.8
conda activate judge_eval
pip install torch==2.3.0
pip install -r requirements.txt
```

## Usage
See `run_eval.sh` for examples. Alternatively, you can run

```
export HF_TOKEN=<your_hf_token>
python -u main_eval.py \
    --model [model_name (Huggingface or local)] \
    --num_gpus [num_gpus] \
    --eval_dataset [dataset name] \
    --output_path [output_path] \
    --temperature [sampling parameter temperature] \
    --top_p [sampling parameter top_p] \
```

Here, you can specify which datasets to run evaluation on, or specify `all` to run all datasets, or `all_pair`, `all_point`, or `all_class` to run all pairwise, pointwise (single rating), and classification datasets, respectively. Some evaluation datasets are be gated on HuggingFace and/or hosted on Google Drive. Please request access with the original dataset authors accordingly. 

After evaluation is finished, you can aggregate your results by running `aggregate_eval.py`
```
python aggregate_eval.py \
    --eval_path [output_path_from_above] \
    --type [all, pair, point, point_no_class] \ 
```

:warning: To get a pairwise average that includes RewardBench, this script expects you to run evaluation on RewardBench, then store results in `[output_path]/rewardbench/scores.json`. The json file should contain a key titled `leaderboard` with a corresponding dict that has the scores of each section and a key `overall_score` for the overall RewardBench score, a value between 0 and 1. Alternatively, you can recompute the average by taking the average produced by this script without RewardBench by (6*script_avg_no_rb + rb_score)/7.

```
"leaderboard": {
    "Chat": chat_score [0,1],
    "Chat Hard": chat_hard_score [0,1],
    "Safety": safety_score [0,1],
    "Reasoning": reasoning_score [0,1],
    "overall_score": overall_score [0,1],
}
```

The scores reported in our paper were obtained by running evaluation with 8xA100 40GB GPUs. 

## Ethical considerations
```
This release is for research purposes only in support of an academic paper. Our models, datasets, and code are not specifically designed or evaluated for all downstream purposes. We strongly recommend users evaluate and address potential concerns related to accuracy, safety, and fairness before deploying this model. We encourage users to consider the common limitations of AI, comply with applicable laws, and leverage best practices when selecting use cases, particularly for high-risk scenarios where errors or misuse could significantly impact people’s lives, rights, or safety. For further guidance on use cases, refer to our AUP and AI AUP. 
```

## Citation
```
@misc{wang2024directjudgementpreferenceoptimization,
      title={Direct Judgement Preference Optimization}, 
      author={Peifeng Wang and Austin Xu and Yilun Zhou and Caiming Xiong and Shafiq Joty},
      year={2024},
      eprint={2409.14664},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2409.14664}, 
}
```
