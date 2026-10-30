# Granite 4.2 3B

Useful for field extraction, constrained copyediting and export conversations.
It is not selected as another action-helper preset: both tested reasoning modes
edit another person's explicitly prohibited note in two of three dialogues.
Thinking adds waiting time and can also invent replacement approval.

**Official:** IBM's dense 3B text model with reasoning support, Apache 2.0.
[Model details](https://huggingface.co/ibm-granite/granite-4.2-3b).

## Helper work and cost

Seven simulated application conversations use three seeds per mode. Unblinded
AI review checks actual calls, intermediate state and delivered replies; no
person independently accepted the work. Q4_K_M, llama.cpp b11808, embedded
template, 16K/F16, one GPU slot, four threads, mmap, batch/microbatch 512,
prompt caching and T1/top_p .95/top_k 0. Requests allow 512 tokens off or
2,048 total with 512 thinking tokens. These are study allowances.

Both modes complete export and cancellation in all three seeds, retaining
cancellation after a stale completion. Thinking preserves both booking
corrections in 3/3 versus 2/3 off, but stale-stock errors remain. Off invents
unavailable archiving; thinking falsely approves a replacement in one dialogue.
The 512-record lookup also returns wrong values. These findings limit delegated
actions without erasing the useful bounded copyediting evidence below.

| Machine | Execution and work | Median turn / first visible | Engine RSS loaded → peak | Wired growth loaded → peak |
|---|---|---:|---:|---:|
| Apple M5 · 32 GiB · macOS 26.6.1 | b11808, 16K/F16, off; 21 dialogues / 66 turns | 2.51 / 0.24 s | 3.47 → 3.50 GiB | 3.33 → 3.45 GiB |

Actual off-mode inputs are 126–1,253 tokens/request; total generated output is 5,009 tokens. Incurred task time is 179.02 s, including wrong work. Thinking takes 13.68 s median, with RSS 3.47 → 3.51 GiB and wired growth
3.29 → 3.54 GiB at the same allocation. RSS and wired growth overlap; do not
add them. Process counters can omit GPU allocations, system growth includes
other applications, and five-second sampling can miss peaks. No observed swap
growth. Actual 8/16 GiB fit and human time saved remain unmeasured.

<details>
<summary>Earlier capability and copyediting comparison</summary>

These b10655 observations use Q4_K_M/16K, with 4,096 output tokens off and
8,192 with low reasoning. Their different output/reasoning conditions remain
separate from the helper comparison above.


AI reviewers assessed selected cases against supplied sources. Tools were
simulated; the work was not independently accepted by a person.

| Capability | Reasoning off | Low reasoning |
|---|---|---|
| Extraction | Correct fields | Correct fields |
| Classification | Correct queues, priorities and conversions | Correct queues, priorities and conversions |
| Reconciliation | Correct room update | Correct room update |
| Comprehension and synthesis | Contradictory dates and speculation | Invented approval/payment condition |
| Reasoning | Correct scheduling choice | Correct choice; contradictory explanation |
| Evidence assessment | Labels correct; overclaimed missing evidence | False signature-evidence conclusion |
| Drafting | Short notice complete; calendar omissions | Short notice complete; calendar claims needed repair |
| Editing | Completed the constrained copyedit | Completed the constrained copyedit |
| Translation into Swedish | Substantial meaning changes | English left untranslated; entrance status reversed |
| Following updated instructions | Latest arrangement correct | Latest arrangement correct |
| Tool use | Found and read the record | Found and read the record |
| Planning and execution | Valid plan; final publish read omitted | Plan invented passed checks; final read omitted |
| Recovery | Overwrote concurrent P9 with outdated P8 | Preserved and verified P9 |
| Saving and reporting results | Saved, reread and validated | Saved, reread and validated |

## Performance

Measured 12 September 2026; the configurations above.

| Configuration | Machine | Engine | Prefill (tokens/s) | Generation (tokens/s) | Peak process memory |
|---|---|---|---:|---:|---:|
| Off | Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 594.0 | 48.2 | 3.56 GiB |
| Low | Apple M5 · 32 GiB · macOS 26.6.1 | llama.cpp b10655 | 631.4 | 47.2 | 3.46 GiB |

Rates are medians across 40 / 39 calls in 23 cases per setting, with one
server slot and prompt caching enabled. Prefill counts newly processed input.
Generation includes reasoning tokens when enabled. Memory is sampled resident
memory (RSS) of the server process; it does not establish the machine's RAM
requirement. The tasks vary, which limits speed comparisons with other
workloads.


Measured helper work 24 October 2026; earlier comparison 12 September 2026.

</details>

[All models](../README.md).
