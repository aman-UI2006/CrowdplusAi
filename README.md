# CrowdPulse AI

**Real-time AI crowd monitoring and public-safety prototype**

CrowdPulse AI explores a camera-to-dashboard pipeline using an Android camera client, a FastAPI backend, WebSocket frame transport, YOLO-based person detection, OpenCV heatmaps, crowd-density classification, and alert presentation.

## Core workflow

```text
Android camera
    → frame capture and JPEG encoding
    → WebSocket transport
    → FastAPI session/frame handling
    → YOLO person detection
    → crowd count and density classification
    → OpenCV heatmap
    → dashboard updates and alerts
```

## Capabilities represented in this repository

- Android camera client using Kotlin, Jetpack Compose and Camera2.
- WebSocket streaming of camera frames to the backend.
- FastAPI session management and frame routing.
- YOLO-based person detection and OpenCV heatmap processing.
- Three-level crowd-density classification.
- Dashboard visualization, reconnect behavior and alert presentation.
- Optional AI-generated alert text and browser speech output.
- Backend containerization support.

## Repository layout

- `app/` — Android application source.
- `backend/` — FastAPI server and backend requirements.
- `web/` — web dashboard source and related assets.

## Local development

### Backend

Use Python 3.10+ (confirm the exact supported version against the pinned dependencies).

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r backend/requirements.txt
```

Configure required environment variables locally. Never commit real API keys, tokens, credentials, or service-account files.

The backend Dockerfile is available at `backend/Dockerfile`. Review its build context and required model/assets before building.

### Android

Open the repository in Android Studio, allow Gradle to sync, configure required local settings, then run the app on a compatible device/emulator with camera permission.

## Engineering notes

- Camera frames can be large; production deployments should enforce payload limits, authentication, backpressure, and bounded buffering.
- Real-time inference performance depends on hardware, model weights, frame size, and concurrency. No throughput or latency guarantee is claimed here.
- Treat AI-generated safety messages as advisory. A prototype should not be used as the sole basis for real-world emergency decisions.
- Keep sample input media separate from generated test output. Large generated videos, local environments, IDE state, and diagnostic logs should not be committed.

## Cleanup policy

Python virtual environments, caches, IDE-local state, diagnostic logs, and generated inference output are excluded by `.gitignore`. Sample source media and the Android APK are retained in this cleanup because they may be intentional demo inputs/deliverables.

## Status

This is a development/hackathon prototype. Validate setup instructions, model availability, and end-to-end behavior in your own environment before relying on the application.
