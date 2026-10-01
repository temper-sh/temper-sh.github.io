# Investigations

Experiments on specific questions about local AI, with the tested conditions,
findings and limitations.

| Question | Finding |
|---|---|
| [Does Magnitude improve our M5 setup?](magnitude-m5.md) | Magnitude 0.2.2 did not justify replacing Splash or llama.cpp on the tested M5 with 32 GiB. |
| [Does hashline editing save tokens without losing correctness?](hashline-editing.md) | With Qwen3.6 27B, median output fell by 20.8%, but correct files fell from 30/32 to 24/32. |
| [Which web-search tool fits Pi?](pi-web-search.md) | A review of extension alternatives and two local integration trials led to Pi Web Access; provider quality was not benchmarked. |

[Assessing local AI](../guides/our-approach.md) explains the distinction between
a focused test, a completed task and measured savings in human effort.

[Guides](../guides/README.md) · [Model catalog](../models/README.md) ·
[Back to Temper](../README.md).
