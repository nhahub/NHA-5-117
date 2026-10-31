# Real-Time Object Detection for Autonomous Vehicles

DEPI graduation project - AI & Data Science track (AWS Machine Learning), Round 5.
Advisor: Eng. Randa

A lightweight YOLOv5s detector with ECA and SE channel attention (PyTorch) for pedestrians, cyclists, vehicles, traffic signs and obstacles. The pipeline runs on AWS: datasets in Amazon S3, training on Amazon SageMaker, the final ONNX model on a SageMaker endpoint, and a drift-triggered retraining loop.

**Research question:** the base paper ([1], see References) reports 98.4% mAP@50 on KITTI for a vehicle-only model but a drop on BDD100K in low light, blur and occlusion. How big is the drop per class and per condition, and do augmentation and fine-tuning reduce it?

Status: work in progress. Results will be added here when they exist.

## Team

| # | Name | Role |
|---|------|------|
| 1 | Loai Ashraf Nour El-Deen Mohamed | Pedestrian & Cyclist pipeline lead |
| 2 | Nour Nabil Ibrahim Mohammed | Vehicle pipeline lead |
| 3 | Mariam Mohamed Elsayed Tantawy | Traffic Sign & Obstacle pipeline lead |
| 4 | Karim Mohamed Alamir Sharaf | Core YOLO, S3 data store, base training |
| 5 | Logain Amr Ali Mahmoud | Inference, ONNX export, SageMaker endpoint |
| 6 | Mazen Reda Ahmed Murad | MLOps, drift monitoring, reports |

## Scope

- In scope: existing public datasets (KITTI, BDD100K, COCO pre-trained weights), AWS pipeline, ONNX export, replaying recorded video as a simulated camera stream, drift monitoring, simulated automated retraining, demo app.
- Out of scope: vehicle hardware, sensors, live camera capture, IoT services, collecting new data, object tracking, vehicle control, safety certification.

## Pipeline

```
Datasets (KITTI, BDD100K) -> Amazon S3 (versioned, DVC remote)
  -> AWS Glue + Athena (SQL on annotations)
  -> SageMaker training job (YOLOv5s + ECA/SE)
  -> S3 model artifacts (PyTorch + ONNX)
  -> SageMaker endpoint (ONNX inference)
  -> Streamlit demo (replays recorded video with OpenCV)

Drift loop: inference logs (CloudWatch + S3) -> Evidently drift report
  -> above threshold: GitHub Actions or AWS Step Functions starts a SageMaker training job
  -> evaluate -> register in MLflow -> redeploy only if it passes
```

## Timeline

Five working weeks plus one buffer week.

| Week | Milestone |
|------|-----------|
| 1 | M1 Data collection, exploration, preprocessing (AWS setup, S3, label-mapping test) |
| 2-3 | M2 Model development and evaluation (SageMaker training, ECA/SE, cross-dataset) |
| 4 | M3 ONNX export, SageMaker endpoint, replay testing |
| 4-5 | M4 MLOps and monitoring (Evidently, automated retraining trigger) |
| 5 | M5 Final report, presentation, infographic |
| 6 | Buffer |

Deadlines: System Analysis & Design 6 Nov 2026; Implementation 30 Nov 2026; Final presentation, testing and reports 4 Dec 2026.

## Repository layout

Adjust to the folders the program already created.

```
docs/        documentation, reports, weekly plans
data/        DVC pointer files only (data lives in S3)
src/         preprocessing, training, evaluation, export, inference
sagemaker/   SageMaker training and inference scripts
mlops/       drift checks, retraining workflows
app/         Streamlit demo
```

## Setup

To be filled in as the code is added. Each folder will list its exact commands.

```
python -m venv .venv
.venv\Scripts\activate        # Windows
pip install -r requirements.txt
```

Never commit AWS keys or data. Use IAM roles and GitHub Actions secrets. See CONTRIBUTING.md.

## Targets (proposed, not results)

mAP@50 of at least 0.70 per slice; at least 30 FPS for the ONNX model on a laptop GPU; at most 2 mAP points lost after export; zero invalid boxes in the Week 1 label-mapping test.

## Data

KITTI and BDD100K are used under their own licenses. Do not redistribute the data.

## References

1. Adam, M. A. A., and Tapamo, J. R. (2025). Enhancing YOLOv5 for Autonomous Driving: Efficient Attention-Based Object Detection on Edge Devices. Journal of Imaging, 11(8), 263. https://doi.org/10.3390/jimaging11080263
2. Geiger, A., Lenz, P., Stiller, C., and Urtasun, R. (2013). Vision Meets Robotics: The KITTI Dataset. https://doi.org/10.1177/0278364913491297
3. Yu, F., et al. (2020). BDD100K: A Diverse Driving Dataset for Heterogeneous Multitask Learning. https://arxiv.org/abs/1805.04687
4. Ultralytics YOLOv5. https://github.com/ultralytics/yolov5
