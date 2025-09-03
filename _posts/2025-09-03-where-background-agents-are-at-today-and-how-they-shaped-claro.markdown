---
layout: post
title: "Where Background Agents Are At Today (And How They Shaped Claro)"
date: 2025-09-03 10:00:00 -0400
categories: claro ai cursor productivity
---

When I first started experimenting with background agents in Cursor, I’ll admit: I wasn’t sure what to expect. The idea was ambitious: let an AI quietly work in the background, tackling tickets, cleaning up code, maybe even handling the chores that developers usually push off until “later.” For a long time, "later" meant _never_. Background agents promised a world where those small but necessary changes actually happened without me lifting a finger.

I’ve been leaning on Cursor pretty heavily while building [Claro](https://clarocal.com), my new task and calendar management app. Claro itself is an attempt to rethink scheduling: lightweight, flexible, and native-friendly. But in practice, most of my energy over the past 48 hours hasn’t gone into building shiny new features. It’s gone into seeing just how far background agents can stretch.

---

#### The Early Days: Rough Around the Edges

When background agents first launched a few weeks (or was it months?) back, I wanted them to do the kinds of things I usually procrastinate on: removing debug logging, enforcing consistency across files, or patching up stray TODOs. In theory, perfect use cases. In reality, the first wave of agents struggled. They could follow simple commands, but as soon as the request got layered or touched multiple parts of the repo, things went sideways.

I remember one of my earliest attempts: “remove all debug logging unless we’re in development mode.” The agent gamely opened a PR . . . But it was riddled with inconsistencies and missed half the cases. Worse, when I tried to merge other changes into main, the agent’s PR immediately was outdated. It didn’t know how to rebase against my recent work, so the result was a PR I couldn’t use. The net time saved? Negative.

Back then, the failure rate was high enough that I stopped bothering. I’d spend more time cleaning up their mess than just doing it myself.

---

#### The GPT-5 Shift

Fast forward to the last couple of days, and it feels like we’ve crossed a line. The background agents now run on GPT-5, and it shows. They can actually follow a detailed prompt without spiraling off into nonsense. Instead of coming back with a barely related PR, they now seem to “get it” in a way that’s closer to a junior developer who actually reads your instructions.

That same “remove all debug logging” request? The new agent handled it cleanly, scoped the changes, and explained what it had done. That’s a leap forward. The precision makes them feel less like a demo feature and more like a real tool I might trust.

But it’s not perfect. They’re still brittle around changes in the codebase. If I’ve merged a handful of PRs since they started their run, chances are I’ll end up tossing their work because it no longer applies cleanly. This feels like the biggest limiter right now: they operate in a snapshot of time, but my repo is alive. Unless they can adapt and rebase, a lot of the value gets stranded.

---

#### The Cost of Delegation

One thing I’ve noticed is that using agents is cheap in _money_ but expensive in _trust_. In the past 48 hours, I’ve run half a dozen background jobs, and I’m probably out less than ten bucks. That’s nothing compared to the hours of engineering time it _should_ have saved. But in practice, I still find myself babysitting their output.

This is the catch: an agent that creates PRs I can’t merge isn’t just wasted money, it’s wasted mental energy. I need to review their work, test it, and often redo it by hand. When that happens, I end up feeling like I paid to be distracted.

That said, when they _do_ land a clean PR, the ROI is massive. It feels like magic to wake up, check the repo, and see chores completed while I slept.

---

#### How Claro Benefited

Claro has already benefitted in small but meaningful ways. For example, I had background agents clean up type annotations across the codebase, enforce consistent error handling, and even refactor a gnarly function into something modular. None of those were glamorous tasks, but together they smoothed out the developer experience.

I like to think of it as the AI doing janitorial work. Not glamorous, not headline-worthy, but it makes the house feel clean. And for a new project like Claro, that’s actually huge. A tidy codebase compounds - you move faster, onboard easier, and make fewer mistakes.

---

#### What’s Still Missing

The glaring gap today is context continuity. Agents don’t really understand what’s happened in the repo since they started working. They can’t rebase intelligently. Until that changes, they’ll remain more like eager interns than autonomous collaborators. You have to supervise them closely and discard work when it gets stale.

Another missing piece is multi-step strategy. An agent can execute a single command beautifully, but ask it to stage a plan with dependencies, and it struggles. Humans think in sequences (“first remove logging, then update configs, then adjust docs”), but agents right now are one-shotters.

If Cursor cracks those two challenges (continuous context and multi-step planning) I could imagine background agents becoming indispensable.

---

#### Where This Is Going

For now, I see background agents as a glimpse of the near future, not the end state. They’re useful in bursts, and they hint at what’s coming. But if you’re expecting them to run like autopilot, you’ll be disappointed.

That said, I can’t shake how different my experience has been compared to just a few months ago. With GPT-5 agents, they’ve gone from frustrating to genuinely helpful. And I suspect six months from now, we’ll look back on this current state as just another stepping stone.

Claro is my testbed for all this. As I push forward on the app, I plan to keep experimenting with agents, not because they’re perfect, but because they’re improving at a pace I’ve rarely seen in any tool. I want Claro’s codebase to be a living proof of what’s possible when humans and AI collaborate. Not as hype, but as daily reality.

---

### Closing Thoughts

Background agents today are like interns who show up early, work hard, and sometimes deliver gold. You wouldn’t leave them in charge of the repo just yet. Still, even that level of help can make a tangible difference when you’re building something from scratch.

If you’re a developer curious about AI in your workflow, background agents are worth exploring. Just don’t expect autopilot. Think of them as assistants who are finally getting good enough to trust with real tasks and who might grow into something more.

And if you’re curious about Claro, well, stay tuned. It’s the app that’s teaching me as much about AI development as I am teaching it about calendars. Learn more at [clarocal.com](https://clarocal.com).
