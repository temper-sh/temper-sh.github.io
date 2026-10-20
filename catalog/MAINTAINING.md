# Maintain the Temper catalog

This folder is Temper's installable catalog. Temper owns it; this site only
hosts it. Every Temper reads the signed files under `v1/` from
`https://temper-sh.github.io/catalog/v1/`, so a change here reaches users on
their next `temper update`.

| Path | Content |
|---|---|
| `README.md` | The "Choose a preset" page: what each preset is for, its license, size and limits |
| `sources/<platform>.json` | The authored catalog for one platform, for example `apple-silicon` |
| `v1/channels/stable/` | The signed channel: one sequence number and the current catalog per platform |
| `v1/snapshots/<sha256>/` | One signed catalog, the exact bytes of a source; never changed or removed |

Files under `v1/` are written only by `temper-catalog publish`. Do not edit,
rename or delete them by hand.

## Change the catalog

Run the commands from a Temper checkout next to this repository
(`../temper-sh.github.io`). The
[catalog contract](https://github.com/temper-sh/temper/blob/master/docs/contracts/execution-lock.md)
defines the source format; the
[publication contract](https://github.com/temper-sh/temper/blob/master/docs/contracts/catalog-signing.md)
defines the commands.

1. **Edit the source and the page together.** Change
   `sources/apple-silicon.json` and `README.md` in the same commit, so the
   page always describes the published presets. Set the source's `date` to
   the day of publication (UTC). Preset, engine and artifact IDs are
   permanent: a user's saved preset remembers the ID it came from. Removing a
   preset is safe; users who saved it keep it under **Other**.

   When adopting a measured preset, include its reviewed `profile.memory`
   observation under the
   [memory contract](https://github.com/temper-sh/temper/blob/master/docs/contracts/memory-estimates.md).
   Retain exact tested engine/version and runtime inputs, machine, loaded/peak
   bytes, counter, workload, limitations and a public assessment URL. Verify the
   tested execution matches; never relabel an older run after an engine update.
   Missing or mismatched evidence leaves estimation as the fallback. The
   measurement field needs Temper 0.4.0; older readers hide affected profiles.

2. **Preview it locally.** `--catalog` reads the file as is, without a
   signature, into a separate root:

   ```sh
   go run ./cmd/temper init --root "$HOME/.temper-dev" --dry-run \
     --catalog ../temper-sh.github.io/catalog/sources/apple-silicon.json
   ```

   Compile every preset for small and large Macs:

   ```sh
   source=../temper-sh.github.io/catalog/sources/apple-silicon.json
   locks="$(mktemp -d)"
   go build -o "$locks/temper" ./cmd/temper
   for preset in $(jq -r '.presets | keys[]' "$source"); do
     for gib in 32 64; do
       "$locks/temper" catalog compile --catalog "$source" --preset "$preset" \
         --memory-gib "$gib" --target darwin/arm64 \
         --out "$locks/$preset-$gib.lock.json" || echo "FAILED $preset $gib"
     done
   done
   ```

   Check what each supported Temper release can use; a preset that needs a
   newer Temper says so through `minimum_temper`:

   ```sh
   go run github.com/temper-sh/temper/cmd/temper-catalog@v0.1.0 availability \
     --catalog "$source" --temper-version 0.1.0
   ```

3. **Publish.** The signing seed comes on standard input and never touches a
   file. Preview first; then publish:

   ```sh
   op read "$TEMPER_CATALOG_KEY_REF" | go run ./cmd/temper-catalog publish \
     --source "$source" --root ../temper-sh.github.io/catalog/v1 --dry-run
   op read "$TEMPER_CATALOG_KEY_REF" | go run ./cmd/temper-catalog publish \
     --source "$source" --root ../temper-sh.github.io/catalog/v1
   go run ./cmd/temper-catalog verify-publication \
     --root ../temper-sh.github.io/catalog/v1 --source "$source"
   ```

   `publish` writes the new snapshot, then the next channel. It refuses a date
   earlier than the current catalog's or later than today.

4. **Commit and push** the source, the page and `v1/` together. GitHub Pages
   serves them a few minutes later. Check that the served files are the
   committed bytes:

   ```sh
   curl -fsS https://temper-sh.github.io/catalog/v1/channels/stable/channel.yaml \
     | shasum -a 256
   shasum -a 256 v1/channels/stable/channel.yaml
   ```

   Then run `temper catalog update` into a fresh root to read it the way a
   user will.

[Choose a preset](README.md) · [Back to Temper](../README.md)
