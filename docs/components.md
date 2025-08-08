## System components

- Ingress & routing
  - Reverse proxy / gateway (rate limiting, size limits, TLS termination)
  - Flask app exposes `/health`, `/v1/predict/image`, `/v1/predict/video`

- Preprocessing
  - Image decoding and validation (MIME sniffing, size checks)
  - Batch resize to the model's expected input size
  - For video: sample N random frames (default 5), extract as images

- Inference
  - Load model weights once at startup (CPU/GPU)
  - Run batched inference for efficiency

- Postprocessing
  - Map logits to probabilities, compute `label` and `confidence`
  - For videos: aggregate per-frame predictions to a single overall decision

- Observability
  - Structured logs with request IDs
  - Basic metrics (latency, throughput, error rates)

- Error handling
  - 400 for invalid inputs
  - 413 for oversized payloads
  - 415 for unsupported media
  - 500 for unexpected errors

## Data flow (video)
1. Receive multipart upload
2. Validate container and size
3. Sample N random frame indices
4. Decode frames -> preprocess -> batch
5. Run inference -> per-frame results
6. Aggregate -> return JSON