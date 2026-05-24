# ZeLing

ZeLing is a private macOS voice productivity app for turning spoken intent into clean text, structured prompts, and target-ready output. It is built for fast daily use: trigger a shortcut, speak naturally, and let ZeLing handle transcription, prompt shaping, target context, and paste/writeback.

This repository is not an open-source distribution. Source access, builds, and test packages are maintained privately by the author.

## What ZeLing Does

- Voice Input: dictate into the current app with optional cleanup, translation, formatting, and paste/writeback.
- Voice-to-Prompt: turn rough spoken intent into polished prompts for AI tools such as ChatGPT, Notion AI, Mail, Obsidian, and other configured Targets.
- Universal Mode: capture selected text plus voice instructions for context-aware rewrite or transformation.
- Target and Scene context: keep output rules, terms, tone, and destination-specific constraints separate from raw dictation.
- Local history and diagnostics: retain recent runs, recordings, and redacted local logs for debugging without a hosted account system.
- Menu bar and HUD workflow: run from global shortcuts with lightweight recording feedback instead of forcing the main window into the foreground.

## Current Distribution Model

ZeLing is currently distributed as private macOS test builds only.

- No public source release.
- No public package registry release.
- Testers receive a signed or ad-hoc signed `.app.zip` build artifact.
- Cloud ASR/LLM traffic goes directly from the tester's device to the configured provider; ZeLing does not operate a hosted model proxy in the current test build.

## Test Build Contents

A test package contains:

- `ZeLing.app`
- bundled UI audio cues for recording start/stop feedback
- bundled local runtime resources required by the packaged app
- app metadata, icons, and notices required for local testing

It does not contain developer logs, local settings, `.env`, secrets, or source control metadata.

## Local Build

Use the existing local packaging script:

```bash
./scripts/build_app.sh
```

The app bundle is written to:

```text
dist/ZeLing.app
```

To create a tester zip without writing to the protected `releases/` directory:

```bash
cd dist
ditto -c -k --sequesterRsrc --keepParent ZeLing.app ~/Downloads/ZeLing-TestBuild.app.zip
```

## Validation Before Sharing

Run the narrowest useful checks for the change being packaged. For a general smoke package:

```bash
python3 scripts/polish_wood_ui_tones.py
plutil -lint App/VoxPrompt.xcodeproj/project.pbxproj
./scripts/build_app.sh
```

Then launch the packaged app on a real macOS desktop and verify:

- microphone permission flow
- global shortcuts
- Voice Input start/stop cue playback
- Voice-to-Prompt generation path
- paste/writeback behavior in at least one target app
- Settings > System recording cue toggle

## Repository Notes

The current product name is `ZeLing`. Some legacy paths, bundle identifiers, and historical files still use `VoxPrompt`; those names are preserved until a deliberate migration is performed.

Do not publish this repository as open source without an explicit release and licensing decision.
