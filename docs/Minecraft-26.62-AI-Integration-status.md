# Minecraft Bedrock 1.26.62.1 AI Integration — status

Target package: `com.mojang.minecraftpe`, version `1.26.62.1`, ARM64.

## Current source snapshot

The full source archive is stored in the project's Dropbox folder:

`/Minecraft-26.62-Web-Edition/Minecraft-26.62-AI-Integration-source-v2.zip`

Archive SHA-256: `6cf587eb93b62dd3aa481e1da0ce8ca6dce22053cb9cb6aa455f09a719c26eb7`

This repository branch tracks the integration status only. The proprietary Minecraft APK and its extracted assets are not committed.

## Implemented in source

- Additive Android library module with a manifest-declared lifecycle provider.
- In-Activity AI panel and Qwen OpenAI-compatible Chat Completions client.
- API Key encrypted by Android Keystore AES-GCM.
- Structured world-state contract and autonomous plan / one-action / re-observe loop.
- Bounded action validation and explicit stop behavior.
- ARM64 JNI bridge scaffold with fail-closed behavior while the game ABI remains unmapped.

## Verified facts and blockers

- The original three APK volumes were concatenated in order and passed ZIP integrity validation.
- The original game library `libminecraftpe.so` remains unmodified in the source design.
- The target has PairIP signature / licensing components and v2/v3 signing. No original signing private key or authorized re-signing path has been provided. This project does not remove or bypass those checks.
- Gameplay C++ objects do not appear as a stable public dynamic-symbol API in the inspected library. The native bridge therefore reports `ENGINE_ABI_UNMAPPED` and refuses world read/write operations.
- Official Script API is supported when the world/host loads the behavior pack. Multiplayer scripts execute server-side; a joining client cannot require an unrelated host or Realm to run a locally installed pack. See the source archive's `docs/SCRIPT_API_FEASIBILITY.md`.
- Android AAR compilation and on-device validation are not yet complete. No modified APK is claimed.

## Local build

Inside the extracted source project, with JDK 17, Android SDK 35, NDK 27.2.12479018, CMake 3.22.1 and Gradle 8.10+:

```bash
gradle --no-daemon :app:assembleRelease
python tests/test_action_protocol.py
```
