# Olmo 3: handling an absent tools variable

The embedded chat template failed during parser generation when the request
did not supply a `tools` variable. The **tools-guard-v2** patch used in the
[model card](README.md) adds a default before the unchanged original template:

<!-- {% raw %} -->

```jinja
{%- if tools is not defined -%}{%- set tools = none -%}{%- endif -%}
```

<!-- {% endraw %} -->

This converts an absent variable to `none`. Explicit tool lists and null
values are unchanged. The [model card](README.md) identifies the engine and
settings used with this patch.

With the patch, the diagnostic started and answered a text request, but
returned an empty response to a tool request. Subsequent suite cases executed
read and publish tools; recovery from a conflicting edit still failed.

These observations describe the patched configuration. They do not establish
general tool reliability or separate model behaviour from the engine parser.
The failure without the patch remains recorded separately.
