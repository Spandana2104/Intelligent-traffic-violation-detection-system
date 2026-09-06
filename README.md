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