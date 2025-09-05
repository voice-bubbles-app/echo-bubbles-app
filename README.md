# EchoBubble

Turn incoming chat messages into speech using consented cloned voices. Runs locally with your own TTS server (XTTS v2).

## Features
- Consent gate before any synthesis
- First-tap voice picker per sender (remembers)
- Floating bubble overlay to read messages
- Foreground service with proper notification channel
- One-click build + install scripts (Windows/macOS/Linux)

## Requirements
- Android 8+ phone
- A computer (Windows/macOS/Linux)
- Python 3.9+ (to run the local TTS server)
- Android Studio or Android platform tools (for adb)

---

## Ultra-simple: run on your phone

1) Start the TTS server on your PC
- cd server
- python -m venv .venv
- source .venv/bin/activate    (Windows: .venv\Scripts\activate)
- pip install -r requirements.txt
- uvicorn app:app --host 0.0.0.0 --port 8000

2) Connect your phone + enable USB debugging
- On phone: Settings → About phone → tap Build number 7× → Developer options → USB debugging ON
- Plug your phone into your PC via USB

3) Make the phone reach your server (pick ONE)
- USB (easiest): adb reverse tcp:8000 tcp:8000 → base URL: http://127.0.0.1:8000
- Same Wi‑Fi: find your PC IP (e.g., 192.168.1.23) → base URL: http://YOUR_PC_IP:8000
- Online: ngrok http 8000 → base URL: your https URL from ngrok

4) Point the app to your server
- Edit android/app/src/main/java/com/echobubble/tts/Api.kt
  - Set base URL to the one from step 3 (e.g., http://127.0.0.1:8000)

5) Build & install the app
- macOS/Linux: bash scripts/install.sh
- Windows: scripts\install.bat

6) First run on your phone
- Open EchoBubble
- Accept the consent
- Grant Notification Access
- Allow “Draw over other apps”
- On Android 13+: allow Notifications when prompted

7) Create voices (one-time per person)
- In WhatsApp/Signal/Telegram: long-press a voice note → Share → EchoBubble

8) Use it
- New message arrives → a floating bubble shows
- Tap the bubble
  - First time per sender: pick a voice (saved)
  - Message plays in that voice

---

## Test on this PC or online

- Android Emulator (on this PC)
  - Android Studio → Virtual Device Manager → run an emulator
  - In Api.kt set base URL to http://10.0.2.2:8000 (emulator → host)
  - Build & Run from Android Studio
  - For quick overlay testing, use the “Start Bubble (Debug)” button in the app

- Online (any phone)
  - ngrok http 8000
  - Use the https URL in Api.kt
  - Install the APK on any phone and it will reach your local server

---

## Troubleshooting
- Can’t connect to server on phone:
  - Try adb reverse tcp:8000 tcp:8000 and use http://127.0.0.1:8000
  - Or ensure same Wi‑Fi and use http://YOUR_PC_IP:8000
  - If using HTTP on Android 9+, allow cleartext (see Manifest notes)

- No bubble?
  - Grant Notification Access + Draw Over Other Apps
  - Disable battery optimizations for EchoBubble

- Audio slow?
  - CPU-only is slower; a GPU server is much faster

---

## Manifest notes (for HTTP and services)
- Add permissions: INTERNET, SYSTEM_ALERT_WINDOW, POST_NOTIFICATIONS (13+), FOREGROUND_SERVICE (+ mediaPlayback type)
- Allow cleartext for http:// URLs (Android 9+): android:usesCleartextTraffic="true" or a network_security_config

---

## Safety
Use only with explicit consent. Add “Delete voice” and mapping reset if you publish.

## License
MIT
