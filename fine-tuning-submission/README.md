# Fine-Tuning Qwen for Job Posting Extraction

**ARIN 5530 — Generative AI and Large Language Models | Fall 2026**

## About my project

I've been looking through job postings while applying for internships, and I noticed that every company lists skills, experience, salary, and remote work details differently. For this assignment, I wanted to see if I could fine-tune a small language model to pull out those details in the same format each time.

I used **Qwen2.5-0.5B-Instruct** with **LoRA**. The goal was to extract a job title, required technical skills, minimum years of experience, remote status, and minimum and maximum salary as JSON.

## What I did

1. I created 40 synthetic job postings with the expected JSON answers.
2. I kept 10 separate postings for testing.
3. I tested the original model using a detailed system prompt.
4. I fine-tuned the model with LoRA on a Colab T4 GPU for three epochs.
5. I tested the fine-tuned model on the same 10 postings.

The comparison is **the original model with the prompt** versus **the fine-tuned model with the same prompt**.

## Results

| Measure | Original model | Fine-tuned model |
| --- | ---: | ---: |
| Correct fields | 44/60 | 50/60 |
| Field-level accuracy after removing optional code fences | 73.3% | 83.3% |
| Strict raw JSON validity | 0/10 | 1/10 |
| JSON that could be parsed after removing code fences | 10/10 | 10/10 |

The fine-tuned model got six more fields correct, an improvement of 10 percentage points on this small test set. The biggest recorded improvement was remote status (4/10 to 9/10), although minimum salary got slightly worse (8/10 to 7/10).

**Why the notebook also shows 0% for the original model:** In the original notebook, the first evaluation tries to parse each response as JSON without changing it. All ten original responses included Markdown code fences, so that strict check failed. The **73.3% baseline** comes from evaluating those same saved responses *after removing the code fences*. The predictions themselves were not changed.

## What I learned

I was happy to see some improvement with only 40 examples, but fine-tuning didn't fix everything. The model often still added Markdown fences around its JSON, even though I asked it not to. I also ran into library compatibility problems in Colab and had to restart the runtime.

While reviewing the training code later, I noticed that the way I counted answer tokens for the training loss might have missed the start of some answers. That is a **possible limitation**, not something I have confirmed as the cause of the formatting issue. These results come from the original experiment; I haven't rerun it with a revised masking method.

Since I only tested on 10 synthetic examples, I wouldn't assume these results apply to real job postings. I would try better prompting and stricter JSON validation before deciding whether more fine-tuning was worth the time.

## Files

- `notebooks/Job_Posting_FineTuning.ipynb` — My actual Google Colab notebook, including the saved training and evaluation outputs.
- `data/train.jsonl` — 40 labeled training postings.
- `data/test.jsonl` — 10 test postings with the final corrected labels.
- `data/README.md` — How I labeled the postings.
- `results/base_model_predictions.jsonl` — Original-model responses used to recalculate the baseline.

To review the experiment, open the notebook in Google Colab. A T4 GPU is needed to rerun the training cells. The notebook includes my original dependency troubleshooting and runtime-restart steps, so **don't click Run all without reviewing those cells first**.

The individual fine-tuned predictions and LoRA adapter weights are not included in this upload. Their results are visible in the notebook outputs. If I add the saved prediction file or adapter ZIP later, I'll upload the actual files from Colab.

*AI-use note: I used AI assistance while organizing the project and improving some explanations. I ran the Colab experiment and reviewed the results myself.*
