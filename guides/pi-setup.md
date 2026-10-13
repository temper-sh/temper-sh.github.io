# Pi: web search and remote access

[Pi](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) is a
terminal coding agent. This guide covers the web-search extension selected in
our testing and the remote-access tool used by Temper's maintainer.

Add [web search](#web-search-with-pi-web-access) to let Pi look up documentation,
or [remote access](#remote-access-with-un-bien) to use a session from your phone.
Temper provides a local model server; these extensions are installed separately.

## Start with a working Pi session

Follow [Pi's installation instructions](https://github.com/earendil-works/pi/tree/main/packages/coding-agent#readme)
and connect a model provider. Before adding extensions, confirm that Pi can
answer a question and read a file in the intended project.

The [model-choice guide](choose-a-local-model.md) describes local models by
task. Pi also supports hosted providers, which can be used alongside local tools.

## Web search with Pi Web Access

Install the extension using the
[upstream command](https://github.com/nicobailon/pi-web-access#install):

```sh
pi install npm:pi-web-access
```

This installs the version currently offered by npm. The trial used **0.24.0**,
Pi **0.84.2** and a local Qwen3.8 27B model on an M5 Mac with 32 GiB. Pi called
search and content-fetching tools and answered from live pages. The
[web-search investigation](../investigations/pi-web-search.md) records the
comparison and its limits. The trial on 21 August 2026 covered search and page
fetching; it does not validate later extension releases or the whole setup.

Configure a search provider using the instructions for the installed release.
The extension offers keyless Exa search and configurable alternatives. Queries
go to an external service even with a local main model. Video processing and
other optional features were outside the trial.

Start a fresh Pi session and ask it to find a public project's official
documentation, fetch a relevant page and answer a question with a source link.
Open the source and check the answer against it.

## Remote access with Un Bien

Un Bien connects three parts: a phone app, a Pi extension and a relay.
The maintainer's macOS setup used relay 0.7.3; this is an everyday-use report,
not a comparative test.
Install the app and prepare the relay using the
[upstream setup guide](https://github.com/georgeharker/un-bien/blob/main/docs/install.md).
Then install the Pi extension:

```sh
pi install npm:@geohar/un-bien
```

Start a fresh Pi session, set the relay address and pair the app:

```text
/unbien set-relay https://relay.example.com
/unbien pair
```

Replace the example address with your relay's URL and use the same URL in the
app. Scan the pairing code, attach to a session and check that you can see its
output and send a message.

The optional launcher starts new sessions remotely; attaching to an existing
session does not require it. The documented macOS relay setup uses a user
service and offers Cloudflare Tunnel for remote access. The service requires
the Mac to remain awake and the user logged in.

The relay operator can read routed traffic. A tunnel provides reachability
and transport protection, but the conversation is not end-to-end encrypted
against that operator. Use a relay you trust and read the
[upstream privacy guide](https://github.com/georgeharker/un-bien/blob/main/docs/privacy.md)
for details.

## A version-specific macOS launcher fix

With Un Bien extension **0.20.9**, the launcher could use a different Pi
configuration directory from the working extension. The observed symptom was
repeated “no relay configured” exits while existing sessions connected
(21 September 2026).

For that version using Pi's default configuration location, this command
prevented the installer from adding the incorrect override:

```sh
PI_CODING_AGENT_DIR='' unbien-admin install launcher
```

A deliberately customised Pi directory needs its own value. Use this repair
only for the recorded version and symptom.

[All guides](README.md) · [Tools and resources](../tools.md) ·
[Back to Temper](../README.md).
