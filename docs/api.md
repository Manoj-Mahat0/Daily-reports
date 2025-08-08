## HTTP API reference

Base URL: `https://<your-render-app>.onrender.com` (production) or `http://localhost:8000` (local)

### GET /health
- Description: Liveness probe
- Response: `200 OK`, body: `ok`

curl
```
curl -sS https://<your-render-app>.onrender.com/health
```

### POST /v1/predict/image
- Description: Classify a single image
- Content-Type: `multipart/form-data`
- Form fields:
  - `file` (required): image file
  - `model_version` (optional): string, model tag to use

Request example (curl)
```
curl -sS -X POST \
  -F "file=@/path/to/image.jpg" \
  -F "model_version=latest" \
  https://<your-render-app>.onrender.com/v1/predict/image | jq .
```

Response 200 example
```
{
  "label": "fake",
  "confidence": 0.92,
  "details": {
    "model_version": "latest"
  }
}
```

Errors
- 400: missing or invalid input
- 415: unsupported media type
- 500: server error

### POST /v1/predict/video
- Description: Classify a video by sampling frames and aggregating predictions
- Content-Type: `multipart/form-data`
- Form fields:
  - `file` (required): video file
  - `num_frames` (optional): integer, number of random frames to sample (default 5)
  - `model_version` (optional): string, model tag to use

Request example (curl)
```
curl -sS -X POST \
  -F "file=@/path/to/video.mp4" \
  -F "num_frames=5" \
  https://<your-render-app>.onrender.com/v1/predict/video | jq .
```

Response 200 example
```
{
  "label": "real",
  "confidence": 0.81,
  "details": {
    "model_version": "latest",
    "per_frame": [
      { "frame_index": 12, "timestamp_ms": 400, "label": "real", "confidence": 0.78 },
      { "frame_index": 87, "timestamp_ms": 2900, "label": "real", "confidence": 0.88 },
      { "frame_index": 131, "timestamp_ms": 4400, "label": "real", "confidence": 0.83 }
    ]
  }
}
```

Errors
- 400: missing or invalid input
- 413: payload too large (enforced by server/gateway)
- 415: unsupported media type
- 500: server error

### Common headers
- `Content-Type`: use `multipart/form-data` for uploads
- `Accept`: prefer `application/json`

### Rate limiting
- Recommend enabling reverse proxy rate limiting in production

### OpenAPI
- See `openapi.yaml` for a machine-readable spec