<img width="1912" height="1069" alt="Screenshot 2026-07-18 112749" src="https://github.com/user-attachments/assets/b3a11191-8507-4776-868c-f0f5f4350bc6" />

# Flutter WebSocket Chat Sample

Two clients + one server. Run the server, then launch the Flutter app twice
(two emulators/simulators, or two desktop windows, or one emulator + Chrome)
to chat between the two instances in real time.

## 1. Run the WebSocket server

```bash
cd chat_server
dart pub get
dart run bin/server.dart
```

You should see:
```
Chat WebSocket server listening on ws://0.0.0.0:8080
```

## 2. Run the Flutter app (twice, as two separate "users")

```bash
cd chat_app
flutter pub get
flutter run
```

Run it a second time in another window/device/emulator to simulate the
second chat participant (`flutter run -d chrome`, `flutter run -d macos`,
a second emulator, etc.).

On the login screen, enter a name (e.g. "Alice" on instance 1, "Bob" on
instance 2) and the server URL:

- **Desktop / iOS simulator / web (Chrome):** `ws://localhost:8080`
- **Android emulator:** `ws://10.0.2.2:8080` (emulator's alias for your host machine)
- **Physical device:** `ws://<your-computer-LAN-IP>:8080` (same Wi-Fi network)

Send a message from one instance and it will appear live in the other.

## How it works

- `chat_server/bin/server.dart` — a `shelf_web_socket` server that keeps a
  set of connected clients and broadcasts every incoming JSON message
  (`{"user": "...", "text": "..."}`) to all other connected clients.
- `chat_app/lib/main.dart` — a simple login screen to pick a display name
  and server URL.
- `chat_app/lib/chat_screen.dart` — connects to the WebSocket via
  `web_socket_channel`, sends messages as JSON, listens for incoming
  messages, and renders them as chat bubbles (your own messages on the
  right, others' on the left).

## Notes

- This is a minimal demo: no auth, no persistence, no reconnect logic.
- For a public/production deployment you'd want `wss://` (TLS), message
  validation, and probably rooms/usernames tied to auth.
