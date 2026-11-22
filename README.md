# MediaForensiX — Deepfake Video Detector

**Final-year project · Bachelor in Computer Forensic (Hons), Management and Science University (2024)**
**🥇 Gold Medal, 18th Idea Regeneration Expo (iReX) 2024**

A web app that checks whether a video is a deepfake. Upload a video, choose how many frames to analyse, and
MediaForensiX labels it **REAL** or **FAKE** with a confidence score. It also shows the face frames it analysed, with
heatmaps of the regions that drove the decision, and the file's forensic metadata.

## How it works
1. **Upload:** a Django form accepts common video formats (mp4, webm, avi, mkv, wmv, flv, 3gp, gif) and the number of
   frames to analyse.
2. **Frames and faces:** OpenCV splits the video into frames, and `face_recognition` finds and crops the face in each
   frame. face-api.js marks the faces in the browser.
3. **Classify:** a ResNeXt-50 (32×4d) network turns each face frame into features, and an LSTM reads them as a
   sequence. A final layer gives *real* or *fake* plus its confidence (PyTorch).
4. **Explain:** heatmaps from the last convolutional layer show where the model looked.
5. **Forensics:** ffmpeg reads the codec, resolution, frame rate and duration, next to the file's timestamps.

## Run it
- Python 3 with `django torch torchvision opencv-python face_recognition ffmpeg-python numpy matplotlib pillow`, and
  ffmpeg on the PATH.
- Put trained weights in `django_app/models/` (not included because of their size). The app picks the model whose
  file name matches the chosen frame count, e.g. `model_<accuracy>_acc_<frames>_frames_<name>.pt`.
- `cd django_app` → `python manage.py runserver` → open http://127.0.0.1:8000.
- Set the `DJANGO_SECRET_KEY` environment variable for anything beyond a local demo.

**Tools:** Python, PyTorch, torchvision, OpenCV, face_recognition, Django, ffmpeg, face-api.js, Bootstrap
