# Qwen3.8 27B / Kapitsa: technical reference

Configuration and evidence behind the
[32 GiB recommendation](kapitsa-mlx4.md). This is a measured direct-engine
configuration; **Temper has no Kapitsa backend or installable preset yet**.
The comparison is between usable compositions: weights, prompt rendering,
KV precision and engine all differ.

## Tested configuration

Measurements used an Apple M5 with 10 GPU cores and 32 GiB RAM, macOS
26.6.1 / 25G76, AC power and low-power mode off. Other chips and memory
capacities were not measured.

| Component | Selected identity or setting |
|---|---|
| Engine | [Kapitsa 0.2.9](https://github.com/nace-ai/kapitsa/releases/tag/v0.2.9), commit `e86dea79b2751d04a3fa78bfa7746f4ebf981515` |
| Target | [lmstudio-community/Qwen3.8-27B-MLX-4bit](https://huggingface.co/lmstudio-community/Qwen3.8-27B-MLX-4bit/tree/6067b15cf581666a4aecf6af3afaba4bb5efc20c), revision `6067b15cf581666a4aecf6af3afaba4bb5efc20c`; affine 4-bit, group 64; native tokenizer/rendering, no Frog override |
| Draft | [incoai/Qwen3.8-27B-DFlash2](https://huggingface.co/incoai/Qwen3.8-27B-DFlash2/tree/015e795645c74b1a0eeef3b570031fb62e769bc5), revision `015e795645c74b1a0eeef3b570031fb62e769bc5`; default 16-row DFlash/KFlash |
| Context and KV | 98,304 configured tokens; FP16 KV, 64 KiB per token |
| Engine allowance | 25,769,803,776 bytes = 24 GiB; Kapitsa flag `--mem-budget 25.769803776` uses decimal GB |
| Prefill | ANE enabled with `NI_ANE=1`; `--prefill-chunk 1024` |
| Service | One slot, loopback listener, disk cache disabled |
| Requests | Thinking off; temperature 1, top-p 0.95, top-k 20, seed 421; text only |

For direct setup, use the pinned [release](https://github.com/nace-ai/kapitsa/releases/tag/v0.2.9)
and its [usage instructions](https://github.com/nace-ai/kapitsa/blob/e86dea79b2751d04a3fa78bfa7746f4ebf981515/docs/usage.md),
then supply the target and draft directories explicitly. The measured server
arguments, with local paths substituted, were:

```sh
kapitsa serve --model /path/to/target --model-id kapitsa-qwen27 \
  --dflash /path/to/draft --host 127.0.0.1 --port 8080 --slots 1 \
  --context 98304 --mem-budget 25.769803776 \
  --disk-cache 0 --prefill-chunk 1024
```

Create a separate writable `KAPITSA_HOME` directory before launch. Measurements
also set `KAPITSA_NO_AUTO_UPDATE=1`, `KAPITSA_NO_PROMPT=1`, `HF_HUB_OFFLINE=1`
and `HF_HUB_DISABLE_TELEMETRY=1`, after obtaining all files. The command needs
the ANE environment and request settings above to match the configuration.
Installing a newer release does not reproduce these measurements.

**Model directories need care on exFAT.** The tested release scanned AppleDouble
`._*.safetensors` metadata files as weights, causing enormous allocation
attempts and startup stops. Use a directory or APFS view containing only the
declared model files. Those startup failures do not measure model memory needs.

The Splash control used **1.3.2**, Unsloth UD-Q4_K_XL revision
`4ca720788d1e01f1bff70c033e0d0028fd02e502`, Frog v22.5 revision
`855bffc49448e299789730ff92c9b8d834d6cc14`, the same DFlash2 draft, int8 KV,
112,000 configured tokens and the same 24 GiB byte allowance. Sampling and
thinking-off controls matched. The offered
[Splash preset](splash-ud-q4-k-xl.md) uses medium thinking; these results do not
measure that preset unchanged.

## Completed code changes

The Flask task added decorators through existing registration APIs, preserving
keyword names, callable identity, setup guards, deferred Blueprint registration
and annotations. The harness allowed bounded correction attempts. Independent
checks and the final typing/source review were not supplied as model feedback.

| Configuration | Final outcome | Time to submission | Loaded → peak engine footprint |
|---|---|---:|---|
| Kapitsa selected profile | Behavior checks, source review and focused typing pass | 793.93 s; four responses | 15.871 → 21.141 GiB |
| Splash thinking-off control | Behavior checks, source review and focused typing pass | 1,176.84 s; five responses | 19.429 → 22.034 GiB |
| Kapitsa initial 65,536-token / 2,048-chunk / 24-decimal-GB profile | Behavior passes; six return annotations missing, focused typing fails | 831.34 s; four responses | 15.871 → 21.494 GiB |

Both selected final patches pass 92 original, 57 independent and 21 automated
review checks; Kapitsa adds 23 passing tests and Splash adds 32. Kapitsa's first
patch broke six keyword-name cases, its second proposal was invalid, and its
third patch passed; the fourth response submitted that patch. Splash also
required corrections. Source review was unblinded and AI-assisted. The focused
mypy 2.4.0 check covered two changed modules with a Python 3.14 override and
imported-module errors suppressed, not full-project or downstream call-site
typing. No human acceptance time was measured.

The annotation failure in the initial Kapitsa configuration remains contrary
evidence. Multiple settings, calibration and response trajectories differed;
the final outcome cannot be attributed to changing chunk size.

A separate pagination repair favored Splash: **180.84 s / three responses**
versus initial-profile Kapitsa's **209.97 s / four responses**. Both final
patches passed original, added and independent behavior tests plus source review.
Loaded/peak footprint was **19.43/20.47 GiB** for Splash and **15.87/19.08 GiB**
for Kapitsa. This was the initial 65,536-token comparison, not a repeat of the
selected Kapitsa profile.

## Long input and continuation

The matched sequence used exact padded record identifiers, a source edit,
an explicit integer sequence from 1 through 1,800, and a continued lookup.
Kapitsa used the selected profile; Splash used the 112,000-token control.

| Request | Kapitsa elapsed | Splash elapsed | Outcome |
|---|---:|---:|---|
| Cold lookup, 85,565 input tokens | 981.18 s | 608.34 s | Both values correct; Splash adds a forbidden Markdown fence |
| Append, about 85,675 input tokens | 3.68 s | 3.93 s | Both correct bare JSON |
| Early source edit, 85,554 input tokens | 978.98 s | 612.93 s | Both corrected values; Splash fence; no cached input in either |
| Complete 7,895-token output | 259.05 s | 279.27 s | Correct, byte-identical sequences |
| Continued lookup, about 93,620 input tokens | 7.44 s | 83.49 s | Both values correct; Splash fence |

Loaded/peak engine physical footprint across the sequence was
**15.871/23.992 GiB** for Kapitsa and **19.428/23.045 GiB** for Splash.
Kapitsa's lower loaded footprint does not extend to this peak.

After long output, Kapitsa reused 93,571 tokens and processed only 46 new
prompt tokens; Splash reused 85,664 and processed 7,958. Native logs report
1.445 versus 79.014 seconds of prefill. The continued question repeated the
same identifiers already answered. This measures continuation/cache reuse,
not fresh retrieval after long output or coding across the full context.

The explicit padded identifiers matter: a separate numeric-label request at
36.7k input returned the wrong middle row. A configured window is not a
quality guarantee, and syntactic JSON validity did not detect that error.

A **separate 106,496-token configuration** passed a sequence reaching 91.7k
source input, 7,895 generated tokens and 99.7k continued input. It used
**15.871/24.130 GiB** loaded/peak footprint; cold lookup took **1,092.24 s**
and continuation **7.14 s**. This establishes that workload extent only. It
is neither the selected 98,304-token setting nor a measured hardware ceiling
or capacity advantage over Splash.

## Tuning and confirmation boundaries

ANE and 16-row drafting were retained. Disabling ANE or using eight draft rows
did not improve the exposed short/populated pair. With identical initial
calibration, 1,024-token chunks reduced populated peak footprint from
**19.221 to 18.924 GiB**, with **15.871 GiB loaded** in both. Short/populated
elapsed was **11.02/139.25 s** versus **11.81/145.43 s** for 2,048 chunks;
answers were byte-identical, including a forbidden populated-answer fence.
The smaller buffer is the reason for selection; one pair does not establish
a stable speed advantage.

A separate four-case state audit, unused for tuning, gave Kapitsa **4/4 correct
states and bare JSON in 7.038 s**, at **15.871/18.924 GiB** loaded/peak
footprint. Splash gave **4/4 correct states in 7.080 s**, at
**19.426/20.463 GiB**, but added a forbidden fence. Both compositions had
format slips elsewhere. This narrow audit does not establish general reliability.

## Measurement limits and provenance

Elapsed times cover requests or the bounded coding workflow, excluding server
startup. Target weights were on external exFAT storage while Splash and draft
files were on internal APFS; startup I/O was not controlled. Tool calls are
buffered until complete by Kapitsa, so first visible tool output is not a
measure of prefill time.

Memory is **engine-process physical footprint**, sampled every five seconds
and including the OS lifetime peak. It excludes Splash helper processes and
allocations in the separate ANE service. System wired-memory growth was also
recorded, but overlaps process memory and includes unrelated activity. Counters
must not be summed or interpreted as minimum machine RAM. Long-context trials
completed without a memory-pressure or swap stop on this M5.

Measurements were recorded on 22–23 October 2026. The selected confirmation
arms started from separate copies of identical KFlash calibration bytes,
SHA-256 `3b01f5a4dd0d4764410d842541ff91b79fb0559c2a8e6dec6c39a30fcbb5abcd`.
A fresh installation calibrates locally and may differ. The release archive
SHA-256 was `370b2ec86b0d27fcb6cf9e19a563e30174e89146889bdf3adc1c3c5d01898d19`.

[Short recommendation](kapitsa-mlx4.md) · [Splash reference](splash-ud-q4-k-xl-reference.md) ·
[How to read the technical references](../../../guides/technical-reference.md).
