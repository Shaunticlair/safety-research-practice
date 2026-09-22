# Does showing the working help?

**A 90-minute empirical AI safety exercise**

> **90-minute pilot version.** The experiments and starter have been checked, but we are still calibrating the participant workload. Please tell us how the timing felt: what was comfortable, what felt rushed, and where you got stuck. Submit the short [pilot feedback](PILOT_FEEDBACK.md) afterward.

Language models can sometimes solve problems without displaying intermediate reasoning. Understanding when visible reasoning helps is relevant to proposals for monitoring a model's chain of thought. In this exercise, you will investigate one small part of that question using an open-weight model.

You have **90 minutes**, including reading, experiments, slides, and recording. Your goal is a defensible finding and a useful explanation. There is no expected direction of result.

## Starting point

The starter uses Qwen3-8B and provides an experiment runner and fresh problems with exact answers. Each problem gives a table describing how a potion changes color when ingredients are added. The model must determine its final color after a sequence of additions. Sequence length varies across problems.

You will compare a condition that permits visible reasoning with a condition that requests a direct answer. Both use the same model and matched problems. Inspect what each condition actually produces when interpreting the comparison.

The package includes raw outputs, an exact-answer grader, editable configurations, a task generator, and a cached baseline. You may reuse the baseline after recording your prediction; identify any cached results in your presentation. Your follow-up should include a new experiment.

**You need access to a GPU** for your new experiment: use one H100, or a comparable GPU with a successful smoke test. You can rent one through [Vast.ai](https://vast.ai/pricing), or **message Nabil to ask for an API key and setup instructions for GPU access**. Check the current rental rate and allow for setup/download time as well as the exercise. Complete [setup](SETUP.md), model download, and the smoke test before starting the clock. No model training is required. The checkpoint revision is pinned in the configurations.

## Reading

Spend **about five to ten minutes** on Neel Nanda's [Astra can do a concerning amount with no chain of thought](https://www.lesswrong.com/posts/eRmzz8J8Qkzqvzrgg/astra-can-do-a-concerning-amount-with-no-chain-of-thought). Focus on the description of the no-CoT measurement and the appendix discussing no-CoT reasoning versus controllability. You do not need to read the whole post or reproduce its benchmark.

This exercise asks what happens in the supplied open-model setting; it does not assume the frontier-model result transfers. Additional public papers and code are listed in [SOURCES.md](SOURCES.md), for reference rather than required reading.

## Questions

Record each **A answer before running or viewing results for its B answer**. Keep the original predictions when your view changes. Predictions are assessed on their reasoning, not whether they prove correct.

### Q1 — What difference does visible reasoning make?

**A.** Predict how the same model will perform when **visible reasoning is permitted** versus when **a direct answer without visible working is requested**, including whether the difference will depend on task difficulty. Explain your reasoning and uncertainty.

**B.** Investigate the comparison. What pattern do you find, and how confident should we be in it? Inspect examples as well as aggregate results, and explain any measurement issue that materially affects your conclusion.

### Q2 — What explains your result?

**A.** Choose an explanation for an important result from Q1. Identify a plausible alternative, and propose one tractable experiment that would help distinguish them. State your prediction before running it.

**B.** Run the experiment and update your explanation. What does the evidence support, what remains unresolved, and how—if at all—does it change your view of using visible reasoning for oversight?

Choose your own intervention, controls, and analysis. One well-chosen follow-up is enough. You may modify the supplied setup or introduce a different task if you can justify the comparison within the time limit.

## What to submit

Send **one small ZIP file and a link to an unlisted YouTube video**.

**The ZIP contains:**

- **Reproduction code**, including the scripts/notebooks, configuration, dependency versions, seeds, and brief run instructions needed to reproduce your experiments and graphs. Identify any supplied cached results you reused. Generate or download inputs in the reproduction workflow where possible.
- A completed **[AI_LOGS.md](AI_LOGS.md)** containing your original predictions, actual timing and pilot feedback, and complete logs of all AI assistance. You can put full transcripts directly in that document or reference small native JSON/JSONL exports bundled under `ai_logs/`. Include all relevant sessions, branches, subagents, and available tool traces. These will be reviewed by the organizer, including with AI agents, to understand your workflow and improve the exercise.

You can ask your agent to collect the logs and prepare the ZIP; see [AI_LOGGING.md](AI_LOGGING.md). Check that it includes assistance from other tools or sessions the agent could not access. Copy your original [predictions](PREDICTIONS.md) and [pilot feedback](PILOT_FEEDBACK.md) into `AI_LOGS.md`; no separate report or slide submission is required.

**Keep the ZIP small:** exclude model weights, checkpoints, downloaded datasets, virtual environments, dependency folders, caches, large generated outputs, and the video file. Include only reproduction code/configuration and the small text/log files needed to understand it. Check the archive contents before sending it.

**The unlisted YouTube video:** upload your **1–2 minute screen recording with your spoken explanation**, showing **one or two graphs across the two-slide template**. Explain Q1's prediction and result, then Q2's prediction and result, how your view changed, and the most important uncertainty or limitation. Aim for close to two minutes if needed.

Set visibility to **Unlisted** so anyone with the link can watch, then send the video URL with your ZIP. Check that the link plays in a signed-out or private browser window. [YouTube's visibility instructions](https://support.google.com/youtube/answer/157177?co=GENIE.Platform%3DDesktop&hl=en). Also record the URL in `AI_LOGS.md`.

Use the graphs to explain the evidence in your own words. Short bullet notes are welcome. A screen recording with clear audio is sufficient; a webcam is optional. The assessment concerns your research reasoning and understanding. Presentation polish, editing, and a flawless delivery are not required.

For each question, cover what you expected, what you compared, what you found, and what you now think. You do not need to narrate every run. One graph can cover both questions if it communicates the evidence clearly. A failed or inconclusive experiment can support a strong submission if you diagnose it carefully and calibrate your claims.

AI assistance is allowed for coding, discussion, and analysis. Preserve the complete task-related conversations and tool traces, explain the work yourself, and keep predictions recorded before seeing results. If you use no AI assistance, say so in `AI_LOGS.md`; the reproduction code/configuration is still required.

## Scoring rubric

| Category | Weight |
| --- | --- |
| Communication | 20% |
| Research answers | 70% — 35% each for Q1 and Q2 |
| Steering and judgment | 10% |

### Communication — 20%

Present what you did and found clearly and faithfully. Appearance, editing, and presentation polish are not graded.

- Graphs are easy to interpret, with clear labels and appropriate uncertainty estimates, such as confidence intervals.
- Explain unfamiliar terms and use concise language in your own words. Avoid unnecessary jargon, filler, and boilerplate caveats.
- Claims in the recording agree with, and can be traced to, your reproduction code, figures, and submitted AI logs.

### Research answers — 70%

Q1 and Q2 each receive 35%. Put each question's prediction, finding, and update on your slides, and explain them in the recording. Only answers presented there receive research-answer credit; additional results in the ZIP support verification but do not earn credit on their own. Brief bullets and one or two graphs are enough.

- Predictions recorded before observing results have a reasonable justification. They are graded on reasoning, not whether they turn out to be right. Preserve the original predictions.
- Interpret the observed results and explain how they change your view, including unexpected or inconclusive findings.
- Make testable claims whose scope and confidence match the evidence, avoiding both overclaiming and underclaiming.
- Address relevant confounders, measurement problems, and limitations.
- Run the experiments needed to support the claims you choose to make within the time and hardware available. Causal claims need suitable interventions, not just correlations. One well-chosen follow-up is enough; narrow your claims when further testing is out of scope.
- Implement the experiments and analysis correctly.

### Steering and judgment — 10%

Show your own research judgment, including how you direct and check AI assistance when you use it. Credit comes from choosing informative directions, allocating time and compute sensibly, and noticing and correcting mistakes.

This is not a test of elaborate agent orchestration. Honest confusion and basic questions are welcome. For this category, AI logs provide evidence of helpful interventions; they are not used to penalize you for asking for help. If you use no AI assistance, your own experimental choices and reasoning provide the evidence for this category.

## Suggested pacing

| Minutes | Activity |
| --- | --- |
| 0–20 | Read, understand the task, and record Q1A |
| 20–40 | Run or inspect the baseline and answer Q1B |
| 40–65 | Record Q2A, run a focused follow-up, and interpret it |
| 65–80 | Select one or two graphs and complete the two slides |
| 80–90 | Record 1–2 minutes, check audio/legibility, and package the submission |

Record your start and finish times and actual elapsed time. Stop at 90 minutes and say what is unfinished; if you do run over, report the actual time and what you did afterward. Installation, model download, logging setup, and a short microphone/screen-recording check happen before the clock. Understanding the research problem, making the slides, and recording the final explanation are inside the 90 minutes. You may export logs, package/transfer the ZIP, upload and share the unlisted YouTube video, and complete the brief pilot feedback afterward; report that administrative time separately and do not continue the research or revise the presentation after the timer.
