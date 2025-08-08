## Python usage examples (client)

Install dependencies
```
pip install requests
```

Set your base URL
```
BASE_URL = "https://<your-render-app>.onrender.com"
```

Predict an image
```python
import requests

def predict_image(image_path: str, base_url: str) -> dict:
    with open(image_path, "rb") as f:
        files = {"file": (image_path, f, "application/octet-stream")}
        resp = requests.post(f"{base_url}/v1/predict/image", files=files, timeout=60)
        resp.raise_for_status()
        return resp.json()

if __name__ == "__main__":
    print(predict_image("/path/to/image.jpg", BASE_URL))
```

Predict a video (with frame sampling)
```python
import requests

def predict_video(video_path: str, base_url: str, num_frames: int = 5) -> dict:
    data = {"num_frames": str(num_frames)}
    with open(video_path, "rb") as f:
        files = {"file": (video_path, f, "application/octet-stream")}
        resp = requests.post(f"{base_url}/v1/predict/video", data=data, files=files, timeout=300)
        resp.raise_for_status()
        return resp.json()

if __name__ == "__main__":
    print(predict_video("/path/to/video.mp4", BASE_URL, num_frames=5))
```

Error handling
```python
import requests

try:
    predict_image("/missing.jpg", BASE_URL)
except requests.HTTPError as e:
    print("Status:", e.response.status_code)
    print("Body:", e.response.text)
```