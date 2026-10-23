# Which draft method helps Gemma 31B on an M5?

**Keep MTP at draft width 3 for the tested Gemma 31B QAT configuration.**
At draft width 7, original DFlash and Red Hat's DSpark accelerated plain
decoding on short work, but MTP was faster and used less memory. DFlash2 Q4 was slower than plain;
its F16 counterpart failed before answering. All completed configurations
returned identical text, including the same formatting and length defects.
This smoke gave us no reason to spend hours tuning these alternative drafts
for this machine and target.

**Claim reproduction**, measured 20 October 2026. We tested upstream
speculative-decoding techniques under local conditions, rather than repeating
their published GPU benchmarks. Speculative decoding proposes several tokens
for the main model to verify together; draft width limits how many are proposed
per step. MTP uses Gemma's prediction head;
the DFlash family uses separate draft models. Useful speed depends on both
accepted proposals and the cost of proposing and verifying them.

## Matched short work

Apple M5 MacBook Air, 32 GiB RAM, macOS 26.6.1 (25G76), llama.cpp
**b11756 / `d80619397c2ba5cf1366c29f7d43699ca3a159b9`**, llama-swap v265.
The target was Unsloth Gemma 4 31B IT QAT UD-Q4_K_XL. Every configuration
used Metal, mmap, four CPU threads, batch and microbatch 512, Q8 target KV
cache and F16 draft KV cache where applicable. The existing 24 GiB Metal wired
limit was unchanged. One model configuration was resident at a time.

The context allocation was **16,384 tokens**, but actual inputs were only
79, 81 and 167 tokens. Thinking was off, sampling greedy, seed 17, and prompt
reuse disabled. The same rendered prompts asked for:

- A JSON lookup of three supplied facts, including an unknown date: 37 output
  tokens. Every completed configuration preserved the facts but added an
  unwanted Markdown code fence.
- An editorial corrections guide, forced to 256 output tokens with normal
  stopping disabled. This measures fixed generation work, not a finished guide.
- An 80–120-word scheduling handoff: 104 output tokens. Every configuration
  correctly calculated recording end at 10:00, upload end at 10:12 and a missed
  10:10 deadline, allowed unattended upload, and preserved ungranted approval.
  All returned the same **64 words**, failing the requested length.

These observations establish narrow output agreement, not broad writing
quality or human preference. Long conversations and thinking-on work were
not tested.

## Time and memory

Times include input processing and generation, excluding model startup.
Model files had been read for identity checks before inference. The two
comparisons used the same controls: plain and DFlash2 were measured with
MTP at 19.55 seconds; original DFlash and DSpark with a fresh MTP run at
19.10 seconds. Ranges below span those two MTP observations. Every other
configuration had one run.

| Configuration / draft width | Fixed 256 tokens, seconds | Handoff, seconds | Engine RSS loaded → peak, GiB | Fixed-output draft acceptance |
|---|---:|---:|---:|---:|
| Plain | 37.85 | 17.23 | 17.60 → 17.75 | — |
| MTP Q4_0 / 3 | **19.10–19.55** | **8.37–8.52** | **17.95–17.96 → 18.12–18.13** | 54.14% |
| Original DFlash Q8_0 / 7 | 28.08 | 8.61 | 19.33 → 19.67 | 15.51% |
| Original DFlash Q8_0 / 15 | 54.70 | 15.61 | 19.32 → 19.72 | 6.39% |
| Red Hat DFlash2 Q4_K_M / 7 | 41.09 | 18.22 | 19.41 → 19.72 | 6.38% |
| Red Hat DSpark Q4_K_M / 7 | 24.51 | 8.91 | 20.21 → 20.54 | 21.56% |
| DSpark, metadata diagnostic / 7 | 24.01 | 8.82 | 20.21 → 20.55 | 23.04% |

Original DFlash at width 7 took **47% longer** than its MTP control on fixed
output; the best observed DSpark time took **26% longer**. DFlash2 took about
**2.1 times** its MTP control's time. Small handoff differences and the two
DSpark timings are not reliably ranked by one observation each.

Acceptance means accepted proposed tokens divided by generated proposals.
Widening original DFlash from 7 to 15 generated 1,925 proposals instead of 851,
while accepted tokens fell from 132 to 123. More speculation cost more time
without advancing the answer further. Drafting and verification costs were
not profiled separately.

Loaded RSS was sampled after readiness; peak RSS covers startup and the three
requests, sampled every two seconds. Sampling can miss brief peaks. No attempt
added swap or showed a thermal or CPU-performance warning. RSS misses some
Metal allocations; process physical footprint omits much mmap weight residency.
Neither establishes minimum machine RAM. RSS, physical footprint and system
wired growth overlap and must not be added together.

## A ready server can still fail inference

The DFlash2 F16 attempt was meant to check whether Q4 quantization explained
poor acceptance. Metal reported insufficient memory during warmup, but the
server nevertheless became HTTP-ready. Its first request then failed while
processing the speculative batch. No valid answer or latency measurement
exists for that configuration, so the precision hypothesis remains unresolved.

F16 engine RSS was **22.56 GiB after readiness and 22.65 GiB at peak**;
system wired growth was **23.92 GiB** under the unchanged 24 GiB wired limit.
That post-ready sample describes an already unhealthy runtime. The failure
shows why a readiness response and apparent RSS headroom cannot establish
usable Metal inference; it does not establish a physical-RAM ceiling.

## Conversion assumptions needed checking

The original DFlash GGUF omitted Gemma embedding-scale metadata. We initially
treated this as a likely conversion defect because the
[llama.cpp converter](https://github.com/ggml-org/llama.cpp/blob/d80619397c2ba5cf1366c29f7d43699ca3a159b9/conversion/qwen.py)
infers Gemma's square-root embedding scale. Checking
[z-lab's reference computation](https://github.com/z-lab/dflash/blob/07ebd93db9f472af339b644bb70221ad8428328a/dflash/model.py)
changed that diagnosis: it uses raw embeddings with a default scale of 1.
The prepared higher-scale copy was **never run**; both measured DFlash
configurations used the unchanged GGUF. Missing metadata was not proof of
a broken conversion. The relevant contract is the drafter's computation,
not an assumption inherited from the target architecture.

For DSpark, inspection of the
[Speculators backbone](https://github.com/vllm-project/speculators/blob/8747d72a41343824eafba38a351ed4ae2697c376/src/speculators/models/dflash/core.py)
and [attention mask](https://github.com/vllm-project/speculators/blob/8747d72a41343824eafba38a351ed4ae2697c376/src/speculators/models/dflash/attention.py)
motivated a bounded diagnostic: set embedding scale to 1 and enable causal
draft attention for this all-sliding-attention checkpoint. All tensor bytes
remained identical. The two metadata changes together moved fixed time from
24.51 to 24.01 seconds and did not close the MTP gap. This neither isolates
either field's effect nor proves numerical parity with the training runtime;
the inspected source is not an established exact training-code revision.

DSpark ran through llama.cpp's `draft-dflash` path.
[Source inspection](https://github.com/ggml-org/llama.cpp/blob/d80619397c2ba5cf1366c29f7d43699ca3a159b9/common/speculative.cpp)
found it equivalent to `draft-dspark` for the tested `p_min=0` and
`sample_from_anchor=false`; the
[model graph](https://github.com/ggml-org/llama.cpp/blob/d80619397c2ba5cf1366c29f7d43699ca3a159b9/src/models/dflash.cpp)
selects the Markov head from its tensors. The draft vocabulary, Markov weights
and confidence head were present.
**Confidence-guided truncation was not tested.** This is a result for that
specific execution path, not a complete assessment of DSpark's controls.

## Tested artifacts and limits

| Component | Pinned source and tested form |
|---|---|
| Target and MTP | [Unsloth QAT GGUFs](https://huggingface.co/unsloth/gemma-4-31B-it-qat-GGUF/tree/43cc1aeb31adf47ec06a854507ce552cd9862e6f): `gemma-4-31B-it-qat-UD-Q4_K_XL.gguf` and `mtp-gemma-4-31B-it.gguf` (Q4_0). |
| Original DFlash | [Anbeeld GGUF](https://huggingface.co/Anbeeld/gemma-4-31B-it-DFlash-GGUF/tree/16e51736d898d460e527acebd5aaa31f72cf7ff7): `gemma4-31b-it-dflash-Q8_0.gguf`, declaring z-lab's Gemma 31B DFlash as its base. Exact conversion provenance was not independently established. |
| DFlash2 | [Red Hat checkpoint](https://huggingface.co/RedHatAI/gemma-4-31B-it-speculator.dflash2/tree/48505630ddf2cc05e6a827df7e54d6d8fac78966), locally converted to F16 and Q4_K_M. |
| DSpark | [Red Hat checkpoint](https://huggingface.co/RedHatAI/gemma-4-31B-it-speculator.dspark/tree/0026c7d1899651ca3c45ede471712f04849723ac), locally converted to Q4_K_M, with the separate metadata diagnostic described above. |

Local conversions used the pinned official llama.cpp converter and quantizer,
with config adaptation for the Speculators checkpoints and no importance
matrix or engine-code changes. Full upstream numerical parity was unverified.
The drafts target Google's original IT model; our target has QAT weights.
Target mismatch, draft quantization and conversion/runtime differences remain
possible explanations for poor acceptance, not established causes.

Across both comparisons, nine runtime attempts made 25 requests; 24 completed.
Native runtime including startup and shutdown totalled **7.07 minutes**;
downloads, conversion and preparation took additional time. That bounded
screen was enough to retain MTP for this short workload. It does not reject
the alternative techniques on other targets, hardware or workloads, or extend
the result to longer-context creative writing.

[Gemma 31B assessment](../models/gemma-4-31b/README.md) ·
[All investigations](README.md).
