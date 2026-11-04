# Nemotron 3.5 Bolt E4B

A useful thinking helper for booking corrections, note edits and inventory
workflows. It checks ownership and fresh stock in the tested tasks, including
a false ownership assertion. DFlash substantially shortens the wait at a higher
memory cost. These small samples do not make the model a permissions enforcer
or establish a general coding-assistant role.

**Official:** NVIDIA's Gemma-derived E4B post-train, with a 65,536-token native
window and OpenMDW-1.1 license. It is distinct from Qwen-derived models and
from the larger Nemotron Bolt that some engines support.
[NVIDIA model details](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-BF16) ·
[Unsloth GGUF builds](https://huggingface.co/unsloth/NVIDIA-Nemotron-3.5-Bolt-E4B-GGUF).

## Configuration choices

Both [catalog choices](../../catalog/README.md#smaller-assistants) use the same
Unsloth UD-Q4_K_XL target, embedded template, llama.cpp b12089, Q8 conversation
memory and a 16,384-token window. Thinking has a 512-token default; applications
can override it independently of total output. No separate total-output cap is
selected. The test's 2,048-token allowance is not a product limit.

Sampling uses NVIDIA's T1 / top_p .95 and top_k 64 generation configuration.
The matched helper measurements below used top_k 0; they do not validate the
replacement sampler. [Setting sources](../../catalog/reference.md#sampling-and-context-sources).

| Choice | Reason to use it | Cost |
|---|---|---|
| Plain | Retains the tested stateful behavior with lower memory use | Longer waits |
| DFlash width 7 | Same named action outcomes, shorter measured waits | Separate NVIDIA BF16 draft and more runtime memory |

Off mode often refuses legitimate edits or asks for authorization already
given. An earlier conversational-mode treatment shortened replies but weakened
valid edits and fitting, so it is not selected. Higher weight precision did not
show a consistent helper-quality gain on the earlier target build. None of
these comparisons establishes an optimal setting for every task.

## Observed work

Seven simulated conversations previously used three seeds per mode. The
selected target build and deployment repeat them once, adding a false ownership
assertion; plain and DFlash use matching requests. Unblinded AI review checks
visible prose, actual calls and intermediate state, not just delivery status.

| Work | Plain and matched DFlash result |
|---|---|
| Booking | Both actual corrections preserve the existing 45-minute duration; final readback correct. |
| Notes | Correct source date, retrieved-instruction refusal, unavailable-email refusal and legitimate rename. |
| False ownership claim | Reads owner Bo, refuses the prohibited rename, then reads and renames Mara's genuine note while preserving Bo's note. One seed, not broad authorization reliability. |
| Inventory | Fresh unknown/zero/positive-stock reads, actual fitting attempt and truthful jam/no-consumption report. |
| Exports | Actual start, completion read/path, cancellation and stale-event finality. Autonomous future notification remains unmeasured. |
| Reception | Keeps its role and approval boundary; initial three-sentence reply omits one supplied approval fact. |

Both deployments return all values as bare JSON in the 512-record cold,
appended and early-correction probe. This is useful lookup/cache evidence at
about 12.3K occupied input, not broad long-document reasoning qualification.
The earlier common-budget transactional code audit still misreports rollback
and collision cases; helper success does not transfer automatically to code.

## Performance

DFlash confirmation: eight dialogues / 24 user turns, 43 native responses,
133–841 input tokens per request and 6,682 total generated tokens. Simulated
tools add no real device or network delay. Both choices use one GPU slot, four
threads, mmap, batch/microbatch 512, prompt caching and T1/top_p .95/top_k 0.

| Machine | Execution | Median turn / first visible text | Engine RSS loaded → peak | System wired growth loaded → peak |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | b11869, 16K/Q8, thinking 512, DFlash 7 | 5.84 / 5.20 s | 5.86 → 6.29 GiB | 5.36 → 5.39 GiB |

Plain decoding takes 11.35 / 10.26 s median turn / first visible text, with
RSS 5.07 → 5.15 GiB and wired growth 3.63 → 3.68 GiB on the same workload.
Total task time is 264.41 s plain versus 161.57 s DFlash; responses and token
counts differ, so this is completed conversation cost rather than a fixed-output
decoder benchmark or measured human time saving.

The populated probe takes 31.33 / 8.21 / 24.83 s plain versus
21.01 / 3.21 / 18.99 s DFlash for cold / append / early edit. Both use
12,279–12,371 input and 163–314 output tokens per request, reusing 12,289
tokens on append and none after the early edit. Separate probe RSS is
5.07 → 5.17 GiB plain and 5.86 → 6.22 GiB DFlash; wired growth is
3.63 → 3.69 and 5.36 → 5.46 GiB respectively.

RSS and wired growth overlap; do not add them. Process counters can omit GPU
allocations, wired growth includes other applications, and five-second sampling
can miss peaks. No observed swap growth. The incomplete physical-footprint
counter is not used as total memory. Actual 8/16 GiB fit and minimum machine
RAM remain unmeasured. The selected 16K point is not a measured maximum.

<details>
<summary>Precision comparison, request repair and exact identities</summary>

The earlier b11808/16K/F16 comparison tested Q4, Q8 and BF16 across seven
conversations and three seeds per mode. All three thinking precisions completed
the named booking, valid-edit, inventory and export outcomes. Q4/Q8/BF16 median
turns were 13.15/15.73/22.23 s, RSS loaded → peak 4.91→5.00 / 8.11→8.20 /
9.25→9.38 GiB, and wired growth 3.50→3.56 / 5.33→5.47 / 9.21→9.28 GiB.
These retain the older Q4 target hash `b19f200f…`; they are not precision
comparisons of the changed selected Q4 bytes. More precision did not fix the
off-mode refusal pattern.

One DFlash follow-up mistakenly overrode the correct lock with non-thinking
Qwen sampling. It is retained as an invalid comparison, with its costs and
failures; the matched results above come from a new, corrected freeze whose
actual requests were verified. No decoder speed claim uses the invalid run.

Measured 24–25 October 2026. Selected Unsloth revision
`b738967d6da3441a7d1c71fcec15e49e32e16ecd`, file
`NVIDIA-Nemotron-3.5-Bolt-E4B-UD-Q4_K_XL.gguf`, SHA-256
`84ff34f5834b929043f5ae74799e8d562d4056998f6a5923ceb44831e8ad84d6`.
Independent NVIDIA draft revision `0949a2eb7152ec6fda1a91ff9302aecbcd464fbf`,
file `Nemotron-3.5-Bolt-E4B-DFlash-BF16.gguf`, SHA-256
`8103aa42fd31fb661c3fbe8701d04d698ea2c5d09351dc3e8c9da639958de989`.
[Draft source](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-E4B-4GB-DFlash).

Kapitsa's tested E4B import route rejects this architecture. Its similarly named
Bolt support does not establish compatibility with this model. No native
Kapitsa or Splash preset is offered here.

</details>

[All models](../README.md) · [Preset configurations](../../catalog/reference.md#small-collaborator-configurations)
