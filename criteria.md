# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. The classifier should achieve an overall accuracy of at least 70%.

<!-- Your criterion. It must name a number. -->

**Why this target:**

Since my classifier uses four labels (news, analysis, hot_take, and reaction), I want it to correctly classify most Eagles posts. I chose 70% because it is significantly better than random guessing with four balanced labels (25%), but still accounts for overlap between categories, especially analysis and hot takes.

---

## 2. The classifier should achieve a macro F1 score of at least 0.60.
<!-- Your criterion. -->

**Why this target:**

I chose this criterion because overall accuracy alone could hide poor performance on less common labels. Since my dataset has four categories that may not be perfectly balanced, I want the model to perform reasonably well across all of them rather than relying on one or two dominant labels.

---

## 3. The `analysis` label should achieve an F1 score of at least 0.50.
<!-- Your criterion. -->

**Why this target:**

While browsing r/eagles, I noticed that genuine analysis posts were harder to find than news updates or opinions. Analysis can also overlap with hot_take, since some opinions mention statistics or specific plays without providing meaningful reasoning. I chose 0.50 because I expect this to be one of the more difficult labels for the model to learn, especially with a relatively small dataset.

---

## 4. No single label should account for more than 50% of the 200 labeled posts in labels.csv.
<!-- Your criterion. -->

**Why this target:**

I noticed that r/eagles contains many news reports and reposts, which could easily dominate the dataset. I chose 50% because it seemed like anything more would be excessive.

---

## 5. The accuracy of the model's highest confidence predictions should be at least 15 percentage points higher than the accuracy of its lowest confidence predictions.
<!-- Your criterion. -->

**Why this target:**

Some Eagles posts are straightforward to classify, while others fall between labels such as analysis and hot_take. I want the model to be more reliable when it makes confident predictions. I chose a 15 percentag difference because it would show that confidence provides useful information, while keeping in mind the small sample size might cause inconsistency.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
