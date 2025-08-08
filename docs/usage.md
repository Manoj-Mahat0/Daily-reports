## Practical usage

### cURL
- Health
```
curl -sS https://<your-render-app>.onrender.com/health
```
- Image prediction
```
curl -sS -X POST \
  -F "file=@/path/to/image.jpg" \
  https://<your-render-app>.onrender.com/v1/predict/image | jq .
```
- Video prediction
```
curl -sS -X POST \
  -F "file=@/path/to/video.mp4" \
  -F "num_frames=5" \
  https://<your-render-app>.onrender.com/v1/predict/video | jq .
```

### Postman
- Import `openapi.yaml` to generate requests automatically
- Set the `server` variable to your deployed base URL

### Local development (generic)
These are generic instructions when running a Flask app exposing the documented endpoints. Adapt to your actual entrypoint.
- Create a virtualenv and install your project requirements
- Ensure your model weights are available where the app expects them
- Start the app (examples):
  - `flask --app app run --host 0.0.0.0 --port 8000`
  - or `python app.py`
- Test locally with the same cURL commands using `http://localhost:8000`

### Notes
- For large videos, increase client and server timeouts accordingly
- Enforce file size limits at the proxy (e.g., Nginx `client_max_body_size`)
- Consider asynchronous processing if inference is slow or workloads spike