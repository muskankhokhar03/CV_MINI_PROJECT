# 🕳️ Pothole Detection and Counting Using YOLO26
👥 Team Members
This project was developed collaboratively by:

Muskan Khokhar
Mohit Dave
Ved Patel
Noman Imran Teli

A Computer Vision project that uses YOLO26 (Ultralytics) to detect potholes in road images and videos. The project includes dataset preparation, model training, image inference, pothole counting, video tracking, and a FastAPI backend.

Detection Class: Pothole (Class ID: 0)

---

## 🚀 Project Overview

Road potholes are a major safety concern for drivers and pedestrians. This project aims to automate pothole detection using deep learning and computer vision.

A YOLO26 object detection model is trained to identify potholes in road images and videos. The system can detect potholes, count them in images, and track unique potholes across video frames to avoid repeated counting.

## ✨ Features

- Pothole detection in road images using YOLO26.
- Image inference with confidence scores and bounding boxes.
- Pothole counting in individual images.
- Pothole counting in folders containing multiple images.
- Video-based pothole detection.
- Object tracking using BoT-SORT.
- Unique pothole counting using a line-crossing mechanism.
- Dataset conversion from Pascal VOC XML to YOLO format.
- Model training and evaluation.
- FastAPI backend for image and video detection.
- CPU-based model training support.

## 🛠️ Technologies Used

| Technology       | Purpose                      |
| ---------------- | ---------------------------- |
| Python           | Programming language         |
| YOLO26           | Object detection             |
| Ultralytics      | Model training and inference |
| OpenCV           | Image and video processing   |
| NumPy            | Numerical processing         |
| FastAPI          | Backend API                  |
| Uvicorn          | API server                   |
| BoT-SORT         | Object tracking              |
| Jupyter Notebook | Dataset preparation          |

## 📂 Project Structure
```
CV_mini/
│
├── assets/
│   └── Demo images and videos
│
├── backend/
│   ├── main.py
│   ├── detector.py
│   └── requirements.txt
│
├── .gitignore
├── dataset.yaml
├── splits.json
├── tracker_pothole.yaml
├── exp.ipynb
├── train.py
├── infer.py
├── pothole_count.py
├── pothole_count_video.py
├── prepare_yolo_dataset.py
├── requirements.txt
└── README.md
```

## 📊 Dataset

The project uses the Annotated Potholes Image Dataset from Kaggle.

Dataset Link:

[https://www.kaggle.com/datasets/chitholian/annotated-potholes-dataset](https://www.kaggle.com/datasets/chitholian/annotated-potholes-dataset)

The dataset contains road images with pothole annotations in Pascal VOC XML format.

The project converts these annotations into YOLO format for model training.

## ⚙️ Installation and Setup

### 1. Clone the Repository
```
git clone https://github.com/Ved720/CV_mini.git
```
```
cd CV_mini
```

### 2. Create a Virtual Environment
```
python -m venv .venv
```

Activate the environment.

Windows:
```
.venv\Scripts\activate
```

Linux / macOS:
```
source .venv/bin/activate
```

### 3. Install Dependencies
```
pip install -r requirements.txt
```

## 📥 Dataset Preparation

1. Configure your Kaggle API credentials.
2. Run the cells in exp.ipynb to download the dataset.
3. Place the downloaded images and XML annotations inside annotated-images/.
4. Ensure splits.json is available in the project directory.
5. Run the dataset preparation script.
```
python prepare_yolo_dataset.py
```

The script converts Pascal VOC XML annotations into YOLO format and creates the following structure:
```
pothole_dataset/
│
├── train/
│   ├── images/
│   └── labels/
│
├── val/
│   ├── images/
│   └── labels/
│
└── test/
    ├── images/
    └── labels/
```

## 🧠 Model Training

Run the following command:
```
python train.py
```

Training configuration:

| Parameter               | Value     |
| ----------------------- | --------- |
| Model                   | YOLO26n   |
| Epochs                  | 50        |
| Image Size              | 416 × 416 |
| Batch Size              | 8         |
| Workers                 | 2         |
| Early Stopping Patience | 10        |
| Training Environment    | CPU       |

The best trained model is saved at:
```
runs/pothole_yolo26n/weights/best.pt
```

## 🔍 Image Detection

To detect potholes in a single image:
```
python infer.py test_image.jpg
```

To change the confidence threshold:
```
python infer.py test_image.jpg --conf 0.45
```

To display the detection window:
```
python infer.py test_image.jpg --show
```

The model detects potholes and displays bounding boxes with confidence scores.

## 🔢 Pothole Counting in Images

To count potholes in a single image:
```
python pothole_count.py test_image.jpg
```

To process multiple images in a folder:
```
python pothole_count.py path/to/image_folder --conf 0.30
```

The system generates numbered bounding boxes for detected potholes.

Output directory:
```
runs/pothole_count/
```

## 🎥 Pothole Detection and Counting in Videos

To detect potholes in a video:
```
python pothole_count_video.py pothole.mp4
```

To configure video processing:
```
python pothole_count_video.py pothole.mp4 --conf 0.25 --vid-stride 2
```

The system uses object tracking and a counting line to count each tracked pothole once when it crosses the line.

The annotated video is saved in:
```
runs/pothole_count_video/
```

## 🌐 FastAPI Backend

The project also includes a FastAPI backend for serving pothole detection functionality.

Navigate to the backend directory:
```
cd backend
```

Install backend dependencies:
```
pip install -r requirements.txt
```

Start the API server:
```
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Open the API documentation:

[http://localhost:8000/docs](http://localhost:8000/docs)

### API Endpoints

| Method | Endpoint        | Description                                    |
| ------ | --------------- | ---------------------------------------------- |
| GET    | /health       | Check API health                               |
| POST   | /detect       | Detect potholes in an uploaded image           |
| POST   | /detect_video | Detect and count potholes in an uploaded video |

The API requires the trained model weights to be available at the configured path.

## 📈 Model Performance

The project was trained for 50 epochs with early stopping.

### Evaluation Results

| Metric    | Validation | Test  |
| --------- | ---------- | ----- |
| Precision | 0.741      | 0.736 |
| Recall    | 0.666      | 0.643 |
| mAP50     | 0.745      | 0.703 |
| mAP50-95  | 0.448      | 0.422 |

Additional information:

- Training time: Approximately 1.93 hours.
- Inference speed: Approximately 67 ms per image on CPU.
- Model: YOLO26 Nano.
- Number of classes: 1 (Pothole).

## 🔮 Future Improvements

- Improve detection accuracy using larger YOLO models.
- Increase dataset size and diversity.
- Improve detection in different lighting and weather conditions.
- Optimize real-time video detection.
- Develop a web-based pothole monitoring dashboard.
- Integrate GPS coordinates for pothole location tracking.

## 📚 Acknowledgements

Dataset: [Annotated Potholes Image Dataset](https://www.kaggle.com/datasets/chitholian/annotated-potholes-dataset) by chitholian.

Model framework: [Ultralytics](https://www.ultralytics.com/).

## 👨‍💻 Author

Muskan khokhar

---

⭐ If you find this project useful, please consider giving this repository a star!

Computer Vision Mini Project | Pothole Detection and Counting Using YOLO26
