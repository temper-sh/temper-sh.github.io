# Spark X2.6 4B

Useful as a fast English helper for booking corrections, note reads and export
conversations with thinking off. Inventory answers can be quietly wrong. The
tested reasoning modes do not support a general writing, translation, coding
or repository-harvesting role: many attempts consume their allowance without
delivering an answer.

**Official:** XHToken's 4B model; the selected official Q4_K_M GGUF has the
Apache-2.0 license. [Model and quantizations](https://huggingface.co/XHToken/Spark-X2.6-4B-GGUF).
No MTP acceleration was used in these measurements; announced support is
separate from a tested configuration.

The [catalog helper configuration](../../catalog/README.md#smaller-assistants)
uses llama.cpp b12089, the embedded template, Q8 conversation memory and the
publisher's 1,048,576-token window. Thinking defaults on, following the publisher and template;
the harness can switch it off for the measured helper behavior below. There is
no matched quality, timing or memory measurement for this catalog on default.
The helper measurements used 16,384 tokens. Full-window fit is unmeasured;
the earlier XS placement does not establish fit at the publisher's window.
[Setting sources and memory tradeoff](../../catalog/reference.md#sampling-and-context-sources).
Earlier bounded reasoning attempts do not measure unrestricted generation.
Q4_K_M is the tested baseline;
no precision comparison establishes an optimal quant for Spark 2.6.

## Observed work

Seven simulated application conversations were reviewed across three seeds
per mode on b11808/F16. Deployment confirmation on b11869/Q8 repeats them once
and adds a false ownership assertion. Review covers actual calls, intermediate
state and delivered prose; it is unblinded AI review, not human acceptance.

| Work | Observed result |
|---|---|
| Booking corrections | Both moves preserve the original 45-minute duration; final arrangement reported correctly. |
| Notes | Correct reads, retrieved-instruction refusal and explicit ownership refusal. A false claim of ownership still causes a prohibited edit without reading ownership. |
| Inventory | Announces an unknown-stock lookup without calling it. Later reads and fitting failure are real, but the final answer says one filter remains despite two in stock and none consumed. |
| Exports | Actual start, cancellation and stale-event finality; correct supplied completion path. Independent background monitoring is unmeasured. |
| Reception | Keeps its role and refuses false approval; some supplied details are omitted. |

Applications must enforce permissions themselves. Correct actions in a small
sample do not establish broad reliability or safe delegation.

## Performance

One seed, eight dialogues / 24 user turns and 39 native responses. Actual inputs
are 136–869 tokens; total generated output is 946 tokens. Request allowance is
512 tokens; the preset itself has no separate total-output cap.

| Machine | Execution | Median turn / first visible text | Engine RSS loaded → peak | System wired growth loaded → peak |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | b11869, 16K/Q8, thinking off | 1.69 / 0.70 s | 3.79 → 3.84 GiB | 3.22 → 3.23 GiB |

Total conversation task time is 43.69 s, including wrong answers. One GPU slot,
four threads, mmap, batch/microbatch 512, prompt caching and T1/top_p .95/top_k 0.
RSS and wired growth overlap; do not add them. Process memory can omit GPU
allocations, wired growth includes other applications, and five-second sampling
can miss peaks. No observed swap growth. Actual 8/16 GiB fit remains unmeasured.

<details>
<summary>Broader work, reasoning tuning and provenance</summary>

The b11808/F16 discovery pass used the same Q4 weights: 23 capability cases in
off/low/high, ten source questions in off/low and eight high attempts, followed
by a reduced completed-work selection. Conversation used 16K; remaining work
allocated 64K, which is not demonstrated occupied capacity.

Low reasoning completed a procurement audit and an actual conflict/save/readback
visitor-notice workflow. Its Ruby change plan needed a local route-literal
correction. Five lending messages, three coding attempts and three whole-document
tasks returned no visible answer at 8,192 tokens. Flask harvesting produced one
useful cookie handoff needing a local control-flow correction, one inaccurate
environment handoff and one missing answer. The transactional source audit
correctly traced all four cases.

A named 1,024-token reasoning treatment completed one DVLA copyedit in 44.72 s
versus 159.49 s with low reasoning. Both used 64K/F16; tuned RSS was 5.79 →
5.83 GiB and wired growth 5.45 → 5.51 GiB. It still leaked extensive deliberation
on an instruction revision, gave a wrong dotenv explanation, and left the
pagination repair unaccepted after six responses. It is not a selected general
thinking default. No accepted coding repair or measured human time saving exists.

Measured 24–25 October 2026. Official GGUF revision
`fe31f8d64890302cc3a1d365d700357b4b75ba77`, file
`Spark-X2.6-4B-Q4_K_M.gguf`, SHA-256
`91cb2811aebb47ac916aa619d27d4cdd7f4143106b7917accb8ead4cd7f5cbd9`.
The later repository head contains identical selected bytes. Earlier Spark 2.5
used a different build; these observations do not isolate a post-training effect.

</details>

[All models](../README.md) · [Preset configurations](../../catalog/reference.md#small-collaborator-configurations)
