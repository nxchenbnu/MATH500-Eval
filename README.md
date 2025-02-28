# MATH500 评测

## 依赖说明
运行以下代码，下载并安装lm-evaluation-harness：
```
git clone --depth 1 https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
pip install -e .
```
其他环境，参考 requirements.txt

## 评测方法
在原有框架下，将MATH500加入新任务

参考：https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/new_task_guide.md

1. lm-evaluation-harness -> lm_eval -> tasks 文件夹下，新建math500文件夹，并加入 math500.yaml 文件

2. 运行以下测试代码

```
export OPENAI_API_KEY=#YOUR_API_KEY
# 生成阶段，生成模型所有的回复
lm-eval --model openai-chat-completions \
        --model_args model=gpt-4o-mini,apply_chat_template=True,max_tokens=4096,until=null \
        --tasks math500 \
        --apply_chat_template \
        --predict_only \
        --output_path ./predictions_0228/output.json \
        --log_samples \
        --use_cache ./predictions_0228/gpt-4o-mini
# 评测阶段，评测模型回复准确率
lm-eval --model openai-chat-completions \
        --model_args model=gpt-4o-mini,apply_chat_template=True,max_tokens=2048,until=null \
        --tasks math500 \
        --apply_chat_template \
        --output_path ./results \
        --log_samples
```

## 运行示例
以测试gpt-4o-mini为示例如下：
```
2025-02-28:10:41:48,857 INFO     [lm_eval.evaluator:169] Setting random seed to 0 | Setting numpy seed to 1234 | Setting torch manual seed to 1234 | Setting fewshot manual seed to 1234
2025-02-28:10:41:48,857 INFO     [lm_eval.evaluator:206] Initializing openai-chat-completions model, with arguments: {'model': 'gpt-4o-mini', 'apply_chat_template': True, 'max_tokens': 4096, 'until': 'null'}
2025-02-28:10:41:48,857 WARNING  [lm_eval.models.openai_completions:116] chat-completions endpoint requires the `--apply_chat_template` flag.
2025-02-28:10:41:48,857 INFO     [lm_eval.models.api_models:116] Using max length 4096 - 1
2025-02-28:10:41:48,857 INFO     [lm_eval.models.api_models:119] Concurrent requests are disabled. To enable concurrent requests, set `num_concurrent` > 1.
2025-02-28:10:41:48,857 INFO     [lm_eval.models.api_models:134] Using tokenizer None
2025-02-28:10:41:48,857 INFO     [lm_eval.evaluator:226] Using cache at ./predictions_0228/gpt-4o-mini_rank0.db
2025-02-28:10:41:53,679 WARNING  [lm_eval.api.task:327] [Task: math500] has_training_docs and has_validation_docs are False, using test_docs as fewshot_docs but this is not recommended.
2025-02-28:10:41:53,679 WARNING  [lm_eval.api.task:327] [Task: math500] has_training_docs and has_validation_docs are False, using test_docs as fewshot_docs but this is not recommended.
2025-02-28:10:41:53,694 INFO     [lm_eval.evaluator:261] Processing math500 in output-only mode. Metrics will not be calculated!
2025-02-28:10:41:53,694 WARNING  [lm_eval.evaluator:421] Chat template formatting change affects loglikelihood and multiple-choice tasks. See docs/chat-template-readme.md for details.
2025-02-28:10:41:53,694 INFO     [lm_eval.api.task:420] Building contexts for math500 on rank 0...
100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 500/500 [00:00<00:00, 7562.46it/s]
2025-02-28:10:41:53,770 INFO     [lm_eval.evaluator:517] Running generate_until requests
2025-02-28:10:41:53,771 INFO     [lm_eval.api.model:261] Loading 'generate_until' responses from cache './predictions_0228/gpt-4o-mini_rank0.db' where possible...
Checking cached requests: 100%|█████████████████████████████████████████████████████████████████████████████████████████████████████| 500/500 [00:00<00:00, 21830.10it/s]
2025-02-28:10:41:53,794 INFO     [lm_eval.api.model:285] Cached requests: 0, Requests remaining: 500
Requesting API:   0%|                                                                                                                            | 0/500 [00:00<?, ?it/s]2025-02-28:10:41:53,795 INFO     [lm_eval.models.api_models:620] Tokenized requests are disabled. Context + generation length is not checked.

####################################################
#################### 省略API访问 ####################
####################################################

Requesting API: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 156/156 [14:35<00:00,  5.61s/it]

#################### 模型评测结果 ####################

openai-chat-completions (model=gpt-4o-mini,apply_chat_template=True,max_tokens=4096,until=null), gen_kwargs: (None), limit: None, num_fewshot: None, batch_size: 1
| Tasks |Version|  Filter  |n-shot|  Metric   |   |Value|   |Stderr|
|-------|-------|----------|-----:|-----------|---|----:|---|-----:|
|math500|Yaml   |get-answer|     0|exact_match|↑  |0.642|±  |0.0215|
```
