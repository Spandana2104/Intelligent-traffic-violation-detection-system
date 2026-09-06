# Intelligent Traffic Violation Detection System (ITVDS) 🚦🚘

An end-to-end autonomous AI traffic enforcement and surveillance system powered by **YOLOv8**, **ByteTrack**, **FastAPI**, **SQLite**, and a real-time **React Admin Dashboard**.

---

## 🌟 Key Features

* 🚦 **Signal Jumping Detection:** Detects vehicles crossing stop line polygons during Red signal phases.
* 🏍️ **Helmet & Triple Riding Detection:** Identifies un-helmeted motorcycle riders and flags >2 passengers.
* 🚗 **Seatbelt & Speeding Violation Detection:** Computes real-time vehicle displacement & speed (km/h) and detects front-seat seatbelt compliance.
* 🚘 **ALPR / OCR License Plate Recognition:** Extracts vehicle registration plates with **PaddleOCR** and validates against standard Indian plate format (`^[A-Z]{2}[0-9]{2}[A-Z]{1,2}[0-9]{4}$`).
* 🗄️ **SQLite Database & Fine Automation:** Stores violation evidence, tracks repeat offenders ($\ge 3$ violations), and automatically applies a **$2\times$ Fine Multiplier**.
* 📊 **React Command Dashboard:** Live AI event streaming, camera grid maps, analytics charts, and payment management.

---

## 🛠️ System Architecture

```text
                  TRAFFIC CAMERA VIDEO / LIVE FEED
                                  │
                                  ▼
                   [Frame Extraction - OpenCV]
                                  │
                                  ▼
                [Vehicle Detection & Tracking - YOLOv8]
                                  │
                                  ▼
                   [Metadata & Feature Extraction]
  ┌──────────────────┬──────────────────┬──────────────────┐
  │                  │                  │                  │
  ▼                  ▼                  ▼                  ▼
Speeding        Signal Jump       Helmet / Seatbelt  Triple Riding
 (>60 km/h)   (Red Light Line)      Compliance       (>2 Riders)
  └──────────────────┴──────────────────┴──────────────────┘
                                  │
                                  ▼
               [FastAPI Violation Classifier Engine]
                                  │
                                  ▼
               [Evidence Generator & Plate Region Crop]
                                  │
                                  ▼
                 [ALPR / OCR - Indian Regex Check]
                                  │
                                  ▼
               [SQLite Database & Repeat Offender Rule]
                                  │
                                  ▼
                  [React Command Dashboard UI]


📂 Project Structure

ITVDS/
├── backend/
│   ├── classifier.py            # FastAPI Server & Rule Classifier
│   ├── database.py              # SQLite Database & Repeat Offender Fines
│   ├── decision_engine.py       # Action Planner Engine
│   ├── evidence_generator.py    # Evidence Record Builder
│   ├── ocr_engine.py            # ALPR OCR & Indian Regex Validator
│   ├── pipeline_cv.py           # Computer Vision & YOLOv8 Video Pipeline
│   ├── test_all_violations.py   # Test Suite for All 6 Violation Rules
│   └── violations.db            # SQLite Database File
├── src/                         # React + Vite TypeScript Dashboard UI
│   ├── components/              # Dashboard Cards, Feeds, Camera Grid
│   ├── routes/                  # Navigation Views (Violations, Analytics, Fines)
│   └── lib/api.ts               # Frontend REST API Service
└── README.md



🚀 Quick Setup & Installation
1. Prerequisites
Python 3.10+
Node.js 18+


2. Backend Setup

# Install Python dependencies
pip install fastapi uvicorn pydantic opencv-python ultralytics supervision
# Start FastAPI Backend Server
cd backend
python -m uvicorn classifier:app --reload --port 8000

3. Frontend Dashboard Setup

# In project root folder
npm install
npm run dev

4. Run Computer Vision Pipeline

python backend/pipeline_cv.py


## 📊 Violation Fine Rules

- 🚗 **Speeding (>60 km/h):** Base Fine ₹1,000 | Repeat Offender Fine ₹2,000
- 🚦 **Red Light / Signal Jumping:** Base Fine ₹1,000 | Repeat Offender Fine ₹2,000
- 🏍️ **No Helmet (Motorcycle):** Base Fine ₹500 | Repeat Offender Fine ₹1,000
- 🚘 **No Seatbelt (Car):** Base Fine ₹1,000 | Repeat Offender Fine ₹2,000
- 🛵 **Triple Riding:** Base Fine ₹1,000 | Repeat Offender Fine ₹2,000
- ⛔ **Wrong Way Driving:** Base Fine ₹500 | Repeat Offender Fine ₹1,000