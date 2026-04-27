# Write-Up: Design Rationale for the Daily Reflection Tree

*Gopal Kumar · DT Fellowship Assignment*

---

## Why These Specific Questions
# 🧠 Reflection Tree Design – Write-up

This system is fully deterministic — no LLM is used at runtime. 
All branching logic is predefined in the tree structure.

## 1. Design Approach
...

The hardest constraint in this assignment is the no-free-text rule. Every question must present options that **genuinely capture the spectrum** — if all four choices are clearly "good," the employee picks the aspirational one and learns nothing. If one is obviously "bad," nobody picks it and the tree collapses into a checkbox exercise.

My design goal for every question was: **would a reasonable, honest person on a difficult day choose any of these options without feeling judged?**

### Axis I — Locus

I opened with the weather metaphor because it's observational, not judgmental. "Stormy" doesn't imply weakness — it just names a texture. This lets the employee report their actual experience rather than performing resilience.

The second question is where the psychological work begins. I deliberately separated agency from optimism. "Find one thing I could still control" (internal) and "Push harder on my own" (also internal, but stubborn rather than adaptive) are close enough that an honest person might pick the second thinking they're being hard on themselves. The branching from here routes based on the nature of their agency — proactive vs reactive.

The reflection nodes avoid the word "agency" — that word signals the intended lesson and breaks the experience. Instead they name the *behavior*: "you looked for what you could affect." Rotter's original work (1954) was careful to distinguish internal locus from optimism or self-confidence; I tried to preserve that distinction by never implying that external locus is a character flaw.

### Axis II — Orientation

The Organizational Citizenship Behavior literature (Organ, 1988) is useful here: OCB is specifically *discretionary* — it goes beyond what's required. So the key question isn't "did you do your job" but "did you do anything you weren't asked to?"

The entitlement branch was the trickiest. Campbell et al. (2004) found that psychological entitlement is often invisible to the holder — which means you can't ask "were you entitled today?" directly. Instead I ask about the response to *unmet expectations*: "I got frustrated and it affected my focus" is an entitlement signal; "I let it go and kept moving" is not. Neither option shames the employee — one is just more honest about a pattern.

### Axis III — Radius

Maslow's 1969 paper distinguishes self-actualization (peak personal development) from self-transcendence (orienting beyond the self). Most of us know the first; fewer know Maslow thought the second was the higher stage. Batson's work on perspective-taking (2011) adds the crucial caveat: *thinking* about another person's experience is not the same as *adjusting your behavior* because of it. I tried to surface this distinction in the Axis III questions.

The progression across options is explicitly widening — from "just me" to "a colleague" to "the team" to "the end user." The wide options are not coded as virtuous; they're just a wider frame. Someone at "just me" on a hard day isn't lesser — they're just narrower in the moment, which is true for most people under stress.

---

## How I Designed the Branching

### Trade-off 1: Two-path vs. Four-path splits

Early drafts had the opening question route to four separate paths based on each answer. This created an unmanageably large tree (16+ leaf paths per axis). I collapsed to binary routing (high/low) based on semantic clustering — e.g., "Clear" and "Partly cloudy" both indicate a tolerable day; "Overcast" and "Stormy" indicate a hard one. The follow-up question is different for each group, which creates the branching effect without combinatorial explosion.

### Trade-off 2: Signal accumulation vs. fixed routing

I chose to accumulate signals on most question nodes and use that tally to route at decision points. This means a single "external" answer doesn't trap you in the external path — the tree responds to the *pattern* across multiple answers. The summary node then reads the dominant signal. This is more forgiving and more honest than hard binary routing.

### Trade-off 3: Reflection cadence

I placed one reflection per axis, after the second question (not after the first). The first question establishes context; the second question focuses it; the reflection reframes based on the path taken. Putting the reflection too early felt preachy. Putting it too late (after three questions) lost the thread. Two-question → reflection felt like the right rhythm.

### The axes build on each other

I designed the bridge nodes to explicitly name the previous axis's focus: "you've thought about how you handled today — now let's look at what you *gave*." This draws a line between agency (locus) and contribution (orientation), suggesting they're related but distinct. The final bridge names both ("agency... what you gave") before zooming out to radius, creating a three-step telescoping structure.

---

## Psychological Sources

- **Rotter, J.B. (1954).** *Social Learning and Clinical Psychology.* The original locus of control framing. Important note: Rotter defined it as a spectrum, not a binary — I preserved this by using accumulating signals rather than hard paths.
- **Dweck, C. (2006).** *Mindset.* Informed the framing of "yesterday's constraints" vs. "today's possibilities." Not directly quoted, but present in the logic of the "you can't always catch the choice in real time" reflection.
- **Organ, D.W. (1988).** *Organizational Citizenship Behavior: The Good Soldier Syndrome.* The discretionary nature of OCB shaped the Axis II questions — I asked about behavior beyond the role, not performance within it.
- **Campbell, W.K. et al. (2004).** "Narcissism, Interpersonal Self-Enhancement, and Strategic Self-Presentation." The invisibility of entitlement to its holder shaped the *indirect* framing of the Axis II entitlement questions.
- **Maslow, A.H. (1969).** "Theory Z." The distinction between self-actualization and self-transcendence is the theoretical engine of Axis III. I was surprised to find this paper is underread even by people who know Maslow's hierarchy.
- **Batson, C.D. (2011).** *Altruism in Humans.* The cognitive/behavioral gap in perspective-taking — you can *think about* someone without *adjusting for* them — directly shaped the Axis III questions.

---

## What I'd Improve With More Time

1. **More granular routing on Axis III.** The current tree merges "team" and "end user" into a single "wide" branch. A fuller tree would differentiate: a colleague's perspective (horizontal empathy) is meaningfully different from the end user's (purpose-level transcendence). I'd add a third fork here.

2. **Cross-axis interpolation in reflections.** The summary does this, but the per-axis reflections don't reference what the employee said in earlier axes. A richer implementation would say: "Earlier you said you adapted when things shifted — that same impulse, extended outward, is what contribution looks like."

3. **Streak and trend data.** A single session tells you where you were today. Seven sessions tells you where you trend. With localStorage (or a lightweight backend), the tree could surface: "Last three Tuesdays you've leaned external. Thursdays you're different. Worth noticing?"

4. **Tone calibration by opening answer.** Someone who said "Stormy" needs slightly different holding than someone who said "Partly cloudy." The current tree asks different *questions* based on that answer, but the reflections use the same warmth level. I'd adjust the reflection register — warmer and shorter for hard days.

5. **A "skip today" option.** Some evenings you genuinely can't do this. A graceful exit that logs "hard pass — couldn't show up" is itself a kind of data, and honoring it would build more trust in the tool.
