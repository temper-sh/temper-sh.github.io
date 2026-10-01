# Choose a local model for the work

Start with what you want to do and how much memory your Mac has.
[Temper's setup](https://github.com/temper-sh/temper#get-started) offers curated
presets: weights chosen for quality, an engine suited to the model and hardware,
and tuned settings. You do not need to assemble those parts yourself.

## Pick a starting point

| What you want to do | Starting point | Main limitation |
|---|---|---|
| Write scenes, revise dialogue and get explanations | [Gemma 4 26B-A4B](../models/gemma-4-26b-a4b/README.md) | Check facts, length and continuity. |
| Try another model for creative writing | [Gemma 4 31B](../models/gemma-4-31b/README.md) | Much longer waits on the measured Mac; no demonstrated human preference advantage over 26B. |
| Draft technical explanations and plans, or get a second review | [Muse Glimmer 30B](../models/muse-glimmer-30b/README.md) | Database details and operational guarantees still need checking. |
| Combine sources and revise documents | [Qwen3.8 27B](../models/qwen3.8-27b/README.md) | Long inputs can take minutes to process; strict formatting and Swedish prose were uneven. |
| Propose bug fixes and code changes | [Qwen3.8 27B with Splash](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl.md) | Faster first patches in two tests, but both needed review and corrections. |
| Extract fields from short records | [Gemma E2B](../models/gemma-4-e2b/README.md) | Missing evidence and unfinished tool actions. |
| Draft short messages from supplied facts | [Gemma E4B](../models/gemma-4-e4b/README.md) | Edits can change quantities or retain false claims. |
| Classify records and handle short tool workflows | [Qwen3.5 4B](../models/qwen3.5-4b/README.md) | Longer writing, editing and changed instructions. |

These are starting points for specific jobs. The cards explain what was tested
and what failed. The [model index](../models/README.md) includes other assessed
models; [Temper's presets](https://github.com/temper-sh/temper/blob/master/catalog/README.md)
show what you can install today. A historical test may use different settings
or an older engine than the current preset.

For Gemma's writing role, 26B is the practical starting point on the measured
M5 with 32 GiB. Its tested scene and revision took 16 and 11.3 seconds, compared
with 94.2 and 71.1 seconds for 31B. Both needed factual and continuity edits.
The cards retain the configurations and review limits behind that comparison.

## Match the configuration to your machine

A model shares memory with its conversation history, macOS and your other
applications. Temper shows an estimate before downloading; leave room for the
work you plan to keep open.

Most measurements here use an M5 with 32 GiB. The index's smaller memory tiers
are estimates; fit on actual 8 or 16 GiB machines remains unmeasured.

Long documents and conversations need more memory and take longer to process.
A model publisher's maximum context does not establish a usable size on your
Mac. The cards report tested sizes and waiting times.

A **layout** lets you keep a setup for a particular use: writing, coding, or
small local helpers alongside a cloud assistant. It groups your presets and
chooses which model an application uses by default. Included models can load
when needed; they do not all need to stay in memory.

## Try a representative job

Start with something you can check: a familiar document, a small code change or
a draft from known facts. Give the model the source and say what must stay
unchanged.

Check the finished result, including saved files and actions the model claims
to have taken. Compare the total time, including your corrections. A smaller
model is useful when it does the job with less waiting and repair.

## Use a local model beside another assistant

A local helper can handle a small, defined step while your main assistant stays
in the cloud. For example, it might extract facts from documents before the
main assistant drafts from them. Keep the source excerpts so you can check
what was lost or assigned to the wrong record.

This is a workflow pattern to evaluate, not an automatic Temper integration.
Connecting the helper, passing context and checking its output are still part
of the job. Running steps in sequence can save memory, but loading models again
adds time and loses cached conversation state. Sending the extracted facts to
a cloud assistant also sends that information outside your machine.

The [approach guide](our-approach.md#improve-the-harness-as-well-as-the-model)
explains how tools and instructions affect the result.
[Context and caching](../models/qwen3.8-27b/tuning.md) explains why follow-up
messages can be faster than the first response.

[All guides](README.md) · [Model index](../models/README.md) ·
[Back to Temper](../README.md).
