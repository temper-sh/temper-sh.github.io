# Working on Temper's public site

The public name is **Temper** and the repository is
[`temper-sh/temper-sh.github.io`](https://github.com/temper-sh/temper-sh.github.io).
`results/` remains its local workspace directory. Markdown is the reader-facing
source now and the intended source for a later website; no generator is selected.

Read the [workspace instructions](../../AGENTS.md),
[V3 requirements](../REQUIREMENTS.md#public-results) and
[public-writing guideline](../PUBLIC-DOCS.md) when working in the private
workspace. Its root plan owns work order; local `PLAN.md` contains editorial
detail when present. Planning documents are not tracked or published.

## Results has no yesterday

Maintain one current assessment. Apply new evidence by revising the relevant
content in place, not appending updates, progress reports or next steps. Dates
and versions identify evidence; they do not organize a research history.
Keep older findings only where they still support a useful claim, limitation or
comparison. Follow the authoritative
[Results maintenance rule](../PUBLIC-DOCS.md#results-has-no-yesterday), including
its distinction between evidence provenance and a worklog.

## File convention

Keep the root for navigation and repository instructions. Organize reader-facing
pages by their purpose:

| Path | Content |
|---|---|
| `README.md` | Public entry point and routes to useful pages |
| `catalog/README.md` | Curated links to tools and resources, grouped by purpose |
| `models/README.md` | General-model index: memory tiers, linked names and one-line uses |
| `models/<model>/README.md` | One compact card per model, shared across its configurations |
| `models/<model>/layouts/<configuration>.md` | Optional detail for a configuration worth comparing or choosing |
| `models/<model>/chat-templates.md`, `tuning.md` | Optional model-specific template guidance and performance tweaks |
| `models/specialists/README.md` | Separate specialist index, grouped by task |
| `models/specialists/<model>/` | Specialist card and optional notes, using the same model-directory structure |
| `guides/README.md`, `guides/<topic>.md` | Guide index and explanations of concepts, choices and tasks |
| `guides/pi-setup.md` | Recommended Pi components, setup guidance and the evidence behind each choice |
| `investigations/README.md`, `investigations/<question>.md` | Investigation index, comparisons and practical verdicts |
| `templates/model-card.md` | Maintained editorial template |
| `AGENTS.md` | Contributor instructions |
| Local `PLAN.md` (ignored) | Private editorial planning |
| `.work/<task>/` | Temporary drafts, previews and generated files; ignored |

Use lowercase descriptive directory names with hyphens, for example
`models/qwen3.5-4b/README.md`. Use the [card template](templates/model-card.md)
when writing a model page. Create optional pages and the specialist index only
when there is assessed content to put there. Do not create empty tier folders,
placeholder notes or a directory cross-product of quant, template and engine.

Memory tiers are navigation in the index, never part of a model's canonical
path. Follow the [tier definitions](../REQUIREMENTS.md#memory-tiers). Different
configurations may appear in different tiers while linking to the same card or
its layout pages. Each index entry gives a very brief useful role; it does not
repeat the capability matrix. Group specialist models by their purpose, such
as embedding, reranking or transcription, with memory requirements beside each
entry. A general model used as a helper keeps its existing card.

A preset is one exact configuration: artifact and quant, chat template
or patch, engine and settings. A layout is a user-owned composition of presets.
Existing `layouts/` page paths remain stable; historical evidence and issued
format field names retain their original meanings. Keep official facts once on the model card; keep
configuration-dependent capability and performance observations distinct.
One tested configuration can stay entirely on the card. Add layout pages only
when a useful comparison needs more room. Runtime chat-template guidance belongs
beside the model, separate from the editorial template in `templates/`.

Tuning notes start from a practical problem and explain the relevant parameter
choices, why they help, and their costs. Include only the controls needed for
those decisions; link upstream for exhaustive options and syntax. Keep debugging
procedures and machine-specific research history in Labs or Field Kit. Summarize
the supporting observations and state which model/template/engine versions the
guidance covers.

## Publishing and maintenance

- Use the voice of a maintainer solving practical problems and sharing the
  work. Describe what was tried, observed, fixed and made available. Avoid
  promotional slogans, sales pitches and promises to make the reader's work
  easy. Personal statements must come from the owner's account or evidence;
  do not invent an origin story. Direct instructions still belong in guides.
- Write connected prose for explanations and procedures. Use tables for useful
  comparisons and concise navigation, and short annotated links for catalogs.
  A catalog entry helps someone decide whether to open the linked project;
  detailed setup belongs in a guide. Keep research chronology out of both.
- The main README explains Temper's purpose and routes to guides, catalogs,
  investigations, the Temper tool and Field Kit. It does not describe the
  workspace's repository architecture.
- Distinguish a tested component, an owner-reported everyday choice and a
  complete tested setup. For Pi, keep versions and test scope beside the
  recommendation; do not turn a search smoke test into a quality benchmark.
- Keep public pages concise and readable. Lead with the useful conclusion,
  explain the mechanisms needed to act on it, and link to deeper evidence or
  reference material. Avoid repeating caveats or cataloging implementation
  details just because they were encountered during research.
- Keep the landing page's routes and model indexes useful and descriptive;
  visitors should not need to browse directories.
- Keep official facts, project assessment and measured performance distinct.
  State the machine and engine version above a table that uses one configuration;
  name them per row when comparing configurations. When configurations differ,
  identify them in both the capability matrix and performance table.
- Labs and Workshop remain private. Public summaries must contain the tested
  conditions, method, useful measurements and limitations needed to assess a
  conclusion. Do not link to private workspace paths or copy logs into Results.
  Underlying records stay with their research owner; link public sources when
  useful and available.
- On relocation, update current inbound and outbound links. Preserve frozen
  source snapshots and historical path descriptions as observations of their
  producing time. Recheck navigation and source identities without rerunning
  model inference for a document move.
- Keep drafts and generated working files under `.work/` from the first write.
  The [ignore rules](.gitignore) admit the named repository files and Markdown
  pages in the content directories. Do not force-add temporary material.
- Before publication, check that each reader-facing page works from this
  repository alone. It must not require sibling research repositories or ignored
  working files. Internal editorial instructions and provenance notes may still
  refer to the private workspace.
