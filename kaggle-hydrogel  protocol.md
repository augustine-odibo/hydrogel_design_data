# Kaggle Protocol: Getting Started to Submission

_As of 2026-10-27_

## Purpose and scope

This protocol takes you from a new Kaggle account to a scored competition submission in six steps. [Kaggle](https://www.kaggle.com) is Google's free data science platform. It offers public [datasets](https://www.kaggle.com/datasets), cloud Jupyter [notebooks](https://www.kaggle.com/code) with free GPUs and TPUs, [competitions](https://www.kaggle.com/competitions) with leaderboards, and a [model hub](https://www.kaggle.com/models).

Follow the steps in order the first time. After that, use the [checklist](#quick-checklist) at the end as a quick reference.

**Quick links:** [Home](https://www.kaggle.com) · [Datasets](https://www.kaggle.com/datasets) · [Code](https://www.kaggle.com/code) · [Competitions](https://www.kaggle.com/competitions) · [Models](https://www.kaggle.com/models) · [Learn](https://www.kaggle.com/learn) · [Discussions](https://www.kaggle.com/discussions) · [Documentation](https://www.kaggle.com/docs)

## Step 1: Set up your account

Phone verification is required before you can use GPUs, TPUs or internet access in notebooks, so do it on day one.

1. Go to [kaggle.com](https://www.kaggle.com) and click **Register**. Sign up with Google or an email address.
2. Choose a permanent username. It appears in every dataset and notebook URL you publish.
3. Open [Settings](https://www.kaggle.com/settings) (profile picture, top right) and verify your phone number.
4. Fill in your profile (bio, GitHub, LinkedIn). Public work on Kaggle doubles as a portfolio.
5. Optional: if you are new to data science, take the free short courses on [Kaggle Learn](https://www.kaggle.com/learn): [Python](https://www.kaggle.com/learn/python), [Pandas](https://www.kaggle.com/learn/pandas) and [Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning).

## Step 2: Find and use datasets

The fastest path is to attach a dataset to a Kaggle notebook. Download it only if you work locally.

1. Open the [Datasets](https://www.kaggle.com/datasets) tab and search by keyword. Filter by file type (CSV, JSON, Parquet), size, license and usability score.
2. On a dataset page, read the **Data Card** (description, columns, source). Check the **License** before you use the data in published work.
3. To work in the cloud, click **New Notebook** on the dataset page. The files mount read-only at `/kaggle/input/<dataset-name>/`.
4. To work locally, click **Download**, or use the CLI ([Step 5](#step-5-set-up-the-kaggle-api-and-cli)): `kaggle datasets download -d <owner>/<dataset-slug> --unzip`.
5. Look at the dataset's **Code** tab. Other users' notebooks show how the data is usually loaded and cleaned.

To load a mounted file in a notebook:

```python
import pandas as pd
df = pd.read_csv("/kaggle/input/<dataset-name>/<file>.csv")
df.head()
```

Reference: [Kaggle Datasets documentation](https://www.kaggle.com/docs/datasets)

## Step 3: Work in Kaggle Notebooks

Kaggle Notebooks are free cloud Jupyter environments. GPU time comes from a weekly quota, so turn on an accelerator only when your code needs it.

1. Go to [Code](https://www.kaggle.com/code) → **New Notebook**, or start one from a dataset or competition page.
2. Open the **Settings** panel on the right (Session options) and set:
    - **Accelerator**: None (CPU), a GPU option (T4 or P100 class), or TPU. The options change over time.
    - **Internet**: turn it on to `pip install` packages. Some competitions require it off for submissions.
    - **Persistence**: keep files or variables between sessions if you need them.
3. Add data with **+ Add Input** (datasets, competitions, models, or other notebooks' outputs).
4. Save files you want to keep to `/kaggle/working/`. That folder becomes the notebook's Output.
5. Click **Save Version → Save & Run All (Commit)** to run the whole notebook in the background. You can close the tab while it runs.
6. Watch your quota in the session panel, and stop idle sessions so you don't waste hours.

| Limit | Value |
| --- | --- |
| CPU and GPU session | 12 hours max |
| TPU session | 9 hours max |
| Weekly GPU quota | Floats; commonly about 30 hours, shown in your account |
| Bonus GPU hours | +15/week (Colab Pro) or +30/week (Colab Pro+) when accounts are linked |

These limits are as reported by [GPUPerHour](https://gpuperhour.com/blog/free-cloud-gpus-and-credits) from [Kaggle's notebook documentation](https://www.kaggle.com/docs/notebooks) in September 2026. Check the live numbers in your own account.

## Step 4: Enter a competition and submit

Your score comes from a file that must match `sample_submission.csv`, so get that format exactly right before anything else.

1. **Pick a competition.** Beginners should start with a **Getting Started** or **Playground** competition, such as [Titanic](https://www.kaggle.com/competitions/titanic) or [House Prices](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques). These have no deadline pressure and many public notebooks to learn from. Browse all of them on the [Competitions](https://www.kaggle.com/competitions) page.
2. **Read the Overview.** Note the evaluation metric, the timeline (entry deadline, team-merger deadline, final deadline) and the prize rules.
3. **Join and accept the rules.** Click **Join Competition**. API and data downloads fail until you accept the rules.
4. **Study the Data tab.** You get `train` (with labels), `test` (without labels) and `sample_submission.csv`.
5. **Build a baseline.** Train a simple model first, then improve it. Use cross-validation on the training set rather than tuning only to the public leaderboard.
6. **Write the submission file** with exactly the same columns and row IDs as the sample.
7. **Submit.** Either upload the CSV under **Submit Prediction** or, in a **code competition**, submit the notebook itself (often with internet off and a runtime cap).
8. **Check the daily submission limit** on the Overview page. It varies by competition.
9. **Select your final submissions** before the deadline (usually two). These are the ones scored on the private leaderboard.

The **public leaderboard** scores a slice of the test set. The **private leaderboard**, revealed at the end, scores the rest and decides final ranks. A model overfit to the public board often drops when the private board is revealed (a "shake-up").

To write a submission file:

```python
sub = pd.read_csv("/kaggle/input/<competition>/sample_submission.csv")
sub["target"] = model.predict(X_test)
sub.to_csv("/kaggle/working/submission.csv", index=False)
```

Reference: [Kaggle Competitions documentation](https://www.kaggle.com/docs/competitions)

## Step 5: Set up the Kaggle API and CLI

The CLI lets you download data, submit predictions and push notebooks from your own terminal. As of release 1.8.0 it needs Python 3.11+ and an API token.

1. Install it: `pip install kaggle` ([PyPI page](https://pypi.org/project/kaggle/))
2. Go to [Settings](https://www.kaggle.com/settings) → **API** on kaggle.com and click **Generate New Token**.
3. Store the token in one of two ways:
    - As an environment variable: `export KAGGLE_API_TOKEN=<your token>`
    - In a file: save the token to `~/.kaggle/access_token`
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

References: [Kaggle API documentation](https://www.kaggle.com/docs/api) · [Kaggle CLI on GitHub](https://github.com/Kaggle/kaggle-cli) · [Release 1.8.0 auth changes](https://github.com/Kaggle/kaggle-cli/pull/863/files)

## Step 6: Publish your own work

Everything you create starts private. Make it public only after checking that it holds no secrets or restricted data.

- **Notebooks**: save a version in [Code](https://www.kaggle.com/code), then change sharing to **Public**. Add a clear title, a markdown intro and comments so others can follow it.
- **Datasets**: go to [Datasets](https://www.kaggle.com/datasets) → **New Dataset** and upload files (or link a GitHub repo, URL or notebook output). Then fill in the description, column notes, source and license. Complete metadata raises the usability score.
- **Models**: go to [Models](https://www.kaggle.com/models) → **New Model** to publish trained weights with a model card, framework and license. Notebooks can then attach the model as input.
- **Updates**: publish a new version rather than a new item, so links and followers carry over.

## Progression and best practices

Kaggle ranks you separately in four categories (Competitions, Datasets, Notebooks, Discussions). The tiers run from Novice through Contributor, Expert, Master and Grandmaster, and you climb them by earning medals. See [Kaggle Progression](https://www.kaggle.com/progression) and the [Rankings](https://www.kaggle.com/rankings) page.

- Read the top public notebooks and the [Discussion](https://www.kaggle.com/discussions) tab of every competition you enter. Winners post their write-ups after each competition ends.
- Trust your local cross-validation score over the public leaderboard.
- Keep experiments reproducible: fix random seeds and save each run as a notebook version.
- Team up only before the merger deadline, and follow the rules on private code sharing between teams.
- Respect dataset licenses and competition data-use rules.

## Quick checklist

- [ ] [Account](https://www.kaggle.com) created and phone verified in [Settings](https://www.kaggle.com/settings)
- [ ] Competition rules accepted (if competing)
- [ ] Data attached at `/kaggle/input/` or downloaded
- [ ] Accelerator and internet set correctly for the task
- [ ] Baseline model with cross-validation
- [ ] Submission matches `sample_submission.csv`
- [ ] Final submissions selected before the deadline
- [ ] API token stored safely, never committed

## Appendix: Hydrogel fabrication and formulation datasets

None of these are on Kaggle. To use one in a Kaggle notebook, download it and upload it as a private dataset (see [Step 6](#step-6-publish-your-own-work)).

| Dataset | What it contains | Link |
| --- | --- | --- |
| pNIPAM lyogels (TUHH, 2025) | Formulation ratios (MBA:NIPAM, NIPAM:PVA), degree of swelling, NMR relaxation time, compressive Young's modulus | [Dataset page](https://tore.tuhh.de/entities/product/8d75ad80-f2a7-42f0-9a30-798004e3e437) |
| Polyacrylamide (Mendeley, 2022) | Rheology, optical and swelling data for monomer-to-crosslinker ratios of 60:1 and 15:1 | [Dataset page](https://data.mendeley.com/datasets/c3rxvdgbrs) |
| GelMA and PEGDA (Mendeley, 2024) | Mechanical tests, ultrasound and rheology versus concentration (% w/v) and curing time | [Dataset page](https://data.mendeley.com/datasets/cx8cfgrhnn/1) |
| HEA organogels (TUHH, 2026) | Monomer and crosslinker wt%, gravimetric swelling with 5 replicates per sample | [README](https://tore.tuhh.de/bitstreams/614edbb0-a066-4aaf-8068-81987aaced9e/download) |
| Gelatin hydrogel fibers (Zenodo) | Thermal-drawing fabrication data, as per-figure Excel files | [Zenodo record](https://zenodo.org/records/18262316) |
| PVA hydrogel mechanics (Gels, 2025) | 350 literature data points on formulation and processing versus tensile strain | [Paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12294283/) |
| Underwater adhesive hydrogels (Nature, 2025) | 180 to 341 bioinspired formulations versus adhesive strength | [Paper](https://www.nature.com/articles/s41586-025-09269-4) |
| Review of hydrogel datasets (2026) | Printability databases (1,568 assays; 150 DIW hydrogels) and others | [Review](https://pmc.ncbi.nlm.nih.gov/articles/PMC13205778/) |

## Sources

- [Kaggle documentation](https://www.kaggle.com/docs)
- [Kaggle CLI 1.8.0 release notes and auth docs](https://github.com/Kaggle/kaggle-cli/pull/863/files)
- [GPUPerHour: Free Cloud GPUs in 2026](https://gpuperhour.com/blog/free-cloud-gpus-and-credits) (cites Kaggle's notebook documentation)
