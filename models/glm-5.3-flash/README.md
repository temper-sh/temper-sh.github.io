# GLM 5.3 Flash

**Our assessment:** Completed two extended tool workflows. All five notices
from supplied sources needed specific corrections to timing or wording, or
removal of added advice.

**Official:** Z.ai multimodal mixture-of-experts model with 320B total
parameters and 18B active. MIT license. [Model
details](https://huggingface.co/zai-org/GLM-5.3-Flash), checked 13 September
2026.

Tested through OpenRouter’s Z.ai FP8 route, reasoning enabled, **131,072-token
configured context**. Text only; other modalities unassessed. Reasoning effort
was high.

## Capabilities

Seven cases were selected after failures by smaller models. They covered
drafting from sources and tool workflows, with AI reviews and simulated tools.
Broader capabilities and acceptance by native speakers were not assessed.

| Capability | Observed result |
|---|---|
| Procurement | Completed the decision and evidence search; identified missing warranty information |
| Concurrent visitor edit | Recovered the conflict, preserved the new instruction and verified the save |
| English notices | Opening time omitted; longer draft added out-of-scope booking advice |
| Spanish notice | Needed the opening time restored |
| Swedish notice | Opening-time and deposit wording repairs |
| Ukrainian notice | Late-fee timing and phrasing repairs |

## Performance

Observed through OpenRouter, 12 September 2026. Local fit is unmeasured.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Memory |
|---|---|---:|---:|---:|
| Hosted hardware undisclosed | Undisclosed · Z.ai FP8 route | Not reported | Not reported | Not reported |

The provider did not disclose its inference engine. These records give total
request time, which cannot be separated into prefill and generation rates.

[All models](../README.md).
