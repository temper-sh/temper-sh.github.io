# Qwen3.8 Flash

**Our assessment:** Completed two extended tool workflows and a Swedish
notice. Other notices from supplied sources needed small factual or wording
repairs.

**Official:** Qwen hosted model supporting text, images and video, with a
one-million-token context. [Model
details](https://docs.qwencloud.com/developer-guides/getting-started/latest-model),
checked 13 September 2026.

Tested through OpenRouter’s Alibaba Cloud route, reasoning enabled,
**131,072-token configured context**. Text only; other modalities unassessed.
Hosted quantization was undisclosed.

## Capabilities

Seven cases were selected after failures by smaller models. They covered
drafting from sources and tool workflows, with AI reviews and simulated tools.
Broader capabilities and acceptance by native speakers were not assessed.

| Capability | Observed result |
|---|---|
| Procurement | Completed the decision and evidence search; identified missing warranty information |
| Concurrent visitor edit | Recovered the conflict, preserved the new instruction and verified the save |
| English notices | Short and long drafts omitted the opening time |
| Spanish notice | Needed the opening time restored |
| Swedish notice | Complete under the AI review against the supplied sources |
| Ukrainian notice | Specific wording and opening-time corrections |

## Performance

Observed through OpenRouter, 12 September 2026. Local fit is unmeasured.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Memory |
|---|---|---:|---:|---:|
| Hosted hardware undisclosed | Undisclosed · Alibaba Cloud route | Not reported | Not reported | Not reported |

The provider did not disclose its inference engine. These records give total
request time, which cannot be separated into prefill and generation rates.

[All models](../README.md).
