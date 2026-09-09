# Bad Apple — Public Beta

Bad Apple is a sovereign, local AI operating-system layer for macOS. It runs the
model on your Apple Silicon Mac, keeps prompts and actions off rented cloud GPUs,
and gives you a native menu-bar assistant, local voice/TTS, a hash-chained audit
ledger, and a secure identity agent backed by the Secure Enclave.

This repo holds public release artifacts only. The development source stays
private.

## What’s new in 0.1.3

- **Curious autopilot now thinks and acts.** `curious_self_improvement` feeds
  cert, doctor, firewall, git, and source-marker data into the local 7B model,
  proposes a concrete `{"patch":{...}}` or `{"no_patch":true}`, and applies the
  patch when Autopilot is on.
- **Bounded self-modification.** The autopilot can edit text files, create missing
  data files (e.g., the output firewall blocklist), and verify its own writes,
  but it is blocked from touching core control files such as the engine and
  policy source.
- **Automatic blocklist creation.** A missing `/var/lib/bad_apple/blocklist.txt`
  is created on the first `curious check`, so the air-gap cert suite reports a
  clean firewall state.

## Download the latest beta

**[Bad Apple 0.1.3 — Download `Bad_Apple-0.1.3-unsigned.zip`](https://github.com/savageAZfck/bad-apple-releases/releases/download/v0.1.3/Bad_Apple-0.1.3-unsigned.zip)**

Or view the release page with notes, checksum, and install instructions:
**[Bad Apple v0.1.3 release](https://github.com/savageAZfck/bad-apple-releases/releases/tag/v0.1.3)**

Older releases are available at [https://github.com/savageAZfck/bad-apple-releases/releases](https://github.com/savageAZfck/bad-apple-releases/releases).

Latest release will always be at `https://github.com/savageAZfck/bad-apple-releases/releases/latest`.

## What you get

- **Local 7B/9B MLX model registry** for coding, reasoning, vision, and general
  queries, with an optional 0.5B fast tier.
- **Native voice and TTS** — speak to it, and it speaks back.
- **Secure Enclave identity** for signing and verifying SLICKS 2.0 proofs.
- **Air-gapped by default** — no required network once model weights are cached.
- **Output firewall** to block unwanted tokens and patterns.
- **Tool system** for local files, shell, AppleScript, Shortcuts, screen capture,
  workspace memory, and more. Destructive actions ask for approval by default;
  the menu-bar **Autopilot** toggle can skip approval for trusted actions.
- **Menu bar app** as the primary daily interface — chat, model download,
  settings, voice, dashboard, Autopilot, and monitoring.
- **Native CLI** (`badapple`) and fetch helper (`badapple-fetch`) for install,
  scripting, and advanced use.

## Requirements

- macOS 26.0 or later.
- Apple Silicon (M1 or newer).
- **8 GB of unified memory** is the practical minimum; **16 GB** is recommended
  for comfortable use. The 7B model uses about **4 GB** at peak; the rest is for
  macOS and other apps.
- **Keep at least 40 GB free** for the OS, model cache, and swap. The 7B model
  weights are about **4–6 GB** on disk once cached.

## Install

The easiest install is Homebrew:

```bash
brew tap savageAZfck/bad-apple https://github.com/savageAZfck/homebrew-bad-apple
brew install --cask bad-apple
```

Or manually:

```bash
unzip Bad_Apple-0.1.3-unsigned.zip
cd Bad_Apple-0.1.3-unsigned
sudo ./install.sh
```

The installer runs a preflight check, copies `Bad Apple.app` into `/Applications`,
installs system LaunchDaemons, links `badapple` and `badapple-fetch` into
`/usr/local/bin`, and loads the menu bar agent.

> **You only need the terminal for install.** After that, day-to-day use is the
> menu bar app, voice, and the local dashboard. Advanced users can still use the
> `badapple` CLI for scripting or diagnostics.

## First run

Bad Apple is intentionally air-gapped: the daemon sets `HF_HUB_OFFLINE=1` so it
only loads cached model weights.

- **If you already have the model cache** from a previous install or P2P seed,
  Bad Apple starts immediately.
- **If you want to download models on first use**, open the menu bar, go to
  **Models**, and allow downloads. This sets `BADAPPLE_ALLOW_DOWNLOADS=1` for the
  fetch helper. The 7B model is about 4-6 GB.
- **If you want to stay air-gapped**, seed the cache manually with `badapple-fetch`:

  ```bash
  BADAPPLE_ALLOW_DOWNLOADS=1 badapple-fetch mlx-community/Qwen2.5-Coder-7B-Instruct-4bit
  ```

## Important notes

- Bad Apple is **unsigned and not notarized**. This is intentional: notarization
  would upload the binary to Apple, which conflicts with the local-privacy goal.
- Homebrew strips the quarantine flag during install. If you install manually and
  macOS warns you, right-click `Bad Apple.app` and choose **Open**, or run:

  ```bash
  xattr -dr com.apple.quarantine "/Applications/Bad Apple.app"
  ```

- This is a **public beta**. Expect rough edges, missing polish, and bugs.

## Uninstall

Homebrew:

```bash
brew uninstall --cask bad-apple
brew untap savageAZfck/bad-apple
```

Manual:

```bash
sudo /Applications/Bad\ Apple.app/Contents/Resources/strip_quarantine.sh 2>/dev/null || true
sudo rm -rf /Applications/Bad\ Apple.app
sudo rm -f /Library/LaunchDaemons/com.badapple.*.plist
sudo rm -f /usr/local/bin/badapple /usr/local/bin/badapple-fetch
sudo rm -rf /var/lib/bad_apple /var/run/badapple
rm -rf ~/.bad_apple
```

## Troubleshooting

- Run `badapple --doctor` for a system health check.
- Check `/var/log/bad_apple_mlx_server.log` and `/tmp/badapple_menubar.log`.
- Make sure the 7B model is in `~/.cache/huggingface/hub` if you are air-gapped.
- Re-install the menu bar agent:

  ```bash
  /Applications/Bad\ Apple.app/Contents/Resources/install_badapple_platform.sh --install --unsigned-install
  src/platform/apple_desktop/install_menu_bar_agent.sh
  ```

## Feedback and issues

Please file issues, bug reports, and feature requests at:
https://github.com/savageAZfck/bad-apple-releases/issues

See all releases: https://github.com/savageAZfck/bad-apple-releases/releases
