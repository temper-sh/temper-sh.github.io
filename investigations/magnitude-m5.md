# Magnitude on an M5 with 32 GiB

Magnitude 0.2.2 did not give us a reason to replace Splash for Qwen or
llama.cpp for Muse and Gemma. We tested its native engine on an Apple M5
with 32 GiB on 1 October 2026, using the same Unsloth UD-Q4_K_XL model
weights as our recommended presets.

| Model | Measurement on M5 / 32 GiB | Magnitude 0.2.2 | Reference |
|---|---|---:|---:|
| Qwen3.8 27B | Time to submit the streaming fix | 66m 47s | 40m 27s, Splash `fdc11c5` |
| Muse Glimmer 30B | Time to produce a technical decision note | 369s with DFlash; 174s without | 160s, llama.cpp b11205 |
| Gemma 4 26B-A4B QAT | Generation speed over 512 tokens | 46.2 tokens/s | 48.7 tokens/s, llama.cpp b11205 |
| Gemma 4 31B QAT | Generation speed over 512 tokens | 6.77 tokens/s | 6.89 tokens/s, llama.cpp b11205 |

Request times include input processing and reasoning. The separate generation
checks count reasoning tokens too. Splash's reference comes from the
26 September comparison; the llama.cpp controls were measured in this study.

Muse's DFlash mode uses a smaller model to propose tokens for the main model
to verify. Only about 1% of those proposals were accepted in the three normal
work requests. A short check that always chose the most likely next token
suggested a modest speed gain over plain decoding. That gain disappeared
in the ordinary work requests.

Quality still required review. Qwen's submitted patch suppresses a context
cleanup error too broadly; this finding comes from static review, with
execution tests unrun. Muse's decision note needed correction in all three
configurations. Gemma 26B produced some answers sooner with Magnitude, but both
engines exhausted the 4,096-token allowance on one writing task without
delivering a visible scene.

Loading was a practical weakness. All four initial loads failed Magnitude's
memory checks. Cached restarts succeeded, with Qwen also needing smaller
input-processing chunks. Startup briefly reached warning memory pressure;
Qwen's short diagnostic added 107.5 MiB of system swap. Generation then
remained stable, and no out-of-memory crash was observed. We established no
memory-efficiency advantage over the existing engines.

Gemma 26B retrieved records and continued correctly from 32,768 input tokens;
Muse and Gemma 31B did so at 8,192. Gemma 26B's 90,112-token request and the
others' 32,768-token requests reached the ten-minute deadline without an
answer. These observations establish a latency limitation for those requests.

These are single attempts with unblinded AI review. Chat templates, KV-cache
formats and Qwen's draft model differ from the references, limiting attribution
to the engine alone. The study ended with Qwen's second patch cancelled and
its context checks unrun.

[Splash coding comparison](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl.md)
· [All investigations](README.md).
