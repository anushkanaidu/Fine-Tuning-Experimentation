# Dataset notes

I made these job postings for my fine-tuning assignment. They are synthetic examples, not scraped real jobs.

- `train.jsonl`: 40 examples for training.
- `test.jsonl`: 10 separate examples for evaluation.

For every posting, I labeled the title, required skills, years of experience, remote status, and minimum and maximum annual salary.

- I included only **required** technical skills, not preferred ones.
- For experience, I used `0` when the posting explicitly said no experience was needed, and `null` when it didn't say.
- For remote status, I used `true` only for fully remote jobs; hybrid and onsite were `false`. If it didn't say, I used `null`.
- I converted annual salaries into USD numbers, such as $120K to `120000`. If a single salary was given, I used it for both minimum and maximum.
- I used `null` when a detail wasn't provided.

I corrected two test labels during the notebook review: posting 41's required statistical modeling skill and posting 49's full job title. The corrections appear **before both models were evaluated** in the Colab notebook. The `test.jsonl` here uses those final labels.
