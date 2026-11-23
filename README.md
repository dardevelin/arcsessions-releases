# ARCSessions

**Audio, Routing and Capture Sessions for macOS.** One app to route, mix and capture the sound of every app and device on your Mac.

- **Mix each app on its own fader** and send it where you want: headphones, speakers, a call, a recording.
- **ARC Mic**: a microphone other apps can choose, carrying your mic, your music or a sound clip into a call.
- **Recording tools**: a recorder, a replay of the last minutes, live transcription on your Mac, and sound clips that play when a word is heard.
- **Stream Deck control**: volume dials that follow your mixer, mute keys, push to talk.
- **Private by default**: everything stays on your Mac. No account, no telemetry, nothing about you sent anywhere.

This repository holds ARCSessions' signed releases: the downloads, their checksums and the release notes. The latest is [0.22.0](https://github.com/dardevelin/arcsessions-releases/releases/latest).

## Install

Requires **macOS 14.2 or later**, on Apple Silicon or Intel. Every download is signed with Developer ID and notarized by Apple.

**With Homebrew:**

```bash
brew install --cask dardevelin/tap/arcsessions
```

It installs the app from the disk image and asks for nothing.

**Or download** from the [latest release](https://github.com/dardevelin/arcsessions-releases/releases/latest):

| Download | For | What it does | What it asks |
|---|---|---|---|
| **`ARCSessions-0.22.0.dmg`** | Most people | Open it and drag ARCSessions onto Applications. | Nothing. An administrator password later, only if you choose to install ARC Mic and ARC Speakers. |
| **`ARCSessions-0.22.0.pkg`** | Managed Macs (MDM), or if you prefer Installer | Installs the app, and ARC Mic and ARC Speakers, in one step. The Mac's audio restarts for a moment. | An administrator password, once. |

Neither installs a background service, and neither sends anything over the network.

**ARC Mic and ARC Speakers** are two optional audio devices ARCSessions can add to your Mac. Install them from the app (Settings › Microphone › Install), which asks for an administrator password once.

**Permissions.** ARCSessions asks for **System Audio Recording** (to route and mix the sound of the apps you add; it never sees your screen and never asks for Screen Recording), and for the **Microphone** only the first time you add a microphone.

## Checking a download

Each download has a `.sha256` file beside it in the release. Put both in one folder and run:

```bash
shasum -a 256 -c ARCSessions-0.22.0.dmg.sha256
```

It prints `ARCSessions-0.22.0.dmg: OK`. macOS also checks the signature when you open it; to check it yourself:
`spctl -a -t open --context context:primary-signature -vv ARCSessions-0.22.0.dmg` says *Notarized Developer ID*.

## Updating

ARCSessions can check once a day whether a newer version is out; it asks you once and is off until you say yes. **Help › Check for Updates…** works at any time, and Settings › General › Updates has the daily check and "Include betas". One click downloads the new version, checks its checksum and its signature, asks about unsaved changes, installs it and opens it again. Installed with Homebrew, it runs `brew upgrade --cask arcsessions` for you after quitting; you can also run that yourself.

## Uninstall

1. In ARCSessions, open **Settings › Microphone** and choose **Remove ARC Mic & ARC Speakers…** (if you installed them). Homebrew doesn't remove them.
2. Quit ARCSessions and remove the app: `brew uninstall --cask arcsessions` if you installed it with Homebrew, otherwise drag it from Applications to the Trash.
3. Your sessions, presets and recordings (`~/.arcsessions`) stay, so installing again brings them back. `brew uninstall --zap --cask arcsessions` also removes the app's preferences, caches and saved window state, never your sessions.

The full steps, the commands to do each by hand, and how to check that nothing is left: [docs/UNINSTALL.md](docs/UNINSTALL.md).
