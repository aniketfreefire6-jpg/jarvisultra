# JARVIS ULTRA — Starter Project

A safe, extensible starter for a personal assistant spanning Windows 11 and Android.

## Included
- `backend/`: FastAPI service with a WebSocket endpoint and optional OpenAI Responses API integration.
- `windows/`: minimal Python desktop voice/chat client (text input first; microphone integration can be added).
- `android/`: Kotlin/Jetpack Compose starter screen that connects to the backend over HTTP.
- `.env.example`: configuration template.

## Requirements
- Python 3.11+
- Windows 11 for the desktop client
- Android Studio (Giraffe or newer) for the Android project
- Optional OpenAI API key for cloud AI

## Run the backend
```powershell
cd backend
py -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy ..\.env.example .env
# Edit .env and add your API key if you want cloud AI.
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```

The backend health endpoint is `http://127.0.0.1:8000/health`.
The chat endpoint is `POST http://127.0.0.1:8000/chat`.

## Run the Windows client
In a second PowerShell window:
```powershell
cd windows
py -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
$env:JARVIS_API_URL="http://127.0.0.1:8000"
python main.py
```

## Android
Open the `android` folder in Android Studio. Set `BASE_URL` in
`android/app/src/main/java/com/example/jarvis/MainActivity.kt` to your PC's
LAN address, for example `http://192.168.1.20:8000/`. Both devices must be on
the same trusted Wi-Fi network. Add `INTERNET` permission (already included).
For a real deployment, use HTTPS and authentication; do not expose this
development server directly to the public internet.

## Safety
This starter intentionally does **not** run arbitrary shell commands, delete
files, send messages, or silently control devices. Add narrowly-scoped tools
with explicit confirmation before enabling sensitive actions. Keep API keys
only in the backend `.env`, never in the Android app or desktop source.

## Next development steps
1. Add authentication and HTTPS before remote access.
2. Add push-to-talk and speech-to-text.
3. Add a permissioned tool registry for specific PC actions.
4. Add Android `Intent` actions with clear user confirmation.
5. Add wake-word detection only after foreground microphone permissions and
   battery/privacy behavior are designed.
