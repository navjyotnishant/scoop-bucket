# scoop-bucket

Scoop manifests for [`dun`](https://github.com/navjyotnishant/whodunit) — the
Windows counterpart to the Homebrew tap.

```powershell
scoop bucket add navjyotnishant https://github.com/navjyotnishant/scoop-bucket
scoop install dun
```

Then, in any repository:

```powershell
dun init
```

## Why this exists

`dun init` writes a git hook that resolves the binary by name at run time:

```sh
DUN="$(command -v dun || echo "<the binary that ran init>")"
```

If `dun` is not on PATH under that exact name, the hook silently does nothing
and every commit is stamped `undetermined` — which downstream reads as "no AI
was used" rather than "the tool was never installed properly".

Unzipping an archive puts nothing on PATH, so on Windows that was the likely
outcome rather than the edge case. Scoop handles PATH, and the manifest shims
the executable to `dun` regardless of what the archive names it.

## The plain archive still works

Scoop is an addition, not a replacement. Download, verify against
`checksums.txt`, unzip, and put the binary somewhere on PATH — that route is
supported for anyone whose policy forbids package managers.

## Updating

The manifest is generated from a release's real artifacts, never hand-edited:

```sh
scripts/release.sh v0.3.0
scripts/scoop-manifest.sh v0.3.0 > ../scoop-bucket/bucket/dun.json
```

Checksums are read from `checksums.txt` and the shim name is read from inside
the archive, so the manifest cannot disagree with what was published.
