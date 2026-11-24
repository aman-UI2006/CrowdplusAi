# CrowdPulse AI

**Camera-to-dashboard crowd monitoring prototype**

CrowdPulse AI explores a real-time pipeline that captures camera frames on an Android device, streams them to a FastAPI service, detects people with YOLO, derives crowd-density signals, and presents updates on a browser dashboard.

This is a development/hackathon prototype—not a certified public-safety or emergency-response system.

## System flow

```text
Android camera (Kotlin / CameraX)
        |
        | JPEG frames over WebSocket
        v
FastAPI backend
        |
        +--> YOLO person detection
        +--> Crowd count and density classification
        +--> OpenCV heatmap generation
        |
        v
WebSocket updates
        |
        v
Browser dashboard and alerts
```

## Components

| Component | Location | Responsibility |
|---|---|---|
| Android client | `app/` | Camera capture, frame preparation, and WebSocket client |
| Inference/API service | `backend/` | FastAPI sessions, frame handling, YOLO inference, and heatmap processing |
| Web dashboard | `web/` | Browser-based monitoring interface |
| Architecture notes | `architecture_guide.md` | Component and data-flow explanation |

## Technology

- **Android:** Kotlin, Jetpack Compose, CameraX, OkHttp WebSockets
- **Backend:** Python, FastAPI, Ultralytics YOLO, OpenCV, NumPy
- **Dashboard:** HTML, CSS, and JavaScript
- **Transport:** WebSockets

## Run the backend

### Prerequisites
- Python 3.10+
- A machine with sufficient memory/compute for the selected YOLO model
- Android Studio for the mobile client

### Install dependencies

From the repository root:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r backend/requirements.txt
```

### Start the API

```bash
cd backend
uvicorn server:app --host 0.0.0.0 --port 8000 --reload
```

The backend loads `yolov8m.pt` when running local inference. The optional `ML_NODE_URL` environment variable switches the code into relay mode. Model availability, hardware requirements, and first-run model setup depend on the environment.

## Open the dashboard

From the repository root, serve the static dashboard:

```bash
python -m http.server 5500 --directory web
```

Then open http://localhost:5500. Configure the dashboard and Android client to use the backend host reachable from their devices; `localhost` on a phone refers to the phone itself, not your development computer.

## Run the Android client

1. Open the repository in Android Studio.
2. Allow Gradle to sync.
3. Configure the backend address for the device/emulator environment.
4. Run on a compatible Android device or emulator and grant camera permission.

The repository includes Gradle wrapper scripts, so use `gradlew.bat` on Windows or `./gradlew` on macOS/Linux for Gradle tasks.

## Engineering and safety considerations

- The backend currently keeps session state in process memory; it is not a horizontally scalable session store.
- Configure authentication, strict CORS origins, WebSocket authorization, payload limits, and bounded queues before any public deployment.
- Detection quality and latency depend on camera placement, lighting, model weights, frame size, and hardware.
- AI-generated alert text is advisory. Do not use this prototype as the sole basis for crowd-control, evacuation, or emergency decisions.
- Keep generated videos, model caches, virtual environments, logs, and secrets out of Git.

## Project status

Prototype / active development. Validate end-to-end Android, backend, and dashboard behavior in the target environment before presenting performance claims.

## License

See the repository license file if one is added; no license grant is implied by public visibility.
