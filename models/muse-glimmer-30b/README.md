# Muse Glimmer 30B

**Our assessment:** Useful for technical explanations, structured drafts, planning
and a second review. DFlash2 speeds generation on the tested M5 / 32 GiB and
supports a demonstrated 131,072-token allocation. Check facts and operational
constraints: database and rollout advice needed substantive corrections, and
both plain and drafted configurations confused values in a dense long directory.

**Official:** Meta's 30B model accepts text and images, with a 131,072-token
native context. Apache 2.0.
[Model details](https://huggingface.co/meta-models/Muse-Glimmer-30B), checked
19 October 2026.

[Choose a Muse preset in Temper](../../catalog/README.md).
Both choices use Unsloth UD-Q4_K_XL, Meta's template, medium reasoning, F16
conversation memory and two context checkpoints. DFlash2 uses a 131,072-token
window; plain decoding retains its tested 57,344-token window.
The DFlash2 choice adds a small model that proposes tokens for Muse to verify;
the plain choice uses less memory. Both run through llama.cpp.

## What it did well, and what needed correction

English tasks were assessed by AI reviewers against fixed briefs. Tools were
simulated; human acceptance, vision and multilingual quality were not measured.

| Work | Observed result |
|---|---|
| Technical explanation | A clear cache analogy, useful tradeoffs, and the correct distinction between a cached copy and the authoritative record. |
| Decision notes | Medium reasoning improved fidelity to the sources; ordering and idempotency advice still needed correction. |
| Database migration plans | Overstated how quickly PostgreSQL could enforce `NOT NULL`; some SQL was invalid. Q5 weights and high reasoning did not repair the errors. |
| Rollout planning | Correctly held back a switch, but invented an acceptable error threshold that contradicted the brief. |
| Conflict recovery | Reread changed state and verified the correct final board in six requests with medium reasoning. |
| Critical review | Found consequential design defects; its proposed repairs still needed an atomicity review. |
| Transport schedule | Both plain and DFlash2 gave correct normal/delayed timings, respected resource constraints and met the word limit. |
| Public-announcement review | Both found every planted error, then added an unsupported publication condition in their replacement announcement. Both still needed correction. |
| Long-record retrieval | Both configurations retrieved five distributed markers and followed a correction. Both returned two wrong values from a denser directory of 1,436 similar records. |

The explanation, database, rollout, conflict and critical-review findings used
llama.cpp b11205 without a draft on 27 September 2026. Explanation and critical
review used low reasoning; planning compared reasoning levels and Q4/Q5 weights.
Those tasks were not rerun with DFlash2. Museum work and the long-record comparison
used the settings below. Broader workflow reliability remains unproven.

## Waiting time and memory

The matched comparison below was measured on 19 October 2026 UTC on Apple M5 /
32 GiB / macOS 26.6.1, with llama.cpp b11741 and llama-swap v265. Both arms used
57,344-token windows and 1024/512 batch/microbatch. DFlash2 uses Inco Q4_K_M
with seven draft tokens per step and F16 draft memory. Service times exclude
model startup. The larger DFlash2 allocations are measured separately below.

| Work | Plain | DFlash2 |
|---|---:|---:|
| Generate 512 tokens from a short prompt, median of two | 64.61 s | 33.54 s |
| Engine RSS loaded → peak for those short runs | 15.78–15.79 → 15.81–15.82 GiB | 17.46–17.47 → 17.70–17.72 GiB |
| Retrieve markers from 49,152 input tokens | 543.81 s | 559.53 s |
| Cached follow-up | 40.40 s | 14.04 s |
| Generate 512 tokens with 49,252 input tokens | 100.79 s | 59.15 s |
| Engine RSS loaded → peak for the context sequence | 15.79 → 16.13 GiB | 17.47 → 18.54 GiB |
| Complete the correct three-trip schedule | 187.83 s | 67.78 s |
| First source review and announcement, both needing correction | 205.34 s | 92.56 s |
| Engine RSS loaded → peak for the two work tasks | 15.79 → 15.83 GiB | 17.47 → 17.86 GiB |

The short diagnostic reduced median elapsed by 48%. It forced 512 greedy tokens
past normal stopping and disabled prompt reuse; it measures generation, not a
finished guide. The populated 512-token check also uses matched prompts and a
fixed output length. Completed-task times include different reasoning and answer
lengths. The first 49,152-token response remains dominated by roughly nine
minutes of input processing; drafting does not remove that wait.
Announcement repair and human acceptance time were not measured; its first
response times are not times to an acceptable announcement.

RSS is sampled engine resident memory, including file-backed weights. It is not
minimum machine RAM. macOS process physical footprint misses much of those
weights, and system wired memory overlaps RSS; these counters must not be added.
No swap growth or thermal/CPU warning was observed. The draft adds 1.65 GB of
downloaded weights; selecting both presets reuses their shared target files.

## Long documents

DFlash2 passed progressively larger allocations through the model's native
131,072-token window on the same M5 / 32 GiB, measured 19–20 October 2026 UTC.
Only the context setting changed. Each point passed a lookup of distributed
records, a cached follow-up asking for previously unqueried records, an early
source correction, a full 4,096-token generation and another lookup of all five
records afterward. All returned values and JSON formats were correct.

| Total window | Initial input | Cold response | Cached follow-up | Early correction | Engine RSS loaded → peak |
|---:|---:|---:|---:|---:|---:|
| 65,536 | 57,344 | 8.78 min | 12.73 s | 10.04 min | 17.57 → 18.68 GiB |
| 98,304 | 90,112 | 17.82 min | 7.71 s | 18.22 min | 17.97 → 19.16 GiB |
| 131,072 | 122,880 | 25.71 min | 29.65 s | 26.81 min | 18.38 → 19.62 GiB |

The window includes output and continuation space. At the largest point, the
full generation took 671 seconds from 122,980 input tokens. The final lookup
passed with 126,545 input tokens in 126 seconds. Maximum observed input plus
output was 127,076 tokens; no extrapolation beyond the native limit was tried.
No swap or thermal/CPU warning occurred. Its short 512-token diagnostic took
33.91 seconds, with 18.39 → 18.64 GiB loaded/peak RSS.

An early correction invalidates the cached prefix and requires another full
input pass. Ordinary append-only follow-ups reuse it: the largest point reused
122,881 tokens and processed 70 new ones. Answers and reasoning lengths differ
across these requests, so their service times are not equal-work speed
comparisons. A short request deadline can stop a healthy long-input run.

The earlier plain configuration passed the same marker, continuation and
correction method at 49,152 input tokens in its 57,344-token window. Its context
sequence generated 512 tokens; the DFlash2 sequence in the matched table also
includes a full 4,096-token generation. Plain decoding was not measured at the
larger allocations.

These checks establish allocation, output room and cache behavior. The forced
output includes reasoning, which is omitted from the next serialized history;
it is not a completed-writing quality assessment. The dense-directory case
still returned the same two wrong values with and without drafting, despite
valid JSON and healthy resources. Check retrieved values against their sources.
Long-document synthesis remains unmeasured.

[All models](../README.md) · [Choose a model](../../guides/choose-a-local-model.md).
