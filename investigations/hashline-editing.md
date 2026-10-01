# Does hashline editing save tokens without losing correctness?

**Keep Pi's built-in string replacement for the tested Qwen3.6 27B setup.**
Our hashline port reduced median output tokens by 20.8%, but produced six fewer
correct files across 32 attempts. We could not establish a time saving.

The [hashline benchmark](https://stencil.so/blog/the-harness-problem) reported
**61% fewer output tokens for Grok 4 Fast** by changing its editing tool.
On a local machine, generating less text could mean less waiting without
buying more hardware. That made the technique worth investigating. The number
belonged to a particular model and benchmark; we wanted to see whether the
benefit carried over to our setup.

The idea makes sense. An ordinary replacement tool asks the model to quote
the old text and supply its replacement. If the old text does not match, it
has to try again. Hashline lets it address lines directly, saving the text
spent identifying the target. We ported the approach to Pi, using line numbers
and a file hash to detect stale reads.

We compared that port with Pi's built-in string replacement on **28 July
2026**, using **Qwen3.6 27B at 4-bit compression**. Both formats received the
source and attempted the same 16 tasks twice. We checked the final files
against exact expected outputs and disabled whole-file writing so the model
had to use the edit tool.

| Measure | String replacement | Hashline port |
|---|---:|---:|
| Correct files | 30/32 | 24/32 |
| Median output tokens | 183 | 145 (20.8% fewer) |
| Recorded input tokens, total | 80,801 | 111,404 (38% more) |

The smaller output came with six fewer successful attempts. Looking at the
edits explained part of the loss: the model could locate the right line and
still rewrite it incorrectly. In one task it dropped three leading tabs.
Other edits had incorrect indentation or an extra blank line.

Ordinary string replacement could leave surrounding whitespace untouched.
Replacing whole lines made the model generate that whitespace again. Both
formats had seen the source; the difference was what the model had to put
into the edit. A valid reference to a line did not guarantee a correct
replacement.

There was another cost behind the output saving: recorded input grew by 38%,
partly because the new tool needed longer instructions. Swapping made elapsed
time unreliable, so we could not establish that the shorter output made the
work faster.

The original benchmark gave us a useful idea to test, but its headline saving
did not predict the tradeoff with our model. This comparison covers our local
port and fixtures; it does not
invalidate the author's measurements or assess current upstream releases.

The test ran on an Apple M5 with 32 GiB RAM, Rapid-MLX 0.11.0 and Pi 0.82.1.
Token figures cover 32 completed string-replacement attempts and 31 hashline
attempts. Hashline's timeout remains a failed task; its incomplete usage is
excluded from token statistics.

[All investigations](README.md) · [Pi setup](../guides/pi-setup.md) ·
[Back to Temper](../README.md).
