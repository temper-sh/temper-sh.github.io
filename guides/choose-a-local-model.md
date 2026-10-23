# Models, tasks and hardware

Start with the work you want to do, the resources you can spare and the application
you want to use. [Temper's presets](../catalog/README.md) prepare particular
combinations of weights, engines and settings. Their pages explain those choices
and report observations from limited tests.

If the terms are unfamiliar, [Making sense of local AI](basics.md) explains
the pieces.

## Choose a general assistant

A general assistant takes conversational instructions and may use tools supplied
by its application. The records below cover different kinds of work; they are
examples to inspect, not a ranking of the models.

| Work to explore | Where there are observations | What to look for |
|---|---|---|
| Conversation, scenes and revision | [Gemma 26B](../models/gemma-4-26b-a4b/README.md) and [31B](../models/gemma-4-31b/README.md) | Preserved facts and continuity, response length, waiting time and memory. |
| Technical drafts and explanations | [Muse Glimmer](../models/muse-glimmer-30b/README.md) | Source fidelity, operational details and unfinished work. |
| Code changes on a 32 GiB Mac | [Qwen / Kapitsa](../models/qwen3.8-27b/layouts/kapitsa-mlx4.md), our recommendation measured on M5 | Direct setup only; Splash is the installable alternative. Compare checked completion time, loaded/peak memory and large-input tradeoffs. |
| Combining sources and editing documents | [Qwen3.8 27B](../models/qwen3.8-27b/README.md) | Format errors, source corrections, actual input size and cache behavior. |
| Short records and messages with a smaller model | [Gemma E2B](../models/gemma-4-e2b/README.md), [E4B](../models/gemma-4-e4b/README.md) and [Qwen3.5 4B](../models/qwen3.5-4b/README.md) | Changed facts, missing fields and whether tool actions actually finished. |

The [model index](../models/README.md) includes other assessed configurations.
An observation applies to its named engine, weights and settings; the current
preset may differ.

## Choose a specialist for a pipeline step

A specialist has a narrower input/output contract: text or an image to structured
records, query and passages to ranking scores, or text to a translation.
The [specialist index](../models/specialists/README.md) groups these by task.

Check supported inputs, language coverage, license, interface and resource costs.
Publisher coverage and local test coverage are separate. A successful component
check does not establish the quality of a complete workflow using it.

## Match the configuration to your machine

Consider how much memory can remain available while your other applications are
open. A small model can be a main assistant or a helper on any machine where it
fits. Longer documents and conversations add working-memory and processing costs.

Most local measurements use an M5 with 32 GiB; selected Splash trials use an
M5 Pro with 64 GiB. Smaller-machine placements are estimates. Actual 8 or 16 GiB
fit remains unmeasured.

Look for the measured workload, elapsed time and named memory counter together.
A publisher's maximum context and a preset's configured window are different
from a demonstrated workload. Process memory excludes some system costs.

A **layout** groups your presets and chooses an application's default model.
Included models can load on demand; they do not all need to remain in memory.
Temper shows an estimate before preparing the selected setup.

## Try a representative job

Use work you can check: a familiar document, a small code change or a draft from
known facts. Check saved files and actions as well as the final response.
Include your own review and repair time when deciding whether the setup helps.

Our suite supplies observations to investigate. It does not predict reliability
for your tasks. [The assessment guide](our-approach.md) explains the checks and
their limits.

## Use a local model beside another assistant

A local helper can extract fields or select passages for another local or cloud
assistant. Keep source excerpts available so lost or misassigned information can
be checked. Passing extracted information to a cloud assistant sends that
information outside the machine.

This is a workflow pattern to evaluate. Connecting components, passing context,
checking results and handling errors are part of the work. Sequential model
loading can reduce simultaneous memory use, but adds loading time and may lose
cached context.

[Explore presets](../catalog/README.md) · [All guides](README.md) ·
[Tools and resources](../tools.md).
