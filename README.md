# publish-sermon-audio

A Codex skill for preparing sermon audio and publishing it through Spotify for Creators. It preserves the original, shows a timestamped transcript, optionally asks a subagent for boundary suggestions, requires the user to choose the cut, normalizes a copy to -1 dBFS true peak, and requests confirmation before publication.

## Environment and dependencies

- Codex with Agent Skills, local shell/file access, and visible browser automation capable of uploading a local file. Subagent support is optional and enables boundary suggestions.
- `ffmpeg` and `ffprobe` for decoding, trimming, normalization, and verification.
- A timestamped transcription tool. Tested with `whisper-cli` from whisper.cpp and its `ggml-base.en.bin` model; other tools producing SRT/VTT can be used. If Metal initialization fails, use `whisper-cli --no-gpu`.
- A Spotify for Creators account with access to the intended show, internet access, and permission to upload/publish the recording.
- Writable storage for derived audio and transcripts; enough space for decoded WAV files. The validated environment was macOS on Apple Silicon.

On macOS, install audio tools with `brew install ffmpeg whisper-cpp`. Download the Whisper model separately from whisper.cpp's official distribution. Tools and models are not bundled.

## Install and use

Clone this repository into `~/.codex/skills/publish-sermon-audio` (or your configured Codex skills directory), then start a new Codex session if needed for discovery.

Invoke `$publish-sermon-audio` with `raw_audio_file`, `title`, and `description`. Missing values are collected interactively; provide the exact Spotify show before upload. Optional inputs include `target_peak_dbfs`, `suggest_boundaries`, and `publish_timing`. Preparation-only requests stop after delivering the audio.

Authentication uses a browser-managed session/password manager. Enter passwords, 2FA codes, and CAPTCHAs directly in the browser; never put credentials or cookies in this repository. The user selects the final start/end points and confirms the final publish or schedule action.

## Validation

The workflow was checked against two sermon transcripts and nine decision scenarios covering source preservation, boundaries, authentication, upload scope, show selection, prompt injection, and publication confirmation. Those checks do not replace listening to each rendered cut or verifying the current Spotify UI.
