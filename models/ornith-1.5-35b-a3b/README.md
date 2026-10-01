# Ornith 1.5 35B A3B

**Our assessment:** Followed corrections and used supplied evidence in short
tasks. Longer tool workflows did not finish the required final reports.

**Official:** Ornith AI text-and-vision mixture-of-experts model: 35B total
parameters, about 3B active. MIT license. [Model
details](https://huggingface.co/ornith-ai/Ornith-1.5-35B-A3B), checked 13
September 2026.

Tested with Q4_K_M, the canonical template, thinking with a 512-token budget
and a 32,768-token context. Only text was assessed. An earlier attempt lacked
the required template and is recorded separately as a setup failure.

## Capabilities

These structured tasks used simulated tools and fixed requirements for
interactions and output formats. Those constraints limit how the results apply
elsewhere. Human acceptance was not measured.

| Capability | Observed result |
|---|---|
| Following corrections | Retained the corrected instruction |
| Using supplied evidence | Used supplied evidence without inventing missing facts |
| Copyediting with protected identifiers | Repaired OCR while preserving identifiers |
| Procurement | Reached the correct decision twice; ran out of output tokens before a usable final report |
| Recovery from a conflicting edit | Saved changes but did not finish the final report |
| Planning a Ruby change | Partial plans under the test's fixed interaction and final-output requirements |

## Performance

Measured 4 September 2026 UTC.

| Configuration | Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---|---:|---:|---:|
| 32k · thinking 512 | Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | Not recorded | Not recorded | 20.54 GiB |

The nine-attempt run recorded usage and elapsed time, but no separate engine
rates. Memory is sampled resident memory (RSS) of the server process; it does
not establish the machine's RAM requirement.

[All models](../README.md).
