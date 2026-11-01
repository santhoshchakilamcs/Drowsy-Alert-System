# Drowsy-Alert-System

Detection of people while driving when they are drowsy. If the driving person closes his/her eyes the alarm switches on so that he can wake up.

## What it does

The script `Raspberrypi_detect_drowsiness.py` runs on a camera feed, detects a face, finds the eye landmarks, and watches how open the eyes are. If the eyes stay closed for long enough, it sounds an alarm.

## How it works

- Face detection: OpenCV Haar cascade (`haarcascade_frontalface_default.xml`).
- Eye landmarks: dlib 68-point facial landmark predictor (`shape_predictor_68_face_landmarks.dat`).
- Eye aspect ratio (EAR): for each eye, the ratio of the vertical eye distances to the horizontal eye distance. The EAR for both eyes is averaged.
- Threshold: `EYE_AR_THRESH = 0.3`. An EAR below 0.3 counts as a closed eye.
- Consecutive frames: `EYE_AR_CONSEC_FRAMES = 16`. The alarm only triggers after the EAR has been below the threshold for 16 consecutive frames, so a normal blink does not set it off.
- Alarm: optional, with `--alarm 1`. When enabled, a TrafficHat alarm (GPIO) is used.

## Hardware

- Raspberry Pi with a camera (the script also has a commented-out PiCamera option, and otherwise uses a video stream from camera source 0).
- A face cascade file and a dlib 68-point shape predictor file, passed with `--cascade` and `--shape-predictor`.

## Usage

```
python pi_detect_drowsiness.py --cascade haarcascade_frontalface_default.xml --shape-predictor shape_predictor_68_face_landmarks.dat
python pi_detect_drowsiness.py --cascade haarcascade_frontalface_default.xml --shape-predictor shape_predictor_68_face_landmarks.dat --alarm 1
```

## Requirements

- Python with OpenCV (`cv2`), dlib, imutils, and NumPy.
- On a Raspberry Pi with the alarm enabled, `gpiozero` with a TrafficHat.
- The cascade and shape predictor files listed above, which are not included in this repo.

## Safety disclaimer

This is a demonstration project, not a safety device. It does not guarantee detection of drowsiness, it can miss drowsiness or trigger falsely, and it must never be relied on to keep a driver awake or to prevent an accident. Do not use it as a substitute for rest, safe driving practices, or any vehicle safety system.
