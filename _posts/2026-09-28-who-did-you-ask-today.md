---
layout: post
title: "Who Did You Ask Today?"
date: 2026-09-28 05:00:00 -0600
excerpt: "Remote work moved our connections into small questions. Now AI is answering them. Here's what remote teams can do about it."
reading_time: 7
image: /assets/images/posts/who-did-you-ask-today.jpg
---

![A reminder notification reading "Call your mother", with "mother" crossed out and "coworker" handwritten above it.](/assets/images/posts/who-did-you-ask-today.jpg)
*Image: Made with Claude*

Years ago, Facebook posts were reminding us to call our mothers with questions. Sure, we could just google things like "I'm out of lemons, can I use vinegar?" or "how to get a popsicle stain out of my dress shirt?", but the idea was that calling your family kept you close and let them feel needed.

As LLMs become the preferred do-everything tool in the corporate world, should we do the same with our coworkers?

When I became a manager, the book I relied on the most was Camille Fournier's *The Manager's Path* (I still recommend this book to anyone who wants to advance as an IC or manager in software-focused teams). Recently she published a written version of her talk, [*The Manager's Path in the Age of AI*](https://skamille.medium.com/the-managers-path-in-the-age-of-ai-279cb6611d66), and one point stuck with me. When we take our questions to Glean, Slackbot, or Claude, she writes, "you aren't talking to your colleagues". We skip the small, low-stakes favours that build trust across a team. Her talk is written for managers, and I wanted to approach this from the other side, as someone who builds these tools and works on a remote team.

In my 15-year career, two large shifts have changed how I communicate with my colleagues.

The first started in March 2020 when COVID-19 sent everyone home to work. At the time I was working a hybrid arrangement. Days in the office were punctuated with non-stop hallway chats (thanks open concept office) and lunch with coworkers. Focus time was at a premium, but touch points were never higher. When we went home, face time went with it. What emerged was the Slack ping "got 5 minutes?", and asking for help became one of the main ways I actually talked to my team. This is backed up by studies, including [one from Microsoft](https://pubmed.ncbi.nlm.nih.gov/34504299/) showing that firm-wide remote work made collaboration networks more static and siloed.

The second shift is much more recent and occurred with the adoption of ChatGPT and other LLMs in 2022. When stuck with a tricky problem in the past, I'd first turn to internal wikis, Stack Overflow, and Google to try and power through. If I couldn't figure it out in a reasonable amount of time, out would come the aforementioned ping and we'd either work through it in chat, or have a huddle to discuss.

Less and less does this need to happen with LLMs, which are trained on years of web forum answers, and with access to internal Search and RAG capabilities. Now when I'm stuck, my LLM is usually able to come up with a path forward (eventually). From a focus and self-serve perspective this is great. I interrupt coworkers less and I can power through issues after hours. This sentiment is shared [by Anthropic itself](https://www.anthropic.com/research/how-ai-is-transforming-work-at-anthropic) who found that Claude became the first stop for questions that used to go to colleagues, with some engineers reporting fewer mentorship and collaboration opportunities as a result. I should admit something here: I work on AI platform tooling, so I'm part of this shift. By most of the measures my field cares about, a question that used to take a Slack thread and now gets answered in thirty seconds is a success.

**The pandemic moved connections into small questions. Now LLMs are answering those questions.**

To be clear, this isn't a case for going back to the office. I work remotely and plan to keep it that way. It's the opposite: remote teams already run on these small questions, so when they disappear there's no hallway to fall back on.

In my career I've transitioned from IC, to management, and back to IC. As a manager, I always knew what people were working on. Some of this came from status meetings, but most of it came from 1:1s and small whiteboard sessions. Those were where I built rapport, caught people heading down rabbit holes, and found our missed requirements. As an IC, the same understanding came from impromptu chats. Individually I'm more productive than ever. But I'm having a harder time explaining what my colleagues are working on. So what can we do? Here's what I've started doing, and what I'd encourage teams to try.

#### 1. Ask a human first, for the why questions

The whats are usually written down somewhere: in the docs, the code, a README. An LLM with access to internal search will find them faster than I will. The whys, on the other hand, mostly live in people's heads. "Why was this built this way?" "Why should we use your team's internal platform?" "Why does a networking ticket take a week to action?" Those answers rarely make it into the wiki (or are out of date and sanitized).

So I've started splitting my questions. The what goes to the bot, then the why goes to a person. It's slower, but the why tends to come with context I didn't know to ask for: the incident behind the design, the team that quietly depends on it, what they'd do differently now. That's often where I spot a rabbit hole before falling in, and the conversation invites an opportunity for productive, human discussion.

#### 2. Share your AI conversations in public channels

LLMs aren't going away, but one thing we can make the deliberate choice to do is share our answers in public channels, and ask our colleagues if the results make sense. Either they do, and we are now jointly educated about the direction someone is going, or they don't and we get to have an honest discussion about them.

For those of us building these tools, this is also a design choice. An internal assistant can end its answer with who owns the system, or who wrote the doc, not just a link to it. A good answer should point you to a person.

#### 3. Keep code walkthroughs, and try pairing with an agent

[Recently, I argued for code walkthroughs](/posts/code-review-was-the-safeguard-ai-is-wearing-it-down/) on larger changes, where the implementer walks the group through a change live. Walkthroughs are one of the few places left where someone has to explain the why out loud, to people, in real time.

I'd add a newer (older?) practice: pairing with an agent. This is pair programming, but you're allowed to bring a coding agent (think Claude, Cursor, etc.). One person drives, the other navigates. While the agent does most of the typing, it's the conversation between the two humans about what to ask for, and whether to accept what comes back, that's most interesting. The conversation is where rabbit holes get caught and missing requirements surface, the same things I used to catch in small whiteboard sessions. If nothing else, at the end two people understand the change instead of one.

#### 4. Give juniors a named human buddy

Juniors have the most questions, and they're often the first to take them to a bot. Michael Lawrence makes this case well in [*The Junior Developer's Trap*](https://levelup.gitconnected.com/the-junior-developers-trap-2b7ba8f02591): "AI removes the friction. The friction was the curriculum". Asking a senior for help was part of that friction. It was also how juniors got to know the team (and vice versa). Assigning a named buddy, and making it clear that questions are welcome, is an old solution we should be deliberate about again.

#### 5. Prioritize human conversations

In her talk, Fournier makes a point I agree with: If AI speeds us up, we have to decide what to do with the time it frees up. Some managers might argue that more time = more tickets. I'd argue we're better off spending some of it on connection. Book recurring chats with colleagues, even if only monthly, the same way you'd book a 1:1 with your manager.

This is something I've become deliberate about. I mostly work from home and I like it that way, but I've been trying to more often work downtown in a coworking space and book coffees and lunches around it: former colleagues, people I've met at meetups, the occasional LinkedIn connection. I also help organize meetups (check out PyData Calgary if you're in the area) and volunteer with similar initiatives when I can. A surprising amount of what I know about where the industry is heading, and who to ask when I'm stuck, comes from those conversations.

---

Nobody was calling mom because Google was wrong about the lemons. The lemons were only the (arguably unnecessary 🙂) pretext for the call. The same is true of the Slack ping. Now, at the end of the day, I've started asking myself one question: who did I ask today? Some days the honest answer is nobody. Those are usually the days I've missed something.

This post was written by me; AI assistance was used for selected rewording and editing for verbosity. Views are my own, not my employer's.
{: .disclosure }
