# GeoAI City Backend

FastAPI backend for route, connectivity, busy-intersection, service-reachability, and map analysis using OpenStreetMap data through OSMnx.

## Railway deployment

Railway detects the included `railway.json` start command automatically. The service listens on Railway's assigned `$PORT`.

Required build/runtime files:

- `main.py` - FastAPI application
- `requirements-backend.txt` - Python dependencies
- `pyproject.toml` and `osmnx/` - local OSMnx package

The first request for a city downloads its road graph. Graphs are stored in `cache/graphs/`; attach a persistent Railway volume at `/app/cache` if you want to retain them across deployments.

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