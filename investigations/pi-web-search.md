# Which web-search tool fits Pi?

**Pi Web Access is the selected extension for this setup.** It handled web
search and page fetching in a local trial. The choice followed a review of
several packages, considering Pi integration, dependencies, installation
behaviour and maintenance.

**Claim reproduction, tested 21 August 2026.** Two candidates reached local
trials. The others received package and documentation review or could not be
verified. The result supports the tested integration, not a comparison of
search quality or later releases.

## Options considered

| Candidate | How it was assessed | Outcome and reason |
|---|---|---|
| [Pi Web Access](https://github.com/nicobailon/pi-web-access) 0.24.0 | Package review and local trial | Selected. Search and fetching worked through native Pi tools, using Pi's JavaScript runtime. |
| [webfetch](https://github.com/firish/webfetch), `webfetch-llm` 0.1.3 | Package review and local trial | Worked, but needed a Pi-to-Python bridge and a separate Python environment. |
| [donsetch](https://github.com/dondai44423/donsetch) | Package review; no local trial recorded | Set aside. The recorded concerns were its AGPL licence, binary download during installation and emphasis on bypassing bot protections, which was outside this setup's needs. |
| [pi-web-access-lean](https://github.com/Nabsku/pi-web-access-lean) | Package review; no local trial recorded | A fork of Pi Web Access. The review preferred upstream; the fork's last npm release was from May 2026. |
| [pi-web-extension](https://github.com/NicoAvanzDev/pi-web-extension) | Package review; no local trial recorded | Set aside during the maintenance review; its last npm release was from March 2026. |
| `pi-web-agent` | Package lookup | An unverified research suggestion. No matching npm package was found. |

These are the reasons recorded in the August review. The release dates refer
to npm packages, not repository activity. The screened-out options have no
measured functionality, search-quality or performance result here.

## What the local trials established

The trials used Pi **0.84.2** and **Qwen3.8 27B UD V3 XL** at 100k context on
an **Apple M5 with 32 GiB**.

The tested Pi version could not use webfetch's MCP server directly. A small
extension called its Python library in an isolated environment. Search and
fetching worked; a cold DuckDuckGo search took **33 seconds**, and a cached
repeat returned immediately. The default installation brought in **78 Python
packages** and used a simpler text-ranking fallback.

Pi Web Access was installed locally in a scratch project. The model called
both `web_search` and `fetch_content` and answered correctly from live pages.
That established the required Pi integration without maintaining the Python
bridge.

The trials did not compare search relevance, relative speed or time saved on
a larger research task. The package count and cold-search timing belong to
webfetch alone.

## Search providers

The extension and its search provider are separate choices. The Pi Web Access
trial used keyless Exa search, which sends queries to an external service even
when the main model runs locally.

[Tavily](https://docs.tavily.com/documentation/api-reference/endpoint/search)
was considered as another provider if Exa's results proved insufficient.
No Tavily trial or quality comparison with Exa is recorded in this research.

## Using the result

The [Pi setup guide](../guides/pi-setup.md) covers installation, a simple check
and remote access with Un Bien. The trial covers **0.24.0**; newer releases,
optional video processing and additional providers were not tested. Project
links and installation details were checked on 24 September 2026.

[All investigations](README.md) · [Tools and resources](../tools.md) ·
[Back to Temper](../README.md).
