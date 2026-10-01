# Ministral 3 3B

**Our assessment:** Hosted tests completed extraction, copyediting and simple
saved changes. Conflicting edits and writing from sources still needed
intervention.

**Official:** Mistral model with a 3.4B language component and 0.4B vision
encoder. Apache 2.0; 256k context. [Model
details](https://huggingface.co/mistralai/Ministral-3-3B-Instruct-2512),
checked 13 September 2026.

Tested via Mistral through OpenRouter; quantization and server template
undisclosed, 131,072-token configured context. Text only; vision unassessed.

## Capabilities

AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Observed result |
|---|---|
| Extraction | Correct fields and missing values |
| Classification | Correct queues, priorities and conversions |
| Reconciliation | Preserved time while changing rooms |
| Comprehension and synthesis | Handoff summary complete; policy explanation needed repair |
| Reasoning | Correct constrained scheduling choice |
| Evidence assessment | Added unsupported proof requirements; confused absence with contradiction |
| Drafting | Allowed a closed collection time; invented a calendar claim |
| Editing | Completed the constrained copyedit |
| Translation into Swedish | Substantial wording and access-direction errors |
| Following updated instructions | Returned the latest arrangement |
| Tool use | Found and read the current record |
| Planning and execution | Invented prior checks; published without reading the final saved result |
| Recovery | Stopped at the conflict and called stale content current |
| Saving and reporting results | Saved, reread and validated the simple change |

## Performance

Observed through OpenRouter, 12 September 2026. Local fit is unmeasured.

| Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Memory |
|---|---|---:|---:|---:|
| Hosted hardware undisclosed | Undisclosed · Mistral route | Not reported | Not reported | Not reported |

The provider did not disclose its inference engine. These records give total
request time, which cannot be separated into prefill and generation rates.

[All models](../README.md).
