# Technical references

These pages contain the configuration details and evidence behind the short
preset descriptions. They are intended for advanced readers and agents helping
someone set up local AI.

| Reference | Contents |
|---|---|
| [Preset configuration and measurements](../catalog/reference.md) | Published versus evaluated settings, resource observations and specialist interfaces. |
| [Qwen / Kapitsa](../models/qwen3.8-27b/layouts/kapitsa-mlx4-reference.md) | Parked experimental layout: exact inputs, coding failures and successful cases, latency and memory on 32/64 GiB machines. |
| [Qwen / Splash](../models/qwen3.8-27b/layouts/splash-ud-q4-k-xl-reference.md) | Decision rationale, alternatives not selected, exact configurations, coding outcomes and resource measurements. |
| [Qwen / llama.cpp](../models/qwen3.8-27b/layouts/llama-cpp-reference.md) | Document tasks, context/cache trials and their failed cases. |
| [Qwen context and caching](../models/qwen3.8-27b/tuning.md) | Parameter interactions and the conditions behind tuning guidance. |
| [Olmo chat template](../models/olmo-3-7b-instruct/chat-templates.md) | Template changes and their compatibility purpose. |
| [Temper command documentation](https://github.com/temper-sh/temper) | Product setup, configuration and runtime interfaces. |

## Reading route for setup agents

1. Establish the user's machine, available resources, application and intended
   work. Read the [preset directory](../catalog/README.md) and the relevant
   model or preset page for its purpose and limitations.
2. Read the linked technical reference for constraints, integration details and
   the reasons behind choices and alternatives. Check the weights, engine,
   settings, workload and machine before applying an observation or decision to
   the proposed setup. A retained default is not necessarily an optimized value.
3. Use Temper's documented commands to resolve installation inputs from its
   published catalog. The [stable channel](../catalog/v1/channels/stable/channel.yaml)
   identifies the signed snapshot. [Authoring sources](../catalog/sources/apple-silicon.json)
   can contain unpublished candidates; reference pages also describe experiments.
4. Explain the proposed selection and changes before the user chooses them.
   Follow the product's setup and review flow rather than constructing a new
   configuration from an experiment's settings.

## Interpret the evidence

**Published** describes what the signed catalog offers. **Evaluated** describes
a specific tested configuration, which may differ from that offering.
**Estimated** describes an expectation, such as memory fit on an untested machine.
**Recommended** describes our selected configuration for a stated use and
machine class; it does not imply availability in Temper's signed catalog.

A configured context window does not establish accuracy at that length.
A measured process peak does not establish minimum machine RAM. A passed case
does not establish reliability across a class of work. Keep observed failures
and missing measurements with the result when advising the user.

Historical observations retain their exact configurations. Installing newer
software does not make older timings or quality findings measurements of the
new setup. [Our assessment method](our-approach.md) explains the other limits.

[Explore presets](../catalog/README.md) · [All guides](README.md).
