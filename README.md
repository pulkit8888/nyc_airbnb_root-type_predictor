# 🏙️ NYC Airbnb Room Type Predictor

A machine learning web application that predicts the **room type** of a New York City Airbnb listing — *Entire home/apt*, *Private room*, or *Shared room* — based on listing attributes such as location, price, and review history.

The project combines a **scikit-learn ML pipeline** served via a **FastAPI** REST API with a **vanilla HTML/CSS/JS** frontend that brings the prediction to life through animated NYC-style building visualisations.

---

## 📸 Overview

| Feature | Detail |
|---|---|
| **Prediction target** | Room type (`Entire home/apt`, `Private room`, `Shared room`) |
| **Model** | Pre-trained scikit-learn pipeline (`Model_Pipeline.pkl`) |
| **Backend** | FastAPI + Uvicorn |
| **Frontend** | HTML5 + Vanilla CSS + Vanilla JS |
| **Live deployment** | [nyc-airbnb-room-type-predictor.onrender.com](https://nyc-airbnb-room-type-predictor.onrender.com) |

---

## 🗂️ Project Structure

```
nycairbnbroomtypepredictor/
├── main.py                                      # FastAPI application & prediction endpoint
├── Model_Pipeline.pkl                           # Pre-trained scikit-learn model pipeline
├── nyc_airbnb_room_type_classification.ipynb    # Jupyter notebook: data analysis & model training
├── index.html                                   # Frontend UI
├── style.css                                    # Frontend styles (dark NYC aesthetic)
├── script.js                                    # Frontend logic (API calls, animations)
├── requirements.txt                             # Python dependencies
├── runtime.txt                                  # Python version for deployment
└── the_build_line_guide.html                    # Design reference / style guide
```

---

## ⚙️ How It Works

### 1. Model Training (`nyc_airbnb_room_type_classification.ipynb`)
The Jupyter notebook covers:
- Exploratory data analysis (EDA) on NYC Airbnb listing data
- Feature engineering & preprocessing
- Training a classification model wrapped in a scikit-learn `Pipeline`
- Serialising the final pipeline to `Model_Pipeline.pkl` via `joblib`

### 2. Backend API (`main.py`)
A **FastAPI** server loads the saved pipeline and exposes two endpoints:

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Health check — returns a greeting |
| `POST` | `/predict` | Accepts listing features, returns room-type prediction + class probabilities |

**Input features validated by Pydantic:**

| Field | Type | Constraints |
|---|---|---|
| `latitude` | `float` | −90 to 90 |
| `longitude` | `float` | −180 to 180 |
| `price` | `float` | > 0 |
| `minimum_nights` | `int` | 1 – 365 |
| `number_of_reviews` | `int` | ≥ 0 |
| `reviews_per_month` | `float` | ≥ 0 |
| `calculated_host_listings_count` | `int` | ≥ 0 |
| `availability_365` | `int` | 0 – 365 |
| `neighbourhood_group` | `str` | Borough name (e.g. `Manhattan`) |
| `neighbourhood` | `str` | Neighbourhood name (e.g. `Williamsburg`) |

**Example response:**
```json
{
  "Predicted_room_type": "Entire home/apt",
  "Probability": [0.87, 0.11, 0.02]
}
```

### 3. Frontend (`index.html` / `script.js` / `style.css`)
- NYC skyline background with animated twinkling window lights
- Form with real-time validation for all 10 listing features
- **"Try an example"** button cycles through 3 realistic listing presets (Manhattan, Brooklyn, Queens)
- Live **API status indicator** (online / unreachable)
- Animated building visualisation — each building's height and lit windows reflect the model's predicted probabilities
- Animated percentage counters for each room-type class

---

## 🚀 Getting Started

### Prerequisites
- **Python 3.12.7** (see `runtime.txt`)
- `pip` (Python package manager)
- A terminal / command prompt

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/nycairbnbroomtypepredictor.git
cd nycairbnbroomtypepredictor
```

### 2. Create and activate a virtual environment *(recommended)*

**Windows (PowerShell)**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux**
```bash
python -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start the FastAPI server

```bash
uvicorn main:app --reload
```

The API will be available at **http://127.0.0.1:8000**

> **Tip:** Visit **http://127.0.0.1:8000/docs** for the auto-generated interactive Swagger UI.

### 5. Open the frontend

Open `index.html` directly in your browser, **or** serve it with any static file server:

```bash
# Python built-in server
python -m http.server 5500
```

Then navigate to **http://127.0.0.1:5500**.

> **Important:** The frontend's `script.js` points to the live Render deployment by default.
> To use your **local** FastAPI server instead, edit line 5 of `script.js`:
> ```js
> // Change this:
> const API_BASE_URL = "https://nyc-airbnb-room-type-predictor.onrender.com";
> // To this:
> const API_BASE_URL = "http://127.0.0.1:8000";
> ```

---

## 📡 API Reference

### `POST /predict`

**Request body (JSON):**
```json
{
  "latitude": 40.7484,
  "longitude": -73.9857,
  "price": 120,
  "minimum_nights": 2,
  "number_of_reviews": 84,
  "reviews_per_month": 2.3,
  "calculated_host_listings_count": 1,
  "availability_365": 210,
  "neighbourhood_group": "Manhattan",
  "neighbourhood": "Midtown"
}
```

**Response (JSON):**
```json
{
  "Predicted_room_type": "Entire home/apt",
  "Probability": [0.87, 0.11, 0.02]
}
```

Probabilities are aligned to the model's internal class order:
`[Entire home/apt, Private room, Shared room]`

### `GET /`

Returns a plain greeting string confirming the API is live.

---

## 🛠️ Tech Stack

### Backend
| Tool | Version | Purpose |
|---|---|---|
| [Python](https://www.python.org/) | 3.12.7 | Runtime |
| [FastAPI](https://fastapi.tiangolo.com/) | 0.115.6 | REST API framework |
| [Uvicorn](https://www.uvicorn.org/) | 0.34.0 | ASGI server |
| [Pydantic](https://docs.pydantic.dev/) | 2.10.4 | Request validation & schemas |
| [pandas](https://pandas.pydata.org/) | 2.2.3 | DataFrame construction for model input |
| [scikit-learn](https://scikit-learn.org/) | 1.6.1 | ML pipeline & classification model |
| [joblib](https://joblib.readthedocs.io/) | 1.4.2 | Model serialisation / loading |

### Frontend
| Tool | Purpose |
|---|---|
| HTML5 | Page structure & semantic markup |
| Vanilla CSS | Dark NYC-themed design, animations, glassmorphism |
| Vanilla JavaScript (ES2020+) | API integration, DOM manipulation, canvas-less animations |
| [Google Fonts](https://fonts.google.com/) | *Space Grotesk*, *Inter*, *JetBrains Mono* typography |

### Deployment
| Tool | Purpose |
|---|---|
| [Render](https://render.com/) | Cloud hosting for the FastAPI backend |

---

## 🧪 Re-training the Model

To retrain or experiment with the model, open the notebook:

```bash
jupyter notebook nyc_airbnb_room_type_classification.ipynb
```

After training, export the updated pipeline:

```python
import joblib
joblib.dump(pipeline, "Model_Pipeline.pkl")
```

Then restart the FastAPI server to pick up the new model file.

---

## 🌐 Deployment (Render)

The API is already deployed at:
> **https://nyc-airbnb-room-type-predictor.onrender.com**

To deploy your own instance on Render:

1. Push the repository to GitHub.
2. Create a new **Web Service** on [render.com](https://render.com).
3. Set the **Build Command**: `pip install -r requirements.txt`
4. Set the **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Set **Python version** to `3.12.7` (matching `runtime.txt`).
6. Deploy — Render will auto-detect `runtime.txt`.

---

## 📋 Requirements

```
fastapi==0.115.6
uvicorn[standard]==0.34.0
pydantic==2.10.4
pandas==2.2.3
scikit-learn==1.6.1
joblib==1.4.2
```
---

> *Model served via FastAPI & scikit-learn · trained on NYC Airbnb listing data*
