# Kaggle Protocol: Getting Started to Submission

_As of 2026-10-27_

## Purpose and scope

This protocol takes you from a new Kaggle account to a scored competition submission in six steps. Kaggle is Google's free data science platform: public datasets, cloud Jupyter notebooks with free GPUs/TPUs, competitions with leaderboards, and a model hub.

Follow the steps in order the first time. After that, use the checklist at the end as a quick reference.

## Step 1: Set up your account

Phone verification is required before you can use GPUs, TPUs or internet access in notebooks, so do it on day one.

1. Go to kaggle.com and click **Register**. Sign up with Google or an email address.
2. Choose a permanent username. It appears in every dataset and notebook URL you publish.
3. Open **Settings** (profile picture, top right) and verify your phone number.
4. Fill in your profile (bio, GitHub, LinkedIn). Public work on Kaggle doubles as a portfolio.
5. Optional: on the Learn tab, take the free short courses (Python, Pandas, Intro to Machine Learning) if you are new to data science.

## Step 2: Find and use datasets

The fastest path is to attach a dataset to a Kaggle notebook; download it only if you work locally.

1. Open the **Datasets** tab and search by keyword. Filter by file type (CSV, JSON, Parquet), size, license and usability score.
2. On a dataset page, read the **Data Card** (description, columns, source) and check the **License** before using it in published work.
3. To work in the cloud: click **New Notebook** on the dataset page. The files mount read-only at `/kaggle/input/<dataset-name>/`.
4. To work locally: click **Download**, or use the CLI (Step 5): `kaggle datasets download -d <owner>/<dataset-slug> --unzip`.
5. Review the dataset's **Code** tab. Other users' notebooks show how the data is usually loaded and cleaned.

Load a mounted file in a notebook:

```python
import pandas as pd
df = pd.read_csv("/kaggle/input/<dataset-name>/<file>.csv")
df.head()
```

## Step 3: Work in Kaggle Notebooks

Kaggle Notebooks are free cloud Jupyter environments; turn on an accelerator only when your code needs it, because GPU time comes from a weekly quota.

1. Go to **Code → New Notebook**, or start one from a dataset or competition page.
2. Open the right-hand **Settings** panel (Session options) and set:
    - **Accelerator**: None (CPU), a GPU option (T4 or P100 class), or TPU. The menu changes over time.
    - **Internet**: on to `pip install` packages; off is required for some competition submissions.
    - **Persistence**: keep files or variables between sessions if you need them.
3. Add data with **+ Add Input** (datasets, competitions, models, other notebooks' outputs).
4. Write files you want to keep to `/kaggle/working/`. That folder becomes the notebook's Output.
5. Click **Save Version → Save & Run All (Commit)** to run the whole notebook in the background. You can close the tab while it runs.
6. Watch your quota in the session panel and stop idle sessions to avoid wasting hours.

| Limit | Value |
| --- | --- |
| CPU and GPU session | 12 hours max |
| TPU session | 9 hours max |
| Weekly GPU quota | Floats; commonly about 30 hours, shown in your account |
| Bonus GPU hours | +15/week (Colab Pro) or +30/week (Colab Pro+) when accounts are linked |

Limits as reported by [GPUPerHour](https://gpuperhour.com/blog/free-cloud-gpus-and-credits) from Kaggle's notebook docs, September 2026. Check the live numbers in your own account.

## Step 4: Enter a competition and submit

Your score comes from a file matching `sample_submission.csv`, so match that format exactly before anything else.

1. **Pick a competition.** Beginners should start with a **Getting Started** or **Playground** competition (for example Titanic or House Prices). They have no deadline pressure and many public notebooks to learn from.
2. **Read the Overview.** Note the evaluation metric, the timeline (entry deadline, team-merger deadline, final deadline) and the prize rules.
3. **Join and accept the rules.** Click **Join Competition**. The API and data downloads fail until you accept.
4. **Study the Data tab.** You get `train` (with labels), `test` (without labels) and `sample_submission.csv`.
5. **Build a baseline.** Train a simple model first, then improve. Use cross-validation on the training set; do not tune only to the public leaderboard.
6. **Write the submission file** with the exact columns and row IDs of the sample.
7. **Submit.** Either upload the CSV under **Submit Prediction**, or for a **code competition** submit the notebook itself (often with internet off and a runtime cap).
8. **Check the daily submission limit** on the Overview page; it varies by competition.
9. **Select your final submissions** before the deadline (usually two). These are scored on the private leaderboard.

The **public leaderboard** scores a slice of the test set; the **private leaderboard**, revealed at the end, scores the rest and decides final ranks. A model overfit to the public board often drops ("shake-up").

Write a submission file:

```python
sub = pd.read_csv("/kaggle/input/<competition>/sample_submission.csv")
sub["target"] = model.predict(X_test)
sub.to_csv("/kaggle/working/submission.csv", index=False)
```

## Step 5: Set up the Kaggle API and CLI

The CLI lets you download data, submit and push notebooks from your own terminal; it needs Python 3.11+ (as of release 1.8.0) and an API token.

1. Install: `pip install kaggle`
2. Go to **Settings → API** on kaggle.com and click **Generate New Token**.
3. Store the token one of two ways:
    - Environment variable: `export KAGGLE_API_TOKEN=<your token>`
    - File: save the token to `~/.kaggle/access_token`
4. Older `kaggle.json` credentials (username + key) still work if you already have them. Lock the file down with `chmod 600`.
5. Never commit your token to Git. In Kaggle notebooks, use **Add-ons → Secrets** instead of pasting keys.

| Task | Command |
| --- | --- |
| List competitions | `kaggle competitions list` |
| Download competition data | `kaggle competitions download -c <competition>` |
| Submit predictions | `kaggle competitions submit -c <competition> -f submission.csv -m "message"` |
| See your submissions | `kaggle competitions submissions -c <competition>` |
| Search datasets | `kaggle datasets list -s <keyword>` |
| Download a dataset | `kaggle datasets download -d <owner>/<slug> --unzip` |
| Pull a notebook | `kaggle kernels pull <owner>/<notebook> -m` |
| Push a notebook | `kaggle kernels push -p <folder>` |

Auth steps from the official [Kaggle CLI repository](https://github.com/Kaggle/kaggle-cli/pull/863/files) (release 1.8.0).

## Step 6: Publish your own work

Everything you create starts private; switch it to public only after checking it holds no secrets or restricted data.

- **Notebooks**: save a version, then change sharing to **Public**. Add a clear title, a markdown intro and comments so others can follow it.
- **Datasets**: **Datasets → New Dataset**, upload files (or link a GitHub repo, URL or notebook output), then fill in the description, column notes, source and license. Complete metadata raises the usability score.
- **Models**: **Models → New Model** to publish trained weights with a model card, framework and license, so notebooks can attach them as input.
- **Updates**: publish a new version rather than a new item, so links and followers carry over.

## Progression and best practices

Kaggle ranks you separately in four categories (Competitions, Datasets, Notebooks, Discussions), from Novice through Contributor, Expert, Master and Grandmaster, earned through medals.

- Read top public notebooks and the **Discussion** tab of every competition you enter; winning write-ups are posted after each one ends.
- Trust your local cross-validation score over the public leaderboard.
- Keep experiments reproducible: fix random seeds and save each run as a notebook version.
- Team up only before the merger deadline, and follow the rules on private code sharing between teams.
- Respect dataset licenses and competition data-use rules.

## Quick checklist

- [ ] Account created and phone verified
- [ ] Competition rules accepted (if competing)
- [ ] Data attached at `/kaggle/input/` or downloaded
- [ ] Accelerator and internet set correctly for the task
- [ ] Baseline model with cross-validation
- [ ] Submission matches `sample_submission.csv`
- [ ] Final submissions selected before the deadline
- [ ] API token stored safely, never committed

## Sources

- [Kaggle CLI 1.8.0 release notes and auth docs](https://github.com/Kaggle/kaggle-cli/pull/863/files)
- [GPUPerHour: Free Cloud GPUs in 2026](https://gpuperhour.com/blog/free-cloud-gpus-and-credits) (cites Kaggle's notebook documentation)
