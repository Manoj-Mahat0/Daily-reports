## What this service does
A Flask-based API that detects whether an input image or a small sample of frames from a video is likely deepfaked.

## Key capabilities
- Image classification: single image upload returns a label (real/fake) and a confidence score
- Video sampling: N random frames (default 5) are extracted and classified; results are aggregated
- Batched preprocessing: images are resized to a fixed batch size for efficient inference
- Simple health check endpoint for monitoring

## Non-goals
- Full video stream processing (the service samples frames, it does not classify every frame)
- Face swapping or editing; this is a detection-only API

## Inputs and limits (recommended)
- Image formats: JPEG, PNG, WEBP
- Video formats: MP4, MOV, AVI (container support depends on available codecs in your deployment)
- Max image size: 10 MB (enforced by gateway or reverse proxy)
- Max video size: 100 MB (enforced by gateway or reverse proxy)

## Outputs
- Label: `real` or `fake`
- Confidence: float in [0, 1]
- For videos: per-frame predictions and an aggregated overall decision

## Model and pipeline (high level)
1. Preprocessing: decode -> resize -> normalize -> optional face detection/cropping
2. Inference: deep learning model runs on CPU/GPU
3. Postprocessing: thresholding and aggregation across frames

## Security & privacy considerations
- Do not upload PII or copyrighted content without authorization
- Files are processed transiently; long-term storage is not required and should be disabled by default
- Consider scanning uploads for malware before processing in production