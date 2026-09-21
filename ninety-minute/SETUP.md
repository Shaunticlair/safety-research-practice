# Setup and running experiments

Run commands from this folder. The GPU path was tested on Linux, Python 3.11, one H100 PCIe 80 GB, with `Qwen/Qwen3-8B` revision `b968826d9c46dd6066d109eabc6255188de91218`. A100 operation has not been measured. CPU-only preparation, grading tests, and cached analysis also work on a laptop. Fresh installation is separate from the timed exercise.

## Before the clock

On the GPU machine, create an environment and install dependencies:

```bash
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m unittest -v
python starter.py run --config configs/smoke.json --output runs/smoke.jsonl
```

The smoke run downloads/loads the pinned checkpoint and uses disjoint easy cases. Allow time and disk space for model/dependency downloads. Do not inspect the baseline cache before writing your Q1 prediction. Resolve installation or GPU problems before starting the 90-minute timer; an organizer should help with setup rather than count debugging against you.

A repeat run needs a **new output filename**: the runner refuses to overwrite or combine experiments. The model is loaded per invocation; this overhead is included in the measured command runtimes. No serving process needs to be left running.

For CPU-only checks and cached analysis, only Matplotlib and NumPy are needed for the plot; the table/JSON analysis and tests use Python's standard library. You still need an available GPU environment for your new model experiment.

Also open [the blank slides](slides_template/slides.html) and make a short test recording to check screen capture and microphone audio before the clock. Read [the slide instructions](slides_template/README.md) for the dependency-free build workflow.

Before starting, read [AI_LOGGING.md](AI_LOGGING.md), use fresh task-specific AI sessions, and check that you can preserve/export their histories. Record task-related AI use during setup as well. Logging setup is outside the timer; AI-assisted work on the research questions is inside it.

## During the exercise

Record your start time so you can report actual elapsed time in the pilot feedback.

After recording Q1A, either run the baseline:

```bash
python starter.py run --config configs/baseline.json --output runs/baseline.jsonl
python starter.py analyze runs/baseline.jsonl --output-dir analysis/baseline
```

or analyze the supplied baseline:

```bash
python starter.py analyze cache/baseline.jsonl --output-dir analysis/baseline
```

The analysis writes `SUMMARY.md`, `summary.json`, and, if Matplotlib is installed, `accuracy.png`. The JSON contains counts by difficulty and an exploratory paired bootstrap interval. All attempted problems remain in the accuracy denominator. A missing/invalid final answer or token-limit truncation counts as unsuccessful; extra working in the direct arm is reported separately from answer accuracy. Inspect raw outputs before trusting a summary.

```bash
python starter.py inspect cache/baseline.jsonl --arm direct --limit 3
python starter.py inspect cache/baseline.jsonl --item-id s2026092017-p0
```

Replace the cache path with your output path if you ran your own baseline.

## Implementing your follow-up

Copy `configs/baseline.json` to `configs/followup.json` and edit it. Keep the task seed, colors, levels, and sample count fixed if you want matched problems; give a changed condition a **new arm name**. You can run only the new arm and compare it with the saved baseline:

```bash
python starter.py run --config configs/followup.json --output runs/followup.jsonl
python starter.py analyze runs/followup.jsonl --reference cache/baseline.jsonl --output-dir analysis/followup
```

The combined analysis requires unique request IDs. If you include unchanged baseline arm names in the new run, analyze that complete run without `--reference`. Automatic paired comparisons use the arm named `direct`; a changed task seed does not create a paired experiment and needs an appropriate separate analysis.

Configuration fields:

| Field | Meaning |
| --- | --- |
| `task_seed`, `levels`, `per_level`, `colors` | Fresh problem generation and difficulty; levels must be positive and smaller than the color count |
| Arm `name` | Unique condition label |
| Arm `thinking` | Qwen's chat-template thinking switch; the default instruction also changes |
| Arm `instruction` | Optional complete replacement of the response instruction |
| Arm `max_tokens`, `temperature`, `top_p`, `top_k`, `seed` | Generation settings; defaults are visible in `starter.py` |
| Arm `format` | `prose` (default) or `table`, preserving transitions and the query |
| Arm `filler_dots` | Number of spaced dots appended after the question; this is not a guaranteed tokenizer count |

You can edit `make_messages` or other code for a different intervention. Retain the final `Answer: COLOR` schema unless you also update and validate the grader. `prepare` writes requests without GPU calls if you want to inspect a configuration first:

```bash
python starter.py prepare --config configs/followup.json --output runs/followup-requests.jsonl
```

Each model result retains the actual prompt, response, stop reason, token counts, and exact grading result. Metadata records configuration, versions, revision, timings, and source/request hashes; an adjacent source snapshot records the script used. The model receives only the messages, not the stored gold answer.

This is an exploratory experiment. Intervals over sampled problems do not capture every uncertainty about prompts, model seeds, or new task distributions. Decide what analysis your explanation needs.

## Finishing

Stop the research and presentation work at 90 minutes and record your actual finish time. Prepare one small ZIP containing reproduction code/configuration and a completed [AI_LOGS.md](AI_LOGS.md), with bundled native AI-log exports if needed. Copy original predictions and pilot timing feedback into that document. Exclude model weights, checkpoints, environments, caches, downloaded datasets, large generated outputs, and the video file.

Upload the 1–2 minute recording to YouTube as **Unlisted**, check that anyone with the link can watch, and send its URL alongside the ZIP. Record the URL in `AI_LOGS.md` too. The [README](README.md) explains the final submission requirements.

Administrative export, upload, sharing, transfer, and feedback can happen after the timer; record that time separately. Keep local research outputs for your own records. Download and verify needed files before terminating a rented GPU; merely closing SSH does not stop billing.
