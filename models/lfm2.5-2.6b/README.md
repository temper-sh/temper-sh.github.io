# LFM2.5 2.6B

**Our assessment:** Hosted tests completed extraction, reasoning and short
drafting. Policy answers needed correction; rate limits left six cases
unavailable.

**Official:** Liquid AI 2.69B hybrid text model. LFM Open License 1.0;
131,072-token context. [Model
details](https://huggingface.co/LiquidAI/LFM2.5-2.6B), checked 13 September
2026.

Tested via Liquid FP8 through OpenRouter, with reasoning requested and a
65,536-token configured context. The unsupported top_p parameter was omitted.
Rate limits prevented assessment of some cases; those are recorded as
unavailable.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues, priorities and conversions |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Handoff summary complete; policy answer invented permission |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Correctly checked all three claims |
| Drafting | Both short English drafts complete |
| Editing | Changed a supplied quantity |
| Translation into Swedish | Substantial terminology and grammar repairs |
| Following updated instructions | Unavailable: HTTP 429 |
| Tool use | Direct read worked; current-record search interrupted by HTTP 429 |
| Planning and execution | Valid plan; release execution unavailable |
| Recovery | Initial read, then HTTP 429 |
| Saving and reporting results | Initial read, then HTTP 429 |

## Performance

Observed through OpenRouter, 12 September 2026. Local fit is unmeasured.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Memory |
|---|---|---:|---:|---:|
| Hosted hardware undisclosed | Undisclosed · Liquid FP8 route | Not reported | Not reported | Not reported |

The provider did not disclose its inference engine. These records give total
request time, which cannot be separated into prefill and generation rates.

[All models](../README.md).
