# Qwen3.8 27B with Splash

**UD 3.1 Q4_K_XL with Splash 1.7.0 is our default for Qwen with Splash.**
It requires Temper 0.6.1+, Apple M3 or newer, macOS 26.4+ and 32 GiB+.
BaseQuant Medium is available as an alternative.

The weight choice follows accepted CSV and Ruby changes on an M5 / 32 GiB
with Splash 1.3.2. BaseQuant Medium used less memory and was faster on the Ruby
follow-up alone. This completed-work comparison does not establish a general
coding-quality ranking or memory fit on 1.7.

| Work including review repairs | BaseQuant Medium | UD 3.0 | UD 3.1 |
|---|---:|---:|---:|
| CSV model-active time | 66m04s; unaccepted | 65m49s; unaccepted | **64m38s; accepted** |
| Ruby initial active model/test time | 70m52s | 90m29s | **44m23s** |
| Ruby follow-up active model/test time | **50m06s** | 96m31s | 54m29s |
| CSV repair wired loaded / peak GiB | **18.27 / 21.11** | 18.89 / 21.70 | 19.33 / 22.14 |
| Ruby follow-up wired loaded / peak GiB | **18.27 / 21.46** | 18.88 / 22.29 | 19.35 / 22.02 |

UD 3.0 produced correct follow-up source, but repeating its final patch
exhausted context. UD 3.1 completed submission. It fixed the initial Ruby
clock defect using its own tests, then needed review repair for the follow-up
policy API and validation. CSV also needed one review repair. Acceptance was
AI-assisted and label-aware; reviewer effort was not timed.

All used Splash 1.3.2, DFlash2, Frog v22.5, int8 conversation memory and medium
thinking. Ruby initial allocated 112,000 tokens; CSV and follow-up 114,681.
The UD 3.1 runs used a newer macOS and Temper supervisor. Different histories and
output volumes affect time, so these observations do not isolate quant speed.
System wired growth includes other applications; five-second samples can miss
peaks. Memory counters are not minimum machine RAM. The UD 3.1 quant-comparison
tasks incurred no swap growth or resource stop.

## Why this combination

The offered default retains the tested 114,681-token allocation under a
24 GiB engine allowance, medium thinking and the same draft/template. Its
accepted Ruby follow-up reached 82,363 occupied tokens; full-window quality
and peak memory remain unmeasured. The separate llama.cpp/MTP preset
serves older Apple Silicon and retains its own target weights.

Splash was originally selected for shorter first code submissions than
llama.cpp in two matched tasks. The [technical reference](splash-ud-q4-k-xl-reference.md)
retains those engine comparisons, exact UD 3.1 provenance and historical
larger-machine observations. Those older
measurements are not relabeled as UD 3.1 results.

Splash 1.7 includes the custom-template override, so Temper can use Frog without
a local frontend patch. Native chat/tools, cancellation and recovery checks
passed. In the repeated initial Ruby task, it reached acceptance after one
review repair and 65m16s of active model/test time. Engine physical footprint
was 19.87 GiB loaded and 22.68 GiB peak at 112,000 allocated tokens. This is not
full-window proof or minimum machine RAM. The catalog uses an estimate because
its retained 1.3.2 memory observations do not match the selected 1.7 engine.
[Matched release comparison and its harness limitation](splash-ud-q4-k-xl-reference.md#splash-17-compatibility-and-completed-work).

**[Set up with Temper](https://github.com/temper-sh/temper#get-started)** ·
[Exact comparison and limitations](splash-ud-q4-k-xl-reference.md#ud-31-default) ·
[Qwen model overview](../README.md).
