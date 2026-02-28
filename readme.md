# Bag Counting using YOLOv8 (Internal CV Model)

## Overview

This project implements an internal computer vision pipeline to detect and count industrial bags from fixed-camera video footage.

The system uses a custom-trained YOLOv8 object detection model combined with multi-object tracking and a virtual line-crossing counting mechanism.

---

## Dataset Preparation

### Data Source
Video clips were provided for the task. Frames were extracted from the videos and manually annotated.

### Annotation Tool
Annotations were performed using Roboflow.

### Annotation Format
- YOLO format (bounding box)
- Single class: `bag`

All annotation files are included in `annotations.zip`.

---

## Why Only Training Split Was Used

During dataset version generation, the validation and test splits contained mostly empty (no-object) images due to automatic distribution.

Since this is an **initial internal prototype version**, the model was trained using the properly annotated training images to ensure:

- Stable object learning
- Faster iteration
- Practical evaluation using full-video inference

Final validation was performed directly on complete video inference instead of relying on the incomplete validation split.

---

## Model Details

- Model: YOLOv8 (Ultralytics)
- Base variant: yolov8n
- Custom trained on annotated bag dataset
- Single-class detection (`bag`)

Trained model file: `best.pt`

---

## Counting Logic

To prevent duplicate counting across frames:

1. YOLOv8 detects bags in each frame.
2. ByteTrack multi-object tracking assigns a unique ID to each bag.
3. A virtual vertical counting line is defined.
4. When a bag’s centroid crosses the line (Right → Left direction), the counter increments.
5. Counted tracking IDs are stored to prevent double counting.

This ensures each physical bag is counted exactly once.

---

## Output

The output video (`output_counted.mp4`) contains:

- Bounding boxes
- Tracking IDs
- Counting line
- Live bag count overlay

Final detected bag count for the sample video: **12**

---

## How to Run

Install dependencies:

pip install ultralytics opencv-python numpy

Run:

python bag_counter.py

Ensure:
- `best.pt` is present in the same directory
- Input video path is correctly set inside the script

---

## Notes

- This is an internal prototype version.
- Optimized for the provided fixed-camera setup.
- Further improvements may include:
  - Balanced validation split
  - Tracker parameter tuning
  - Region-based counting
  - Performance optimization
