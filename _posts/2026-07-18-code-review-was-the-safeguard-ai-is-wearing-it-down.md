---
layout: post
title: "Code Review Was the Safeguard. AI Is Wearing It Down."
date: 2026-07-18 05:00:00 -0600
excerpt: "AI is making it easier to write code and harder to review it well. Here's why, and what teams can do about it."
reading_time: 11
image: /assets/images/posts/code-review-was-the-safeguard-ai-is-wearing-it-down.png
---

![Code Review Was the Safeguard. AI Is Wearing It Down.](/assets/images/posts/code-review-was-the-safeguard-ai-is-wearing-it-down.png)
*Image: ChatGPT Image*

It was 1979, in the early days of computing, when an internal [IBM training manual included the prophetic quote](https://dotneteers.net/the-1979-ibm-presentation-reflections-on-accountability-in-the-age-of-ai/) "A computer can never be held accountable. Therefore, a computer must never make a management decision". While technology and automation have evolved dramatically since then, prudent organizations have largely upheld this principle, ensuring that humans remain accountable for outcomes.

In software development, one of the clearest expressions of that accountability is in the pull request (PR) review.

Teams building software use PR reviews to share knowledge and changes, mentor juniors, discuss tradeoffs and, most importantly, maintain quality, accountability, and correctness in shipped code. With the rise of LLM-assisted coding, and a [documented increase](https://x.com/kdaigle/status/2040164759836778878) in code output, this gate is more important than ever. Review is often cited by CTOs and AI leaders as the safeguard: there's no need to worry about AI-generated code, humans check it first.

The reality is more complicated. [Evidence from other domains](https://arxiv.org/abs/2505.16339) shows that increased reliance on automation leads to measurable effects such as automation bias and skill erosion, as seen in radiology and aviation. Perhaps expectedly, early signals suggest software engineering may be following a similar pattern.

I see four reasons for this:

#### 1. Skill Atrophy

Many experienced developers, myself included, already feel this intuitively: reviewing code is a "use it or lose it" skill. Writing less code manually (and doing less of the deep thinking that accompanies it) is making it harder to evaluate code critically.

Michael Lawrence's ["AI Deformation" series](https://levelup.gitconnected.com/ai-isnt-replacing-developers-it-s-doing-something-worse-2d18fb595362) explores this idea in depth. His core argument is that AI is not replacing developers, but deforming them by removing the friction that once drove learning. For junior and intermediate engineers, this can stunt the development of judgement. For seniors it risks eroding hard-won intuition.

Whatever the mechanism, the output is concerning: the people closest to the code may be less able to assess it rigorously.

#### 2. Volume Pressure

The bottleneck in software delivery [has shifted](https://arxiv.org/abs/2508.11034) from development to review. AI is making it easier to produce code quickly, but that also means review queues are getting longer. When shipping speed remains a priority, review fatigue follows. PRs get skimmed, attention drops, and more slips through as engineers spend less time per review.

#### 3. Deference Bias

[Algorithmic Deference Bias](https://javier-marin.medium.com/towards-a-new-psychology-of-human-ai-interaction-91ef58e1bb07), part of the broader class of Authority Biases, outlines how users "systematically lower their critical evaluation standards for AI-generated content compared to identical content from human sources". In practice, this means developers may apply less scrutiny to AI-generated code, and overly trust the output of AI-generated code reviews.

This is exactly backward. AI-generated content should receive equal, if not greater, scrutiny. Treating it as inherently reliable undermines the very purpose of review.

#### 4. Context Collapse

LLM-Generated code can be larger and more self-contained than human changes, making it harder to assess how a change fits the broader system. Without sufficient context brought into the review (think a new intermediate engineer approaching a review), reviews become similarly narrowly focused. Engineers evaluate what is immediately visible, but miss system-level implications.

None of this is an argument against AI-assisted development. The productivity gains are real and they are here to stay. It's an argument for treating review as a discipline that has to keep pace.

---

It's difficult to predict how both the nature of software development and the role of a software engineer will evolve. That said, the judgement and accountability of experts will continue to keep software engineering skills relevant. So, as humans, how can we return to impactful reviews? From both research and experience, three themes stand out: making code easier to review, using LLMs as reviewers, and actively maintaining human judgement.

#### 1. Write code that is easy to review

Software engineering has always been a human discipline. The code we write must foremost be understood by our peers. This principle of readability remains more true than ever as AI is generating more of the code.

- Keep changes small and focused. Smaller PRs are easier to reason about and reduce cognitive load for reviewers. Work as a team to divide and sequence work appropriately to enable this.
- Separate refactors from behavioural changes. Refactors should not meaningfully alter tests whereas feature changes should. If both are required, split them into multiple PRs.
- Use tests as a signal. Unit tests remain one of the clearest indicators of intent and correctness. Unexpected changes to tests can signal that generated code is bloated or misaligned with the original request.

Agent-based development practices, such as decomposing problems into smaller tasks, naturally support this. They do not just improve generation quality; they also make review tractable.

#### 2. Use LLMs as an additional reviewer

LLMs excel at tirelessly reviewing code, summarizing changes and identifying issues. Ignoring this capability is a missed opportunity.

- More recent generations of LLMs are quite good at catching issues in code. They should be treated like linters and static analysis tools: useful, but not authoritative.
- Run automated diffs on every PR, with comments added directly to the diff. This takes care of the first piece every reviewer will look for: what changed?
- Have the author triage LLM feedback prior to human review, resolving valid issues and filtering noise.
- With respect to the review prompts, tune prompts to reflect senior-level expectations. Have them include in their comment the severity, impact and suggested fixes. A good example of such a prompt is [provided by Copilot](https://docs.github.com/en/copilot/tutorials/customization-library/prompt-files/review-code).
- Similarly, customize review focus based on context: security and compliance in regulated industry, performance for media apps, etc.
- Consider multiple models for higher-stakes changes to reduce blind spots.

Beyond PRs, periodic full-repository reviews (human or LLM-assisted) can surface systemic issues such as the accumulation of tech debt or broader security/performance issues, that incremental changes miss.

#### 3. Maintain and train human judgement

The most subtle risk in AI-assisted development is not bad code, but rather degraded judgement.

In his [essay](https://levelup.gitconnected.com/how-to-use-ai-without-getting-worse-at-your-job-747fc7d91338), Michael Lawrence argues for the continuation of human growth quite eloquently:

> "[Keeping up your human Judgement] is maintenance. It is the same routine, unexciting upkeep as keeping your tests passing and your dependencies current. You keep the system healthy so it works when you need it. You are part of the system."

So, how do we do this? Lawrence warns against the obvious of asking whether a task "feels" like something you've already mastered. This question invites self-deception as "I already know this" is the convenient answer and lets you skip the hard part and feel efficient instead of avoidant. He proposes a sharper, harder-to-fake test: could you write this from scratch with the tool closed, or would you only recognize the answer once it appeared? Recognizing an answer isn't the same as knowing it, and it's easy to get this wrong in your own favor often enough.

Lawrence cites two concrete habits to counter this:

- Before using AI, attempt the problem yourself, even briefly. This forces engagement and creates a baseline for evaluation.
- After shipping, try explaining not just what the code does, but why it works, how it behaves under stress, and how it would change under new requirements.

Another practice that preserves judgment and human connection is "Code Walkthroughs" for larger changes. Engineers meet in person or virtually, and the implementer walks through the change while the group reviews it live. Bringing multiple reviewers together creates space for perspectives to collide and broaden. It also gives junior engineers a chance to learn from seniors and ensures the implementer understands the code well enough to lead the discussion.

Since AI assisted development isn't going away, the real work is watching our blind spots and accounting for the new gaps it introduces in ourselves and our processes. The tradeoff is simple to state and hard to live: time saved today against judgment lost tomorrow. What separates the teams that keep their edge from those that don't will be whether they kept reviewing as seriously as they kept shipping.

This post was written by me; AI assistance was used for selected rewording and editing for verbosity. Views are my own, not my employer's.
{: .disclosure }
