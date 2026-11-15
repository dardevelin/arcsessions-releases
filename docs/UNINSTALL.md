# Uninstalling ARCSessions

ARCSessions puts things in three places. Removing it means removing each, in
this order. Nothing else is installed: no background service, no login item
unless you turned on "Launch at Login", nothing in other apps.

| What | Where | Removed by |
|---|---|---|
| ARC Mic and ARC Speakers (the audio driver) | `/Library/Audio/Plug-Ins/HAL/ARCSessions.driver` | Step 1 |
| The driver's settings file | `/Library/Application Support/ARCSessions/devices.json` | Step 1 |
| The app | `/Applications/ARCSessions.app` | Step 2 |
| Your sessions, presets, recordings and settings | `~/.arcsessions`, and the app's preferences | Step 3, only if you want |

It makes no difference whether you installed from the disk image (and then
installed ARC Mic from the app) or from the Installer package: the same files
are in the same places.

## 1. Remove ARC Mic and ARC Speakers

Skip this step if you never installed them (Settings › Microphone says "Not installed").

In ARCSessions, open **Settings › Microphone** and choose **Remove ARC Mic & ARC Speakers…**.
It says exactly what will be removed, then macOS asks for an administrator
password, once. Sound on the Mac pauses for a moment while macOS unloads the devices.

What happens:

- If ARCSessions is playing the Mac's sound (ARC Speakers is the Mac's output),
  it first puts your own output device back.
- The driver is removed only after three checks: what is at the path is a real
  folder (not a link), it carries the ARCSessions driver's identifier, and it
  is signed by ARCSessions' developer. If any check fails, nothing is removed
  and the message says why.
- `devices.json` is removed, and its folder if that leaves the folder empty.
- The Installer package's receipts for the driver are forgotten, and what an
  early ARCSessions version left behind (an old helper) is removed.
- Your sessions, presets and recordings are not touched.

Apps that were using ARC Mic lose it; choose another microphone in them.

### If the app is already gone, or it refused

These four commands do the same thing by hand. Run them in Terminal; the first
asks for your password.

```bash
sudo rm -rf "/Library/Audio/Plug-Ins/HAL/ARCSessions.driver"
sudo rm -f "/Library/Application Support/ARCSessions/devices.json"
sudo rmdir "/Library/Application Support/ARCSessions"
sudo killall coreaudiod
```

The last one restarts the Mac's audio service (macOS starts it again at once);
sound pauses for a moment. If you installed with the package, also forget its
receipts: `sudo pkgutil --forget com.arcsessions.driver.pkg`,
`sudo pkgutil --forget com.arcsessions.driver-config.pkg`.

If the Mac's output is still "ARC Speakers" in System Settings › Sound, choose
your own speakers or headphones there before removing the driver.

## 2. Remove the app

Quit ARCSessions, then drag **ARCSessions** from the Applications folder to the
Trash. If it was installed with the Installer package, Finder asks for an
administrator password, and you can forget that receipt too:
`sudo pkgutil --forget com.arcsessions.app.pkg`.

If you turned on "Launch at Login", macOS drops the login item when the app is
gone (System Settings › General › Login Items shows it).

## 3. Your data (only if you want it gone)

Your sessions, presets and recordings stay on the Mac after the app is removed,
so installing again brings everything back. To erase them:

```bash
rm -rf ~/.arcsessions
defaults delete com.arcsessions.app
```

The first removes everything ARCSessions stored (`docs/DATA.md` lists each
file). The second removes its preferences. An in-app "Erase All Data" with an
"Export first" option is planned (`data.erase`, docs/DATA.md §5); until it
ships, these two commands are the way.

The permissions you gave ARCSessions stay listed in System Settings › Privacy &
Security (System Audio Recording, Microphone) until you remove them there, or with
`tccutil reset All com.arcsessions.app`.

## Checking that nothing is left

```bash
ls /Library/Audio/Plug-Ins/HAL/ | grep -i arcsessions
ls "/Library/Application Support/" | grep -i arcsessions
pkgutil --pkgs | grep arcsessions
ls -d /Applications/ARCSessions.app ~/.arcsessions
```

Each should print nothing (the last one: "No such file or directory").