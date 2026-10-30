# FrogNano 4B

FrogNano completed booking corrections, note edits and export conversations in
the tested native configuration. It still acted without fresh stock evidence,
missed lookups in larger inputs and produced no accepted coding repair in our
bounded trials. Its faster native configuration is not a Temper catalog preset.

**Official:** Microsoft's Qwen-derived coding post-train, released under MIT.
The publisher targets Python-heavy agentic software work with its Leaf scaffold
and explicitly excludes general-assistant use from its evaluation.
[Official model details](https://huggingface.co/microsoft/FrogNano-4B-2609).
Our helper and supplied-source trials examine different uses; they do not
reproduce the publisher's coding benchmarks.

## Observed work

Seven short English scenarios used simulated applications and three seeds.
An unblinded AI reviewer checked replies, actual tool calls and intermediate
and final state. No person independently accepted the work. The native
configuration imports Bartowski Q4_K_M into MLX affine tensors, uses Kapitsa
0.2.15, thinking off and a 16,384-token window. It is a different weight
composition from the original GGUF.

| Task | Observed result |
|---|---|
| Booking changes | Preserved both corrections and the existing duration in all three dialogues |
| Notes and ownership | Ignored the tested retrieved injection, refused prohibited edits and completed authorized edits after reading ownership; false ownership assertions were untested |
| Stock and fitting | Omitted every required final positive-stock refresh; truthfully reported the actual fitting failure |
| Export and cancellation | Completed all three export and cancellation sequences; stale completion did not undo cancellation |
| Reception | Kept the role boundary but omitted supplied status and sometimes invented next steps |
| Larger-input lookup | At about 12.3K input tokens, plain and MTP native runs returned the wrong middle value on cold and appended requests; the early-edit request was correct |
| Supplied-source coding | Q4, Q8, BF16 and plain native pagination attempts exhausted their first reasoning allowance without a patch; the separate Flask attempt produced an incomplete patch and no accepted repair |

A larger-budget GGUF source-audit attempt correctly traced four transaction
cases as bare JSON. That narrow success does not establish general code review
or repair ability. More weight precision did not consistently improve helper
work or resolve the larger-input lookup errors. One native off reply exposed
self-correcting deliberation during an authorized note edit.

## Runtime choices and cost

MTP depth 4 reduced native conversation workload time from 133.45 to 84.58
seconds, preserving the actual operations and final states. It did not repair
the stock-grounding errors. These are matched sequential observations, not
independent reliability estimates.

| Machine | Configuration and workload | Time | Loaded → peak memory |
|---|---|---:|---|
| Apple M5 · 32 GiB · macOS 26.6.1 | Kapitsa 0.2.15, imported Q4, MTP 4, thinking off, 16K allocation; 21 dialogues / 66 user turns / 107 requests | 1.23 s median turn; 84.58 s total | Engine RSS 3.30→3.31 GiB; system wired growth 3.11→4.01 GiB |

The native plain condition took a median 1.79 seconds per turn, with RSS
3.30→3.31 GiB and wired growth 3.12→3.90 GiB. Original llama.cpp b11808 Q4
off took 3.27 seconds per turn, with RSS 3.27→3.31 GiB and wired growth
3.27→3.34 GiB. Conversion and effective sampling differ across engines:
Kapitsa maps requested top-k zero to 32. The cross-engine comparison therefore
does not isolate the engine alone.

Actual native prompts contained 132–1264 tokens. Requests allowed 512 output
tokens and none reached that limit. The native engine used a 12-decimal-GB
memory allowance, ANE off and disk caching off; this allowance is not measured
RAM use. Turn medians include incorrect work, rather than measuring time to an
accepted result. No swap growth or thermal/resource stop was observed.

RSS and wired growth overlap and must not be added. RSS can omit GPU
allocations; system wired growth includes other applications, and five-second
sampling can miss peaks. These 32 GiB observations do not establish minimum
RAM or actual fit on an 8/16 GiB machine.

Kapitsa is parked and excluded from the prepared catalog. Its retained Temper
adapter requires a public MLX target and DFlash2 draft; it cannot express this
imported MTP composition. Splash 1.4.0 rejected
FrogNano's hidden size. These are version-specific runtime boundaries, not
model-quality failures. The [original GGUF](https://huggingface.co/bartowski/FrogNano-4B-2609-GGUF)
remains a manual llama.cpp option, with the limitations above.

<details>
<summary>Measurement identity</summary>

Measurements: 24 October 2026. GGUF revision
`dfbb801e04ddf045ffd245b9f1a546ed0c7d960f`, Q4_K_M SHA-256
`01fba00c57e2976399ae4947f0b4fdde4623d3e410a435bf2ae39ddf164e9b0a`.
Kapitsa revision `5c489cc69b397d2088aa165f9017129b4a724b7a`.
Weights were on USB/exFAT storage; runtime and evidence on internal APFS.
Native thinking also produced reasoning-only output stops in three reception
dialogues under its separate 2048-token allowance; that condition is not the
off-mode configuration measured in the main table.

</details>

[All models](../README.md) · [Explore presets](../../catalog/README.md).
