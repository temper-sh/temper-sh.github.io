# Muse Glimmer 30B

**Our assessment:** Useful for technical explanations, structured drafts, planning,
and a second review. Database and rollout advice still needed substantive
corrections, even with higher precision or more reasoning. It completed one
simulated workflow involving a conflicting edit; broader reliability is unproven.

**Official:** Meta's dense 30B model accepts text and images, with a
131,072-token native context. Apache 2.0.
[Model details](https://huggingface.co/meta-models/Muse-Glimmer-30B), checked
27 September 2026.

[Temper offers a Glimmer preset](https://github.com/temper-sh/temper/blob/master/catalog/README.md).
The measured configuration used Unsloth UD-Q4_K_XL, Meta's template,
llama.cpp b11205, medium reasoning, F16 conversation memory, two context
checkpoints, and a 57,344-token window. It used no draft model.

## What it did well, and what needed correction

English tasks were assessed by AI reviewers against fixed briefs. Tools were
simulated; human acceptance, vision, and multilingual quality were not measured.

| Work | Observed result |
|---|---|
| Technical explanation | A clear cache analogy, useful tradeoffs, and the correct distinction between a cached copy and the authoritative record. |
| Decision notes | Medium reasoning improved fidelity to the sources; ordering and idempotency advice still needed correction. |
| Database migration plans | Overstated how quickly PostgreSQL could enforce `NOT NULL`; some SQL was invalid. Q5 weights and high reasoning did not repair the errors. |
| Rollout planning | Correctly held back a switch, but invented an acceptable error threshold that contradicted the brief. |
| Conflict recovery | Reread changed state and verified the correct final board in six requests with medium reasoning. |
| Critical review | Found consequential design defects; its proposed repairs still needed an atomicity review. |

Explanation and critical-review tasks used low reasoning. Planning compared
several reasoning levels and Q4/Q5 weights. These results do not mean every
task was repeated with the final settings. Earlier b10655 tests with a
1,024-token budget left longer workflows unfinished and selected files outside
the requested Ruby change.

## Waiting time and memory

Measured 27 September 2026 on Apple M5 / 32 GiB / macOS 26.6.1, with
llama.cpp b11205 and llama-swap v260.

| Work / configuration | Time | Rate or memory |
|---|---:|---|
| Generate 512 tokens, medium / F16 / 57,344 window | 62.01 and 66.73 s | 8.37 and 7.78 tokens/s |
| Retrieve facts from 49,152 input tokens | 582.3 s | Input processing: 89.41 tokens/s; peak engine RSS: 16.07 GiB |
| Follow-up using the same context | 7.4 s | Reused 49,153 tokens |

The 512-token check used greedy sampling, disabled prompt reuse, and forced
generation past normal stopping. It measures output speed, not writing quality.

A separate 32k/low-reasoning comparison found less than a 10% elapsed-time
improvement from a short DFlash2 draft, at the cost of another 1.65 GB of weights.
Larger draft proposals were slower. High reasoning took 483.7 seconds on the
migration task and still made critical errors.

## Long documents

The 57,344-token setting retrieved distributed facts and answered a follow-up
correctly, with 4,096 tokens reserved for output. Both responses were bare JSON.
No new swap or thermal warning was observed.

The first response nearly reached the ten-minute limit. A smaller 49,152-token
window handled 40,960 input tokens in 419.7 seconds, with a 25.0-second
follow-up; offline validation briefly overlapped that test. A larger
65,536-token window timed out while memory remained around 16 GiB.

These are retrieval and continuation results. The output reserve was not fully
generated; long-document reasoning and minimum machine RAM remain unmeasured.

[All models](../README.md) · [Choose a model](../../guides/choose-a-local-model.md).
