# KIZAMI DJ User Manual

> Japanese version: [user-manual.md](user-manual.md)

> This manual is written for beginner to intermediate DJs. Technical terms are explained where they first appear.
> Items marked "unverified" could not be confirmed from the program itself.

## Contents

1. [Introduction](#1-introduction)
2. [Screen Overview](#2-screen-overview)
3. [Library](#3-library)
4. [Decks](#4-decks)
5. [Mixer](#5-mixer)
6. [Beat FX](#6-beat-fx)
7. [Recording](#7-recording)
8. [Settings](#8-settings)
9. [Supported MIDI Devices and Control Tables](#9-supported-midi-devices-and-control-tables)
10. [Keyboard Shortcuts](#10-keyboard-shortcuts)
11. [Troubleshooting and Glossary](#11-troubleshooting-and-glossary)

---

## 1. Introduction

### 1.1 What KIZAMI DJ can do

- Import your own music files and automatically analyze BPM (tempo), key, and beat positions.
- Play tracks on two decks (A / B) and blend them by adjusting volume and tone on the mixer.
- Use loops, Hot Cues (mark any position and jump to it instantly), Beat Jump (jump by beats), and SYNC (match BPM).
- Apply Beat FX (echo, reverb, and more).
- Preview the next track in headphones (CUE; requires a 4-channel output device).
- Record your mix to a WAV file.
- Control everything from the keyboard or a MIDI controller.

### 1.2 System requirements

| Item | Details |
|---|---|
| OS | Windows (parts such as the recording destination are built with Windows-only code). The list of supported versions is unverified |
| Supported track formats | MP3, AAC (M4A), FLAC, WAV, AIFF, ALAC |
| Headphone CUE | Requires a 4-channel output device (channels 3-4 are the headphones) |
| Waveform display | Requires an environment where WebGL2 works |
| Beat analysis (AI) | AI is used only when the Beat This! model is installed. Otherwise a simple estimator is used (see 8.1) |
| Required PC performance (CPU, memory, etc.) | Unverified |

### 1.3 Launching and first-time setup

1. Launch KIZAMI DJ. "KIZAMI DJ / Starting…" appears, and after a short wait the main screen opens.
2. On the empty Library screen, press **[Add Folder]** and choose your music folder. Import and analysis start automatically (chapter 3).
3. Check the output device in Settings (the ⚙ at the top right) (8.1).
4. Drag a track row onto a deck to load it, then press PLAY (chapter 4).

If startup fails, the recovery screen appears (8.4).

#### License agreement on first launch

The first time you launch the app, a license agreement screen appears before the main screen. Read it and press **[Accept]**; the version of the text you agreed to is saved in the settings, and the screen will not appear on later launches. If you press **[Decline]**, the app quits (nothing is saved, so the screen appears again the next time you launch). Until you agree, neither on-screen controls nor keyboard shortcuts work. However, in this version, MIDI controller input still reaches the app while it waits for your agreement. If the text of the agreement is revised, even users who have already agreed are asked to agree again at launch.

---

## 2. Screen Overview

```
┌───────────────────────────────────────────────────────────┐
│ TopBar: logo, Master BPM, ● REC, CPU, ⚠ n, CLIP, GR, ⚙   │
├───────────────────────────────────────────────────────────┤
│ Banner (red; shown only when there is a problem)          │
├───────────────────────────────────────────────────────────┤
│ Zoomed waveform A (8 seconds)                             │
│ Zoomed waveform B                                         │
├──────────────┬──────────────────┬─────────────────────────┤
│ Deck A       │ Mixer            │ Deck B                  │
│ (full wave)  │ TRIM/EQ/FILTER   │ (full wave)             │
│ CUE PLAY SYNC│ VOL, CUE         │ CUE PLAY SYNC           │
│ HOT CUE, LOOP│ X-FADER, MASTER  │ HOT CUE, LOOP           │
│ TEMPO        │ HP LEVEL / MIX   │ TEMPO                   │
├──────────────┴──────────────────┴─────────────────────────┤
│ BEAT FX 1 / BEAT FX 2                                     │
├───────────────────────────────────────────────────────────┤
│ Library (folders, playlists, track list)                  │
└───────────────────────────────────────────────────────────┘
```

- If the window is short, the screen scrolls vertically.
- Deck A is pink and Deck B is light blue.
- Keyboard shortcuts are disabled while Settings, the recovery screen, the startup screen, or MIDI Learn is active.

### 2.1 Top bar (TopBar)

| Display | Meaning |
|---|---|
| Master {BPM} | BPM of the master deck |
| ● REC | Start/stop recording (Ctrl+R). Elapsed time is shown while recording. "⚠ Dropped {x}" shows the amount of audio lost because writing could not keep up (chapter 7) |
| CPU bar | Time spent on audio processing (as a ratio of the buffer duration). Warning color above 70%, danger color above 90% |
| ⚠ n | Number of underruns (audio dropouts) |
| CLIP / ⚠ CLIP | Lights for 1 second when the level exceeds 0 dB before the limiter |
| GR | Amount by which the limiter reduced the volume |
| MIDI Learn active [Exit] | Shown only during MIDI Learn (8.3) |
| ⚙ | Opens Settings |

Below the TopBar and the red banner, an update notification bar may appear when a new version is available (2.2).

### 2.2 Update notification

Updates are notify-only; an update is installed only when you press **[Update]**.

| Display | Meaning / action |
|---|---|
| "Version {version} is available" + **[Update]** / **[Later]** | A new version is available. **[Update]** starts the update. **[Later]** hides the notice for that version until the next launch. The supporting text shows the release notes (if there are none, "Updating will restart the app") |
| "Installing update ({version})" | The update is being installed. "The app will restart when it finishes. Please wait." is shown; wait until it finishes and the app restarts |
| "Could not check for or apply the update" | Checking for the update, or installing it, failed. The reason is shown in the same bar. If installing failed, the update remains available, so you can press the button again |

---

## 3. Library

This is the screen for managing tracks. The folder/playlist tree is on the left and the track list is on the right.

### 3.1 Importing (folders)

- Add a music folder with the **＋** next to "Folders" in the tree, or with **[Add Folder]** on the empty Library ("Your library is empty. Add a music folder to start importing and analyzing.").
- To add just a few tracks, press **♪＋** next to "Folders" and choose audio files (you can select several). Only the chosen tracks are imported; their folder is not watched. Dragging files in from Explorer is not supported yet.
- Import starts when you add a folder, and "Importing done / total", a progress bar, and **[Cancel]** appear.
- When it finishes, "Import finished" and "Added n, skipped n, failed n" appear. If you cancel, "Import cancelled" appears.
- You cannot register a folder that is inside, or that contains, an already registered folder.
- To remove a folder, use **[Remove]** on the General tab in Settings (8.1). Removing a folder only takes it out of the Library; the track files are not deleted.

### 3.2 Tree

| Item | Details |
|---|---|
| All Tracks (n) | Shows all tracks |
| Folders | Filter by each registered folder |
| Playlists | Always shown. Indented by playlist folder |
| rekordbox | Import a rekordbox XML (3.2.2) |

#### 3.2.1 Editing playlists

- Use the **＋** next to "Playlists" to create a new playlist, or **📁** to create a playlist folder. Type a name and press Enter (Esc cancels).
- When you hover over a row, **✎** (rename; F2 also works) and **🗑** (delete; Delete also works) appear. Deleting asks for confirmation. The tracks themselves are not deleted.
- Drag a playlist or folder row in the tree onto another folder row to move it inside. Dropping it on the empty area of the playlist list moves it to the top level. It cannot be moved into itself or into its own contents.
- Drag a row from the track list onto a playlist in the tree to append it to the end (the same track can be added any number of times).
- While a playlist is open in its own order, drag rows to reorder them, and use the row action to remove a track from it (the track itself is not deleted).

You can also work from the toolbar above the track list.

| Control | Details |
|---|---|
| Add to playlist (selector) | Appends the tracks selected in the track list to the end of the chosen playlist. The selector initially shows "Choose…", or "(No playlists)" if there are none. It is unavailable when no track is selected or there are no playlists. Only playlists can be chosen as a destination, not playlist folders |
| **[↑ Up]** / **[↓ Down]** | Moves the selected track up/down by one. Shown only while a playlist is open. Up is disabled for the first track, Down for the last |
| **[Remove from playlist]** | Removes the selected track from the playlist (the track itself is not deleted). Shown only while a playlist is open |

- Reordering (Up / Down, dragging rows) and Remove work only when the search box is empty and the playlist is open in its own order. Otherwise the three buttons are disabled and "Reordering works only with an empty search while viewing in playlist order" is shown.

#### 3.2.2 Importing a rekordbox XML

- Under "rekordbox" in the tree, press **[Import…]** and choose an XML exported from rekordbox.
- The confirmation includes **"Overwrite existing cues and manually edited values with the XML contents"**, which is off by default. When off, existing cues and manually corrected beats and keys are kept, and only missing data is added. When on, they are replaced with the contents of the XML.
- When it finishes, a notification shows the number of tracks, playlists, and cues added. If some files were not found, that count is shown too.
- For AAC tracks (such as m4a), the start offset from the export source (about 34 ms) is corrected automatically when beat positions are imported.

### 3.3 Search

- Type in the search box (Ctrl+F jumps to it); the search runs 150 ms after you stop typing. Enter searches immediately.
- Esc leaves the search box. It does not misfire while you are composing text with a Japanese input method.

### 3.4 Sorting

- Click a column header to sort by that column. Click again to reverse the order.

### 3.5 Columns

| Column | Meaning |
|---|---|
| (Deck column) | The deck the track is loaded on (see "Reading the track list" below) |
| Title / Artist | Track title and artist |
| BPM | Track speed (beats per minute). "—" if not analyzed |
| Key | Musical key of the track, in Camelot notation (see "Key notation" below). "—" if not analyzed |
| Time | Track length |
| Rating | Shown as ★/☆ (5 levels). Display only; clicking does not change it |

- A maximum of 1000 tracks are shown. Beyond that, "Showing the top 1000 tracks. Narrow down with search." appears.
- Click a row to select it; drag it to load it onto a deck (4.1).

### 3.6 Analysis and row states

When you import a track, its BPM, key, and beat positions are analyzed automatically.

| Display | Meaning | What to do |
|---|---|---|
| ⏳ (waiting for analysis / analyzing) | Waiting its turn for analysis, or being analyzed | Wait |
| ⚠ (analysis failed) + **[Re-analyze]** | Analysis failed | Press **[Re-analyze]** |
| (missing) | The file was moved or deleted. The row cannot be dragged | Put it back in its original location, or register the folder again |

- If you see "Automatic analysis stopped" or "{n} analyses timed out. You can still use [Re-analyze] on each track", automatic analysis has stopped. The per-track **[Re-analyze]** still works.
- Tracks that cannot be analyzed can still be loaded onto a deck and played. However, without beat analysis, SYNC, loops, and Beat Jump cannot be used (4.8).

### 3.7 Reading the track list

#### Deck column (A / B)

- The column to the left of the title shows which deck the track is on. Deck A is in pink text, Deck B in light blue.
- While playing, a ▶ is added (such as "▶A") and the text is bold and glowing. If the track is merely loaded and stopped, only colored text and an outline are shown.
- If the same track is on both decks, both A and B are shown.
- The row of a loaded track has a deck-colored bar at its left edge, and its title is bold.
- Tooltip: hovering shows "Playing on Deck A" or "Loaded on Deck A".

#### ★ mark (key-compatible tracks)

- A ★ is added to tracks whose key (Camelot notation) is compatible with the track on the master deck. It marks tracks that are likely to sound good when mixed. On a ★ row, the title and key cells are shown in green.
- The "master" is the deck used as the reference for BPM and key. When the master changes, the ★ marks move accordingly.
- Hovering over the ★ shows "Compatible with the master key".

##### About key notation (8A, 7B, and so on)

The "8A", "7B", etc. in the Key column are **Camelot wheel** codes for the key (musical scale) of the track. DJs use them to tell which tracks mix easily.

- The number (1-12) is the position on a circle that arranges keys in fifths (the circle of fifths). Like a clock face, 12 is followed by 1.
- The letter: A is a minor key (darker sound) and B is a major key (brighter sound).
- Examples: 8A = A minor (Am), 8B = C major (C). 7A = D minor (Dm), 9A = E minor (Em).
- Keys that are neighbors on the wheel, or the same number with A/B swapped, tend to stay clean when blended and are easy to mix.

##### When ★ is added

A ★ is added to a track whose key has one of the following relationships with the master's key.

| Condition | When the master is 8A |
|---|---|
| Same key | 8A |
| Same number, only A/B differs (relative key) | 8B |
| Same A/B, number ±1 (12 and 1 are neighbors) | 7A, 9A |

- Example: if the master is 8A, ★ is added for four keys: 8A, 8B, 7A, and 9A. If the master is 12A, they are 12A, 12B, 11A, and 1A.
- Keys that are ±1 but with a different A/B (7B, 9B, etc.) and keys 2 or more steps away do not get a ★.
- The master track itself also gets a ★, because its key is the same.

##### When ★ is not added

- No track is loaded on the master deck.
- The key of the master's track has not been analyzed.
- The key of that track has not been analyzed, or is not in the "1A" to "12B" format.

(Implementation: `isCompatibleKey` in `ui/src/lib/camelot.ts`)

---

## 4. Decks

How to use the two decks (A / B) on the left and right of the screen.

### 4.1 Loading a track

| Method | Operation |
|---|---|
| Drag | Drag a track row from the Library onto a deck |
| Shortcut | Select a track in the Library, then Shift+← (Deck A) / Shift+→ (Deck B) |

- An empty deck shows "No track loaded (drag one from the library, or press Shift+←)" (Shift+→ for B).
- While loading it shows "Loading…". If loading fails, the cause and a remedy are shown.
- The full waveform loads in two stages, starting from the beginning. Parts not yet loaded are shown dark.
- Files that are not found cannot be loaded.

### 4.2 Deck display

- Header: deck name, title, artist. The master deck shows "MASTER", and a deck that is master automatically shows "MASTER (auto)".
- Readouts: BPM, key, elapsed time, and remaining time (with a minus sign). "⏳ Analyzing" and "⚠ Analysis failed" can also appear.

### 4.3 Play, stop, and CUE

| Button | Behavior |
|---|---|
| PLAY (▶ / ❚❚) | Play / pause |
| CUE | When stopped, sets a Cue point at the current position. It sounds only while held, and returns to the Cue point when released (hold style) |
| SYNC | Matches the other deck's BPM (4.7). Cannot be pressed without beat analysis |
| M | Makes this deck the master. Press again to return the master to automatic |

### 4.4 Hot Cue

A feature for marking any position in a track and jumping straight to it. Each deck has 8 of them, numbered 1-8.

| State | Operation | Behavior |
|---|---|---|
| Empty | Press | Records the current position |
| Set | Press | Plays from that position (hold style: only while held) |
| Set | Shift+press | Deletes it |

- Hovering over a button shows its position and key.

### 4.5 Loops

Plays a chosen section repeatedly. Requires beat analysis.

| Operation | Details |
|---|---|
| Auto loop ½ / 1 / 2 / 4 / 8 | Creates a loop of that many beats from the current position |
| ½× / 2× | Halves / doubles the length of the current loop |
| IN / OUT | Set the loop start / end by hand. Pressing IN enters "waiting for OUT" |
| ⟲ | Exits the loop. While "waiting for OUT", cancels Loop In |
| SLIP | When on, the playback position keeps advancing in the background during loops and scratches, and returns to that position when they end |

### 4.6 Beat Jump

- ◀◀ ▶▶ move back / forward by the chosen number of beats. Choose 1 / 2 / 4 / 8 / 16 beats (default 4).
- Requires beat analysis. The keyboard Beat Jump also uses the beat count chosen on this screen.

### 4.7 Tempo, pitch, SYNC, and master

Terms: **tempo** is the playback speed of a track, and **pitch** is the height of the sound. Normally, changing the speed also changes the pitch.

- **TEMPO fader** (on the outer edge of the deck; left for A, right for B): moving it down speeds up (+), up slows down (−). Double-click to reset to 0%.
- The current value is shown in %. Each press of the range button cycles ±8% → ±16% → ±50%.
- **KEYLOCK**: keeps the pitch the same when you change the tempo.
- **BEND**: while the speed is temporarily shifted with the jog wheel (MIDI controller), "BEND +x.x%" is shown.
- **SYNC**: automatically matches the other deck's BPM. Requires beat analysis.
- **Master**: the deck used as the reference for BPM and key. Set it manually with the M button; releasing it returns it to automatic. It is also the reference for the ★ mark (3.7).

### 4.8 When there is no beat analysis

The deck shows "⚠ No beat grid analysis(SYNC, loops and Beat Jump are unavailable)". Analysis of the track has not finished or has failed. Try **[Re-analyze]** in the Library (3.6).

### 4.9 Waveforms

| Waveform | Details |
|---|---|
| Zoomed waveform (top of screen) | 8 seconds with the playback position at the center. Beat grid, loops, Cue, and Hot Cues are overlaid |
| Full waveform (in the deck) | The whole track. Click to move to that position |

- If WebGL2 is unavailable, "Cannot display the waveform (WebGL2 is unavailable)" is shown; if drawing fails, "Cannot display the waveform (drawing failed)" is shown.

---

## 5. Mixer

The mixer in the center of the screen is made up of two channels (A / B) and a shared section.

### 5.1 Knob and fader controls

| Operation | Behavior |
|---|---|
| Drag vertically | Changes the value (about 200 px for the full range) |
| Shift+drag | Fine adjustment |
| ← ↑ → ↓ | Change in fixed steps (finer with Shift) |
| Home / End | Minimum / maximum |
| Double-click, Delete, Backspace | Reset to the default value |
| Alt+click, or Enter while an EQ is focused | Toggle KILL (5.3) on/off |

### 5.2 TRIM and auto gain

- **TRIM**: a knob that adjusts the input level (±12 dB). Use it to even out differences in recording level between tracks. Double-click for 0 dB.
- "Auto x dB" is the auto-gain correction amount. Auto gain matches the loudness of tracks to −14 LUFS (a unit of perceived loudness), and can be turned on/off in Settings (8.1). TRIM is a manual adjustment and is applied on top of the automatic correction.

### 5.3 EQ (HI / MID / LOW) and KILL

- **EQ**: splits the sound into highs (HI), mids (MID), and lows (LOW), and raises or lowers each. The range is −40 dB to +6 dB. Double-click for 0 dB.
- **KILL**: cuts that band completely. "KILL" is displayed. Alt+click again to restore it.

### 5.4 FILTER

- One knob switches between two kinds of filter. Turn it left for LPF (low-pass = cuts the highs) and right for HPF (high-pass = cuts the lows). The center is OFF.
- The display reads "LPF n%", "HPF n%", or "OFF". Double-click for OFF.

### 5.5 Channel fader (VOL) and meter

- The VOL fader adjusts the channel volume (shown in %). The meter beside it shows the peak level.
- The meter starts at −48 dBFS. It turns the warning color above −6 dB and the danger color above −0.3 dB (on the verge of clipping).

### 5.6 CUE (headphones)

- The **CUE A / CUE B** button on each channel lets you listen to that channel in headphones (it is not sent to the main output).
- It works only with a 4-channel output device. When unavailable, the button cannot be pressed and "No headphone output (channels 3-4)" is shown.

### 5.7 Crossfader (X-FADER)

- Move it left and right to switch the volume between A and B. The display reads "CENTER", "A n%", or "B n%". Double-click for center.
- The curve (smooth / cut) can be changed in Settings (8.1).

### 5.8 MASTER

- The vertical fader adjusts the overall output volume. The range is 0 to +6 dB. Double-click for 0 dB. Left and right (L/R) meters are attached.
- If you turn on the master limiter in Settings, it prevents clipping (8.1). You can see whether the limiter engaged in CLIP / GR in the TopBar.

### 5.9 Headphones (HP LEVEL / HP MIX)

| Knob | Details |
|---|---|
| HP LEVEL | Headphone volume (0-2, default 1) |
| HP MIX | The balance of what you hear in the headphones. Fully left is CUE only; fully right is MASTER only. The default is the center (0.5) |

- When there is no headphone output, "⚠ No headphone output (channels 3-4)" is shown.

---

## 6. Beat FX

Effects that follow the beat of the track. There are two units, BEAT FX 1 and 2.

| Item | Details |
|---|---|
| Type | ECHO / REVERB / FLANGER / FILTER |
| Beats | Choose with ◀ ▶ from 1/16, 1/8, 1/4, 1/2, 3/4, 1, 2, 4, 8, 16, 32, 64. The length of the effect's cycle |
| MIX | How much effect is applied (wet/dry). Default 50%, in steps of 5% |
| Target | Choose A / B / M (the whole master) |
| BPM display | The BPM the effect is based on |
| ON / OFF | Turns the effect on/off |

---

## 7. Recording

Records the final output of the mix (the MASTER sound) to a WAV file.

| Item | Details |
|---|---|
| Start / stop | ● REC in the TopBar, or Ctrl+R |
| Save location | `KIZAMI DJ\Recordings\` in your Documents folder |
| File name | `YYYYMMDD-HHMMSS.wav` (example: 20261022-213045.wav). If the name already exists, `-2`, `-3` is appended so nothing is overwritten |
| Format | WAV (24-bit) |
| Display while recording | Elapsed time on the REC button |
| When stopped | "Recording saved" and the save location are shown |

- If writing cannot keep up and audio is lost, "⚠ Dropped {x}" is shown.
- If writing fails, a red banner "Recording stopped: ..." appears and the recording up to that point stays in the file. Dismiss the banner with **[Close]**.
- Even if you have moved your Documents folder to OneDrive or elsewhere, the recording is saved in `KIZAMI DJ\Recordings\` inside that location.
- The maximum recording length and file size limit are unverified.

---

## 8. Settings

Open with the **⚙** at the top right. Esc closes it. There are three tabs: General, Shortcuts, and MIDI.

### 8.1 General tab

#### Output

| Item | Details |
|---|---|
| Output device | The device that plays the sound. Choose from "Default device", "{name} (default)", and "{name} (not found)" |
| Current: ... | The device in use, its channel count, and buffer size |
| Auto gain (match track loudness to −14 LUFS) | Matches track loudness to −14 LUFS (on by default) |
| Master limiter (prevents clipping) | Prevents clipping (on by default) |
| Crossfader curve | Smooth (default) / Cut (a small move from either end fully opens the opposite side, and it cuts only at the end; suited to scratching) |

- If the channel count is less than 4, "Headphone output requires a 4-channel device (currently Master only)" is shown.

#### Library folders

- A list of registered folders. Press **[Remove]** on a row, then **[Really remove]** in the confirmation, to remove it (track files are not deleted).
- Add folders with **[Add folder]**. Removing a folder only removes it from the library; your music files are not deleted.

#### Beat analysis

- It shows "Using Beat This! (AI beat detection)." or "Using the simple estimator." In the simple case, the reason the AI cannot be used is also shown.
- Specify the model folder with **[Choose folder]** and revert with **[Reset to default]**.

### 8.2 Shortcuts tab

- Press **[Change]** on an action's row, then press the key you want to assign. Esc cancels.
- Usable keys: a single key, or Shift+key, Ctrl+key. Hot Cues allow only keys without modifiers (because Shift+key is used for deletion).
- If you choose a key that is already in use, "{combo} is already used by {name}. Swap them?" is shown. Choose **[Swap]** or **[Cancel]**.
- Each row has **[Reset]**, and there is **[Reset all to default]** for everything.
- Keys that cannot be assigned: Tab, Escape, CapsLock, NumLock, kana, a modifier key alone, combinations with Alt or Win, Shift+Ctrl together, and Ctrl+C/V/X/A/Z/Y. Numpad digits cannot be assigned to Hot Cues, and cannot be combined with Shift.
- Key labels are shown as US-keyboard engravings (for example -, =, `, ', Numpad1).

### 8.3 MIDI tab

Settings for using a MIDI controller. For supported devices and control tables, see chapter 9.

#### Devices and mappings

- A list of connected MIDI devices is shown. A device may not appear right after you connect it, so wait a moment.
- Status per device: "Connected", "Not connected", "Cannot open (another app may be using it)".
- Use the **Mapping** selector to choose the mapping used for that device. You can also choose "No mapping". Mappings not yet checked on real hardware are labeled "Untested".
- The table below shows the mapping's Action / MIDI / Type (button, knob, jog, 14-bit). The MIDI column reads type, channel, number, for example "Note ch0 #11" (note; channel 0, number 11), "CC ch0 #20" (control change; channel 0, number 20), or "14bit CC ch0 #msb/lsb".
- If a mapping has rows that cannot be read, a "⚠ ..." row appears. If the mapping itself cannot be read, "Could not read this mapping: ..." is shown.
- When the state changes, the screen shows "MIDI device connected: {name}", "MIDI device disconnected: {name}", or "Cannot open MIDI device: {name}".

#### MIDI Learn (assign buttons yourself)

A feature that lets you link on-screen controls to your controller's knobs and buttons yourself.

1. Press **[Start Learn]**.
2. "Click the control you want to assign (Esc to exit)" -- click a control on the screen (button, knob, etc.).
3. If the control has several actions, "Which action do you want to assign?" appears; choose one.
4. "{action}: Move a control on your controller" appears. Operate the knob or button you want to use. **[Cancel]** aborts.
5. If it is already assigned to another action, "{midi} is already assigned to {action}. Replace it?" appears ({midi} looks like "Note ch0 #11"). Choose **[Replace]** or **[Keep]**.
6. "{midi} assigned to {action}." appears, and the type (button, knob, jog, 14-bit) is shown. Choose **[Redo]** or **[Next control]**.
7. If no message arrives, "No message received." appears. Choose **[Try again]** or **[Cancel]**.

- During Learn, controls that can be assigned get an outline. Finish with **[Exit]** in the TopBar or with Esc.

#### Jog wheel (scratch and pitch bend)

- Touch the top surface of the jog wheel and turn it: scratch (the sound moves back and forth as if you were moving a record by hand). One revolution advances 1.8 seconds.
- Turn it without touching the top (the side or top surface): pitch bend (temporarily shifts the speed so you can match beats). One revolution per second gives +5%, up to ±10%. When you stop turning, it returns to 0 in about 0.1 seconds. The amount is shown as "BEND +x.x%".

### 8.4 Banners, notifications, and warnings: meaning and fixes

#### Banners (red bar at the top of the screen)

| Display | Meaning | What to do |
|---|---|---|
| Audio output: {device} was disconnected / Reconnecting to the same device | The audio output device was unplugged | When you reconnect it, playback resumes at the same position. If it does not come back, choose another device in Settings |
| Output lost (an error and remedy are shown) | Output stopped | Follow the remedy shown. If it does not recover, restart KIZAMI DJ |
| Recording stopped: ... | Writing the recording failed | The recording up to that point remains at the path shown. Check free space and the save location. Dismiss with **[Close]** |

#### Notifications (messages shown on screen for a few seconds)

Notifications disappear after 6 seconds (errors stay until closed). Up to 4 are shown at once.

| Display | Meaning |
|---|---|
| Importing {folder} / Import finished / Import cancelled | 3.1 |
| Automatic analysis stopped / {n} analyses timed out | 3.6 |
| {saved device} not found. Playing through the default device | The previous output device was not found, so playback switched to the default device. You can choose again in Settings |
| Recording saved | Chapter 7 |
| MIDI device connected / disconnected: {name}, Cannot open MIDI device: {name} | 8.3. For "Cannot open", check whether another app is using it |
| Mapping {name} has lines that could not be read | 8.3. Rows that cannot be read are not used |

#### Recovery screen (at startup)

- "The library database is corrupted": choose the backup to restore (newest first) and press **[Restore and start]**. To quit, press **[Quit]**. If there is no backup, "No backups are available to restore." is shown.
- "KIZAMI DJ could not start": **[Start again]** or **[Quit]**.
- This screen is not closed by Esc.

#### Error hints (text shown on screen)

| Situation | Guidance shown |
|---|---|
| The library is in use | Check that KIZAMI DJ is not already running in another window |
| The library is from a newer version | Open it with a newer version of KIZAMI DJ |
| The track file cannot be read | Check whether it was moved or deleted, whether another app is using it, and permissions |
| Unsupported format | Supported formats are MP3, AAC (M4A), FLAC, WAV, AIFF, and ALAC |
| The track is corrupted | Check that the file is not corrupted and that it plays in another player |
| The track is too long | Tracks that are too long cannot be loaded (the maximum length is unverified) |
| Out of memory | Close other apps to free up memory, then try loading again |
| Output device not found | Check the device connection and power, or choose a different output device in Settings |
| Internal error | Restart KIZAMI DJ. If the problem continues, please contact the developer |

---

## 9. Supported MIDI Devices and Control Tables

### 9.1 Support status

| Device | Status |
|---|---|
| Numark MixTrack Pro | Partly verified on real hardware (9.3) |
| Pioneer DJ DDJ-FLX4 | **Not verified on real hardware.** The mapping was transcribed from the official MIDI message list. It may not work or may be off |
| Other devices | No mapping is included. Assign controls yourself with MIDI Learn (8.3) |

- You can change the mapping from the selector on the tab. The bundled mappings are read-only.
- On Windows, only one app can open a given MIDI port. Close other DJ software and the like.
- Even similar models (such as MixTrack Pro 3) may use different MIDI numbers. In that case, choose a mapping again on the MIDI tab, or assign controls with MIDI Learn.

### 9.2 DDJ-FLX4 (not verified on real hardware)

These are the actions included in the bundled mapping. In particular, "SYNC", "TEMPO", "Jog (top surface / side)", and "effect amount" were already marked unverified at the time of transcription. **Everything in the table is unverified on real hardware.**

| Category | Actions |
|---|---|
| Playback | PLAY/PAUSE, CUE, SYNC |
| Hot Cue | Set / play 1-8, delete 1-8 (Decks A and B) |
| Loop | Auto loop, Loop IN / OUT, ½, ×2 |
| Beat Jump | Back, forward |
| Mixer | TRIM, EQ HI / MID / LOW, FILTER, channel fader, CUE (headphones), crossfader, MASTER, headphone MIX / LEVEL |
| Tempo / jog | TEMPO fader (14-bit), jog (top surface / side), jog touch |
| Beat FX | ON (units 1 and 2), next / previous type, beats down / up, amount |
| Library | Browse (select a track), load to Deck A / B |
| LED (lighting on the device) | Playing, Cue set, SYNC on, in loop, Hot Cue set |

Main numbers (MIDI notes; from the bundled mapping):

| Action | Number |
|---|---|
| PLAY (A) | Channel 0, note 11 |
| CUE | Note 12 |
| SYNC | Note 88 (unverified) |
| Hot Cue 1-8 | A: channel 7, B: channel 9, notes 0-7 |
| Hot Cue delete | A: channel 8, B: channel 10 |
| Auto loop / IN / OUT / ½ / ×2 | 77 / 16 / 17 / 81 / 83 |
| Beat Jump back / forward | Channels 7 and 9, notes 32 / 33 |
| CUE (headphones) | Note 84 |
| Jog touch | Note 54 |
| Beat FX ON | Channels 4 and 5, note 71 |
| FX type next / previous | 99 / 100 |
| FX beats down / up | 74 / 75 |
| Library load | Channel 6, note 70 (A) / 71 (B) |

### 9.3 MixTrack Pro (partly verified on real hardware)

For MIDI devices whose name contains "MixTrack Pro", the bundled mapping (USB VID_15E4 / PID_0077) is selected. Everything uses MIDI channel 0.

| Action | Deck A | Deck B | Notes |
|---|---|---|---|
| Jog | CC25 | CC24 | 600 pulses per revolution |
| Jog touch | Note 78 | Note 77 | |
| PLAY/PAUSE | Note 59 | Note 66 | |
| CUE | Note 51 | Note 60 | |
| SYNC | Note 64 | Note 71 | |
| Pitch fader | CC13 | CC14 | Top is −8%, bottom is +8% |
| Channel fader | CC8 | CC9 | |
| EQ HI | CC16 | CC17 | |
| EQ MID | CC18 | CC19 | |
| EQ LOW | CC20 | CC21 | |
| Crossfader | CC10 (shared) | | |
| MASTER GAIN | CC23 (shared) | | May fluctuate around 45-47 even when untouched |

Verification status on real hardware (from the developer's records):

| Item | Status |
|---|---|
| Jog scratch (touch the top and turn) | Verified |
| Jog bend (turn without touching) and on-screen display | Verified |
| PLAY / CUE / SYNC | Verified |
| EQ, MASTER GAIN, channel fader | Verified |
| Direction of the pitch fader | Verified |
| Direction of the crossfader | Verified |
| Noise when the jog is spun quickly in reverse | Unverified |
| Feel when stopping the jog while touching it | Unverified |
| CUE GAIN / CUE MIX, loading tracks, loops, effects | Mapping not created (not in the bundled mapping; assign with MIDI Learn) |

Note: if an old settings file (`%APPDATA%\KIZAMI DJ\mappings\mixtrack-pro.json`, one containing only the jog) remains, it is chosen before the bundled one, and SYNC, playback, and faders stop working. Delete that file, or choose "MixTrack Pro (Default)" on the MIDI tab.

---

## 10. Keyboard Shortcuts

These are the defaults. You can change them on the Shortcuts tab in Settings (8.2). The action names below match those shown in the app (for example "Deck A Play"). Names such as "Deck A Auto Loop" are shown in the app exactly as written.

### 10.1 Decks

| Action | Deck A | Deck B |
|---|---|---|
| Play (Deck A Play / Deck B Play) | Z | M |
| Cue | X | N |
| Sync | C | B |
| Auto Loop (4 beats; exits the loop if in one) | S | K |
| Beat Jump Back | A | J |
| Beat Jump Forward | D | L |
| Hot Cue 1-4 | 1 2 3 4 | 7 8 9 0 |
| Hot Cue 5-8 | Q W E R | U I O P |
| Hot Cue delete | Shift+the Hot Cue key | Shift+the Hot Cue key |

### 10.2 Library and other

| Action | Key |
|---|---|
| Library Select Up / Library Select Down | ↑ / ↓ (hold to repeat) |
| Library Load to Deck A | Shift+← |
| Library Load to Deck B | Shift+→ |
| Library Search | Ctrl+F |
| Recording Start/Stop | Ctrl+R |

### 10.3 Rules

- Cue and Hot Cue are hold style: they sound only while the key is held and stop when released.
- While you are typing text, shortcuts do not work (except those using Ctrl).
- Ctrl+R and Ctrl+F work as KIZAMI DJ functions, not as the screen reload or the browser search.
- Nothing happens on a deck with no track loaded. For a track without beat analysis, SYNC, loops, and Beat Jump also do not work.
- The Beat Jump beat count for the keys uses the value chosen on screen (4.6).

---

## 11. Troubleshooting and Glossary

### 11.1 Troubleshooting

| Symptom | What to check |
|---|---|
| No sound | (1) The output device in Settings is correct (2) PLAY is pressed on the deck (3) The VOL fader and MASTER are not pulled down (4) The crossfader is not pushed to the opposite side (5) EQ KILL is not on (6) FILTER is not turned all the way (7) No red banner (output disconnected) is shown |
| Cannot hear in headphones | Whether the output device is 4-channel (check the channel count in "Current: ..." in Settings). With 2 channels, CUE is unavailable. Also check HP LEVEL and HP MIX |
| Sound breaks up | Look at the CPU bar and ⚠ n in the TopBar. If they are high, close other apps. You can check the output device's buffer size in "Current: ..." in Settings (how to change it is unverified) |
| Sound is distorted | Lower MASTER and the channel VOL. Turn on the master limiter. Keep the meters from going red |
| SYNC, loops, and Beat Jump do not work | The track has no beat analysis (4.8). **[Re-analyze]** in the Library |
| Analysis never finishes / fails | See 3.6. **[Re-analyze]**. Check that the track format is supported (1.2) |
| Track shows "(missing)" | Put the file back in its original location, or register the folder again |
| Cannot load a track | See the error hints table in 8.4 |
| MIDI device does not appear / a warning appears | (1) Cable and power (2) Wait a moment (3) Whether another app is using the MIDI port (on Windows, only one app can) (4) Choose the mapping again |
| Only some actions work over MIDI | Whether an old mapping file remains (note in 9.3) |
| Cannot start | Follow the recovery screen in 8.4 |
| No ★ appears | "When ★ is not added" in 3.7 |
| Recording stopped | Chapter 7. The recording up to that point remains at the path shown in the banner |
| Want to report a problem | **[Open logs folder]** in Settings opens the folder with the activity logs (`kizamidj.date.log`). Only the newest 7 days are kept, and track names and file locations are not written. Attach the latest file when you report. For a detailed investigation, start with the environment variable `KIZAMIDJ_LOG=debug` to get detailed logs that include track locations (check the contents before sharing them) |

### 11.2 Glossary

| Term | Meaning |
|---|---|
| BPM | Beats per minute. The unit of a track's speed |
| Camelot | A notation that expresses a track's key as 1A-12B. Neighboring keys are easy to match (3.7) |
| Beat grid | A scale showing where the beats of a track fall. The foundation of SYNC, loops, and Beat Jump |
| Deck | One player that plays a track |
| Mixer | The device (screen) that adjusts the volume and tone of two decks and blends them |
| Master | The deck used as the reference for BPM and key. Or the final output |
| SYNC | A feature that matches the BPM (and beats) of the two decks |
| CUE | (1) A position to start a track from, or the operation of returning to it (2) The mixer's CUE button: listen to that sound in headphones |
| Hot Cue | A mark placed at any position. A button jumps to it instantly |
| Loop | Playing a section repeatedly |
| Beat Jump | Jumping forward or back in units of beats |
| Scratch | Playing by moving the sound back and forth as if moving a record by hand |
| Pitch bend | Temporarily shifting the speed to match the beats of two tracks by hand |
| KEYLOCK | Keeping the pitch unchanged when the tempo changes |
| TRIM | Input-stage volume. Evens out volume differences between tracks |
| Auto gain | A feature that automatically matches track loudness to −14 LUFS |
| LUFS | A unit of loudness as heard by the human ear |
| EQ (equalizer) | A feature that raises or lowers highs, mids, and lows individually |
| KILL | Cutting a band completely |
| FILTER | A feature that cuts the lows or highs (LPF / HPF) |
| LPF / HPF | Low-pass (cuts highs) / high-pass (cuts lows) |
| Crossfader | Switches the volume between A and B with a single horizontal fader |
| Limiter | A feature that automatically holds the level below a set volume to prevent clipping |
| Beat FX | Effects applied in time with the beat (ECHO, REVERB, FLANGER, FILTER) |
| MIDI | A communication standard connecting instruments and controllers to a computer |
| MIDI Learn | A feature for assigning a controller's knobs and buttons to on-screen controls yourself |
| Jog | The controller's rotating platter. Used for scratching and pitch bend |
| Underrun | Audio processing could not keep up, so the sound breaks up |
| Beat This! | The name of the AI model that estimates beats |
