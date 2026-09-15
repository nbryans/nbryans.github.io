---
layout: post
title: "Who Reviews the Reviewer?"
date: 2026-09-15 04:00:00 -0600
excerpt: "A talk about measuring LLM code reviewers. They catch almost every planted bug — and invent new ones with exactly the same confidence."
reading_time: 7
image: /assets/images/posts/who-reviews-the-reviewer.jpg
---

This is a written version of a talk I gave at [Calgary AI Tinkerers](https://calgary.aitinkerers.org/) on August 27, 2026. The code is on GitHub at [**nbryans/phantombench**](https://github.com/nbryans/phantombench). It's a harness that takes real merged PRs (e.g. from OSS projects), injects a synthetic defect into them, and scores the clean vs buggy code against several models.

<div class="walkthrough">

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/01-title.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/01-title.webp" alt="Title slide: Who Reviews the Reviewer? Exploring LLM code reviews."></a>
  <div class="step-note" markdown="1">
I'm a Senior ML Engineer and platform tech lead, I've found the number of PRs I'm reviewing has gone up substantially.

Tools like Claude Code and Cursor let the team push PRs faster than ever, which means more for all of us to review. As the queue piles up, I keep coming back to one question about this part of the workflow: are LLMs helping or hurting us as reviewers?
  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/02-premise.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/02-premise.webp" alt="Premise slide: automation bias and skill erosion are measured effects, in radiology and aviation. Three practices to counter it."></a>
  <div class="step-note" markdown="1">
I wrote about this in July, in [Code Review Was the Safeguard. AI Is Wearing It Down.]({% post_url 2026-07-18-code-review-was-the-safeguard-ai-is-wearing-it-down %}) The short version: automation bias and skill erosion are measured effects in other domains like radiology, aviation where automation has taken over, and code review is the same shape of problem.

I proposed three practices to counter it. Write code that's easy to review. Use LLMs as an additional reviewer. Maintain and train human judgement.

That middle one is an assertion based on first hand experience. So this project is me trying to test it: if you're going to put an LLM in the review loop, how would you know whether it's any good?
  </div>
</div>

</div>

## The method

<div class="walkthrough">

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/03-method.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/03-method.webp" alt="Method slide: 12 real merged PRs used twice — once with an injected defect, once unmodified — reviewed by three models."></a>
  <div class="step-note" markdown="1">
My first instinct was to write code from scratch with true and false bugs in it. I talked myself out of it: hard to justify, hard to prove correct, and a lot of work.

Instead I took real merged PRs from an open-source project (`langfuse-python`) and injected defects into copies of them. Each PR is reviewed twice — once modified, once untouched as a control — so every model sees both a PR that has something wrong with it and one that (supposedly) doesn't.

Some discussion on my choices
1. Using `langfuse-python` as the source of my original PRs would make grading easier as it's a package/tool I use daily.
2. I wanted to test **workhorse models**, not SOTA frontier, as these are most commonly wired into automated review pipelines.
3. Finally, I opted to use the diff-only (no repo access) for my reviewers. This is kind of a limitation I'll come back to.
  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/04-worked-example.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/04-worked-example.webp" alt="Slide showing PR 1745: the real defensive fix, and the injected variant that logs the first completion instead of the last."></a>
  <div class="step-note" markdown="1">
A concrete example. [PR #1745](https://github.com/langfuse/langfuse-python/pull/1745) is an ordinary merged change by a real package contributor: a small defensive fix where a value could come back as `None` when the key is present but empty, and the expected behaviour is `[]`.

The injected variant changes one line. Where the code took the last element of `choices`, it now takes the first with the effect of the recorded output silently becomes the first generated completion rather than the one actually returned. At the default `n=1` it's indistinguishable from correct.

In the repo, defects are patch files under `defects/`, so any of them can be regenerated or put on screen live. All three models caught this one, though they disagreed on how much it mattered: Claude called it blocking, GPT and Gemini should-fix.

Every patch includes a `detection_hint` written *before* any model output was seen, which helped with scoring.
  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/05-defect-classes.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/05-defect-classes.webp" alt="Appendix slide: five classes of defect, from local_mechanical through silent_semantic, with per-model catch counts."></a>
  <div class="step-note" markdown="1">
Twelve patches across five classes, ordered roughly easiest to hardest, from a wrong constant or index that's visible without leaving the diff, up to a semantic shift where meaning changes and the code keeps working.

Perhaps as expected, `silent_semantic` performed the worst (probably a callback to the lack of additional repo context).
  </div>
</div>

</div>

## Grading it

<div class="walkthrough">

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/06-grading.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/06-grading.webp" alt="Grading slide showing a local annotation dashboard for tagging 87 findings by hand."></a>
  <div class="step-note" markdown="1">
Reviewing findings through the command line at first, then an excel sheet, was miserable. I had Fable credits and decided to spend them building an annotation dashboard instead — one shot, about 8 minutes, $6 in credits.

This resembles familiar diff tools in git reviews. During review, I opted to hide the model's own draft tag until my annotation was entered (to make sure I didn't just end up agreeing with the machine). Therefore, I tagged 87 findings by hand.
  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/07-hard-part.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/07-hard-part.webp" alt="Slide: grading a false positive requires knowing the codebase."></a>
  <div class="step-note" markdown="1">
Catching a real defect is easy to score (I wrote the defect, I know what it was). Scoring a False Positive is harder. When a model says "this will break under concurrent access," the only way to tag that is to go read the code and find out.

I'm familiar with langfuse-python, but I don't know it intimately. Verifying every "unrelated" and "false positive" tag meant wading through unfamiliar code by hand. Which is exactly what a reviewer has to do, and exactly the context collapse the safeguard exists to prevent. The grading is harder than the injection and harder than the prompt.
  </div>
</div>

</div>

## What came back

<div class="walkthrough">

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/08-findings.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/08-findings.webp" alt="Chart: defects caught versus false positives per clean PR. GPT 12/12 at 0.58 FPs, Claude 11/12 at 0.83, Gemini 8/12 at 1.17."></a>
  <div class="step-note" markdown="1">
The (code) reviewers were good at catching real defects on these small, limited-context changes. GPT described 12 of 12, Claude 11, Gemini 8.

They were also excellent at inventing them. Across the 12 clean, unmodified control PRs: 0.58 false positives per PR for GPT, 0.83 for Claude, 1.17 for Gemini.

`n` is small (12) for this analysis, so not making any sweeping claims. The point is more the shape of the chart. Catch rate is nearly saturated on changes this small, so the separation between models is almost entirely on the vertical axis. If you only measure whether the reviewer caught the bug, all three look about the same.
  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/09-numbers.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/09-numbers.webp" alt="Appendix slide with the underlying numbers: 87 findings, 39% of false positives tagged blocking, 25 of 28 real catches also blocking."></a>
  <div class="step-note" markdown="1">
Two numbers from the worksheet do most of the work.

**39% of the false positives arrived tagged `blocking`** — 12 of 31. And 25 of the 28 real catches were tagged `blocking` too. The severity label carries no signal about whether the finding is real. You cannot triage by it, which means the reviewer can't tell you when to trust it.

**Gemini caught zero defects GPT didn't.** The union of all three models is 12 of 12 — exactly what GPT got on its own — at 2.58 false positives per clean PR instead of 0.58. Running the ensemble quadrupled the triage burden and found nothing extra.

Also worth noting: 16 of the 72 responses returned nothing at all, and GPT stayed correctly silent on 8 of the 12 clean PRs, Claude on 6, Gemini on 2. All of it is in `data/scores/worksheet.csv`.
  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/10-real-bugs.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/10-real-bugs.webp" alt="Slide: three real bugs caught in clean code — PRs 1577, 1709 and 1792."></a>
  <div class="step-note" markdown="1">
Interestingly, on the clean control PRs — where by construction there was no defect to find — Claude produced four findings that turned out to be real, hand-verified bugs still on `main`. Two of them share a root cause, so calling this three distinct bugs.

One of these, a `None` silently stringified to `"None"` in `propagation.py`, was flagged by Langfuse's own automated reviewed and ignored.
  </div>
</div>

</div>

## Where this goes

<div class="walkthrough">

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/11-whats-next.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/11-whats-next.webp" alt="Slide: four directions — better defects, more context, open weights, prompts."></a>
  <div class="step-note" markdown="1">

  </div>
</div>

<div class="walkthrough-step">
  <a class="slide-link" href="/assets/images/posts/who-reviews-the-reviewer/12-fork-it.webp" target="_blank" rel="noopener"><img class="slide-shot" src="/assets/images/posts/who-reviews-the-reviewer/12-fork-it.webp" alt="Slide: point it at your own repo. github.com/nbryans/phantombench"></a>
  <div class="step-note" markdown="1">
Instructions are in the README: [**github.com/nbryans/phantombench**](https://github.com/nbryans/phantombench).
  </div>
</div>

</div>

## What I took away

Fabricated vs real findings are indistinguishable without more context. Similarly, the automated reviewers had no calibrated sense of their own reliability, making the `blocking` tag almost useless.


That puts the load back on the human, which is a key point in my previous argument. An LLM reviewer is a genuinely useful second pair of eyes; it found three real bugs nobody else did. But it produces findings faster than you can verify them, and verifying them is the part that requires actually knowing the code. If review was already the bottleneck, adding a reviewer that generates roughly one plausible false alarm per clean PR doesn't obviously widen it.

This is, of course, a simplification and in my experience more context + better models reduces a lot of this noise. Overall this was a fun project and a chance to try out Fable.


Numbers here are hand-scored by me, n=12, on a single repository, with diff-only context. Treat them as a case study only (rather than a benchmark or claims on model performance). Views are my own, not my employer's.
{: .disclosure }
