# GeoAI City Backend

FastAPI backend for route, connectivity, busy-intersection, service-reachability, and map analysis using OpenStreetMap data through OSMnx.

## Railway deployment

Railway detects the included `railway.json` start command automatically and installs the root `requirements.txt`. The service listens on Railway's assigned `$PORT`.

Required build/runtime files:

- `main.py` - FastAPI application
- `requirements.txt` - Railway Python dependencies
- `requirements-backend.txt` - equivalent local backend dependency file
- `pyproject.toml` and `osmnx/` - local OSMnx package

The first request for a city downloads its road graph. Graphs are stored in `cache/graphs/`; attach a persistent Railway volume at `/app/cache` if you want to retain them across deployments.

## Render deployment

Render uses the included `render.yaml`, or enter these commands manually:

```bash
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port $PORT
```

Use a Render **Web Service**, not a background worker. Render supplies `$PORT` automatically.

## API endpoints

- `GET /` - health check
- `GET /route?city=...&origin=...&destination=...`
- `GET /connectivity?city=...`
- `GET /bottlenecks?city=...&top_n=5`
- `GET /accessibility?city=...&target=...&sample=10`
- `GET /map?city=...&origin=...&destination=...`

## Local run

```bash
pip install -r requirements-backend.txt
python -m uvicorn main:app --host 0.0.0.0 --port 8000
```