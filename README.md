# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

I am using the r/eagles community on Reddit, which is a subreddit for fans of the Philadelphia Eagles NFL team. My classifier sorts posts into four labels: `news`, `analysis`, `hot_take`, and `reaction`, based on the main purpose of the post. The goal is to distinguish emotional responses, unsupported opinions, factual news, and evidence-based arguments.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `news`

**Definition:** A post that primarily reports factual information, statistics, or updates from reporters or official sources about the Eagles without adding significant personal interpretation or opinion.

**Example 1:**
> [Lane Johnson] Thank You 🦅 [Body contains picture of a statement from Lane Johnson announcing his retirement]

**Example 2:**
> [Garofolo] The #Eagles are releasing QB Andy Dalton, sources say. Tanner McKee and Cole Payton will be the backups to Jalen Hurts moving forward.

### `analysis`

**Definition:** A post that evaluates or explains something about the Eagles using specific, checkable evidence that meaningfully supports its main conclusion.

**Example 1:**
> Despite the loss, Carter had his best game by far for the year: 5 TKL, 3 TFL, 2 SACK, 2 FF, 1 PD. Included a pivotal goal line stop.

**Example 2:**
> One concern coming into the season that has turned into a positive is Jake Elliott. 4/4 on FG’s, including two makes from 50 plus. And 9/9 on XP’s. Glad to see him back in the groove again.

### `hot_take`

**Definition:** A post that primarily makes a strong opinion, judgment, demand, or prediction about the Eagles without developing sufficient evidence-based reasoning to support its conclusion.

**Example 1:**
> Time to fire Sirianni - Yet another terrible game management decision. 8 seconds to go, they have no timeouts, they already took one to the house on a kickoff...yet we don't kick a touchback. I personally have given so much grace to this guy, but I'm done. I'm done watching the eagles as long as he is the coach.

**Example 2:**
> Jalen Hurts should receive NO blame for this game, after carrying the team without Smitty, Saquon and half the OL. Sirianni blew it again.

### `reaction`

**Definition:** A post that primarily expresses personal feelings, appreciation, excitement, frustration, or disappointment about an Eagles-related event rather than developing a broader argument or judgment.

**Example 1:**
>THANK YOU CAM - Gutted it out in the Super Bowl, and one of the biggest reasons for Saquon's historic season. Will be rooting for him in Baltimore.

**Example 2:**
>Don't cry because it's over, smile because it happened. - Remember what these guys did for us in the years to come. Greatest rushing season of all time.

### The hardest boundary

**Which two labels:** `analysis` and `hot_take`

**The decision rule I used every time:**
If a post uses specific, verifiable evidence to explain or evaluate something and that evidence meaningfully supports its main conclusion, I classify it as `analysis`. If a post mainly makes a strong statement without enough supporting evidence, I classify it as `hot_take`. Simply mentioning a statistic or specific event is not enough to qualify as analysis.

<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** I got the posts from the r/eagles subreddit, sorted by Top This Month. I tried a few different methods to collect them, including manually copying posts, JavaScript, Python scripts, and ChatGPT. I started with around 400 posts, but a lot of them had pictures, videos, or links instead of written bodies. I ended up removing posts that didn't have enough context on their own, leaving me with 158 examples.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I first labeled 20 posts on my own. Then I used ChatGPT to help label the rest based on my definitions. I went through the suggestions, especially the harder ones, and changed some labels that didn't fit. I also removed a few posts that relied too much on pictures or videos.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| news | 71 | 44.9% |
| analysis | 30 | 19% |
| hot_take | 27 | 17.1% |
| reaction | 30 | 19% |
| **Total** | 158 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1. Jalen Hurts should receive NO blame / Sirianni blew it again**

> *The post:* "Jalen Hurts should receive NO blame for this game, after carrying the team without Smitty, Saquon and half the OL. Sirianni blew it again."
>
> *Could have been:* `analysis` or `hot_take`.
>
> *I chose:* `hot_take`, because the missing players are relevant context but do not establish the stronger judgments that Hurts deserves *no* blame or that Sirianni caused the loss.

**2. Fangio's refusal to adjust**

> *The post:* "Fangio’s refusal to adjust needs to seriously be discussed. We are last in the entire league in quick pressure rates... Fangio is refusing to accept that his front 4 can’t do it."
>
> *Could have been:* `analysis` or `hot_take`.
>
> *I chose:* `hot_take` after initially considering `analysis`. The pressure statistic supports concern about the pass rush but does not demonstrate that Fangio refused to change his approach. Mentioning a statistic isn't enough unless it supports the actual conclusion.

**3. Three touchdowns in the final seconds of halves**

> *The post:* "[Fun Fact] The Philadelphia Eagles have 3 touchdowns within the final 10 seconds of a half so far this season" followed by three specific touchdowns and their times.
>
> *Could have been:* `news` or `analysis`.
>
> *I chose:* `news`, because this primarily reports a collection of checkable statistics. The positive reaction at the end does not turn it into an explanation or evaluation supported by evidence.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**
`distilbert-base-uncased` (DistilBERT)

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, maximum length 128 tokens, and seed 42. Trained on my Mac using the Apple Silicon GPU (`mps`) with PyTorch 2.14.1.

**Anything I changed from the defaults, and why:**
I kept the default training settings. The only change I made was setting `LABELS` to my four labels: `analysis`, `hot_take`, `reaction`, and `news`.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

| Label | Train | Validation | Test |
|---|---:|---:|---:|
| analysis | 21 | 4 | 5 |
| hot_take | 19 | 4 | 4 |
| reaction | 21 | 5 | 4 |
| news | 49 | 11 | 11 |
| **Total** | **110** | **24** | **24** |

Three labels (`analysis`, `hot_take`, and `reaction`) have fewer than 8 examples in the test set. This is mainly because I ended up with 158 total posts instead of 200, so the test set is pretty small. With so few examples per label, their F1 scores could vary a lot between different training seeds. I'll keep that in mind when evaluating the results in Unit 6.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I used ChatGPT to help me figure out the differences between my four labels, especially `analysis` and `hot_take`. Some posts mentioned statistics or specific plays, but I wasn't sure if that automatically made them analysis.
- *What came back:* ChatGPT helped me compare borderline examples, including a post calling for Sirianni to be fired because of a specific coaching decision. It initially suggested that the post could count as analysis.
- *What I changed:* I disagreed because the post was mostly a strong opinion rather than actual analysis. I refined my decision rule so that evidence needs to meaningfully support the main conclusion, not just be mentioned.

**Moment 2**

- *What I asked for:* I used ChatGPT to help with collecting and organizing Reddit posts because getting the full post bodies was difficult. I also asked it to suggest labels for the posts after I labeled my first 20 on my own.
- *What came back:* ChatGPT helped me explore different collection methods and suggested labels using my definitions. Some posts were difficult to classify because they mixed opinions, reactions, and statistics.
- *What I changed:* I reviewed the suggested labels and corrected some that didn't fit my definitions. I also removed posts that relied too much on pictures, videos, or links without enough written context.

**Pre-labelling disclosure:**

I labeled the first 20 posts myself without AI assistance and marked them `cold` in `labels.csv`. For the remaining posts, I used ChatGPT to suggest labels, then reviewed the suggestions and made corrections where needed.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.208 | 0.458 | +0.250 |
| Macro F1 | 0.086 | 0.157 | +0.071 |
| F1 — `analysis` | 0.345 | 0.000 | -0.345 |
| F1 — `hot_take` | 0.000 | 0.000 | +0.000 |
| F1 — `news` | 0.000 | 0.629 | +0.629 |
| F1 — `reaction` | 0.000 | 0.000 | +0.000 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I predict my trained model will perform better on news posts since they make up the largest part of my dataset. However, I think the baseline might perform better overall because my trained model struggled to distinguish the other three labels.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
My trained model performed better overall, with 45.8% accuracy compared to the baseline's 20.8%. However, most of that improvement came from predicting news for every post, which was also the most common label in my test set. The baseline actually did better on analysis, but failed on the other three labels. This shows that fine-tuning improved accuracy, but my model still hasn't learned how to properly distinguish between all four categories.

I also tested the baseline using only the label names without definitions. Surprisingly, it performed better, improving from 20.8% to 33.3% accuracy and from 0.086 to 0.220 macro F1. This suggests that my definitions didn't help the model distinguish the categories as well as I expected.

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
