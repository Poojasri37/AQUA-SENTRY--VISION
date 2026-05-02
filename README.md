# Aqua Sentry: AI-Powered Water Quality Monitoring & Management System

![Aqua Sentry UI](https://via.placeholder.com/1000x500?text=Aqua+Sentry+Dashboard)

Aqua Sentry is an advanced, end-to-end IoT and AI-driven platform designed to monitor, analyze, and remediate water quality in real-time. By integrating state-of-the-art sensor arrays, predictive AI models, and computer vision, the platform provides actionable insights to ensure safe drinking water, mitigate health risks, and suggest localized natural purification methods.

---

## 🌟 Core Features

- **Real-Time IoT Monitoring**: Continuous tracking of pH, turbidity, temperature, and TDS via ESP32 sensor integration.
- **Microplastic Detection**: Computer vision model (YOLO) to identify and quantify microplastic contamination in water samples.
- **Carcinogenic Risk Index (CRI)**: Algorithmic assessment of long-term health risks based on chemical contaminants and heavy metals.
- **AI-Driven Predictive Analysis**: Utilizing LLM models to predict water quality trends, assess anomalies, and generate human-readable safety reports.
- **Natural Purification Recommendations**: Context-aware suggestions for eco-friendly, accessible water treatment methods based on specific regional and contamination data.
- **Interactive 3D Geospatial Dashboard**: View global and regional water data on an interactive 3D globe.

---

## 🧠 Deep Dive: AI Components & Architecture

Aqua Sentry leverages a multi-faceted AI approach to provide comprehensive water quality intelligence. 

### 1. Computer Vision: Microplastic Detection (YOLO)
Microplastics are a growing threat to aquatic ecosystems and human health. Aqua Sentry integrates a custom-trained **YOLO (You Only Look Once)** deep learning model to detect microplastics in microscopic water sample images.
*   **How it Works**: Users upload microscopic images of water samples via the Microplastic Detection dashboard. The image is passed to a dedicated Python microservice (`yolo_inference.py`) running via a child process in the Node.js backend.
*   **Capabilities**: Identifies particle counts, provides confidence scores, and renders bounding boxes highlighting detected contaminants. The system assesses severity (Low, Medium, High, Critical) based on particle density.
*   **Tech Stack**: PyTorch, Ultralytics YOLOv8, OpenCV.

### 2. Generative AI: Trend Analysis & Insights (Gemini & Groq)
Instead of overwhelming users with raw sensor data, Aqua Sentry uses powerful Large Language Models (LLMs) to contextualize the readings.
*   **Google Gemini**: Integrated via the `@google/generative-ai` SDK, Gemini is tasked with generating deep analytical reports. It takes historical numerical data (pH, turbidity, etc.) and evaluates overall habitability, predicting potential health impacts over time.
*   **Groq (Llama Models)**: Utilized for ultra-fast, low-latency reasoning and conversational agents. Groq's high-speed inference powers real-time alerts and instantaneous dynamic feedback on the main dashboard.

### 3. Predictive Algorithms: Carcinogenic Risk Index (CRI)
The CRI engine evaluates the potential cancer risk associated with long-term exposure to detected contaminants (like Lead, Arsenic, Cadmium).
*   **Mechanism**: Employs EPA-standardized risk calculation formulas (Chronic Daily Intake x Cancer Slope Factor). 
*   **Application**: Synthesizes sensor data to calculate an aggregate risk score (0-100). If the score exceeds safety thresholds, the system automatically flags the water source and limits access, ensuring proactive public health intervention.

### 4. Smart Remediation: Natural Purification Engine
When contamination is detected, the AI doesn't just alert the user; it prescribes solutions.
*   **Context-Aware Matching**: The purification engine analyzes the specific types of contaminants (e.g., high turbidity, biological pathogens, or heavy metals).
*   **Eco-Friendly Solutions**: Suggests localized, natural treatment methods such as Moringa Seed Coagulation (for turbidity), Solar Water Disinfection / SODIS (for pathogens), or Bio-Sand Filtration.

---

## 🏗️ System Architecture

The application is built on a scalable, decoupled architecture:

### Frontend (User Interface)
*   **Framework**: React (Vite)
*   **UI/UX**: Tailwind CSS, Framer Motion (for fluid animations), Lucide React (icons).
*   **Visualizations**: Recharts for data trends, `react-globe.gl` and `three.js` for the interactive 3D map.
*   **Real-time Communication**: `socket.io-client` for live IoT updates.

### Backend (API & Processing)
*   **Server**: Node.js & Express.js
*   **Database**: MongoDB (Atlas) via Mongoose for robust data persistence.
*   **Authentication**: JSON Web Tokens (JWT) & bcryptjs for role-based access control (Admin, Technician, User).
*   **Real-time engine**: Socket.io for bidirectional communication with the frontend and IoT devices.
*   **AI Integration**: Python child processes (for YOLO inference) and direct API integrations (Gemini, Groq)

### Edge/IoT (Hardware)
*   **Hardware**: ESP32 Microcontroller
*   **Sensors**: pH, Turbidity, Temperature, TDS probes.
*   **Connectivity**: WebSockets / MQTT to push data to the backend at high frequency.

---

## 🚀 Getting Started

### Prerequisites
*   Node.js (v18+)
*   Python 3.8+ (for Microplastic AI)
*   MongoDB Atlas Account
*   API Keys for Google Gemini and Groq.

### 1. Clone & Setup
```bash
git clone https://github.com/Poojasri37/Aqua-sentry---AI-BY-HER.git
cd Aqua-sentry---AI-BY-HER
```

### 2. Backend Environment Variables
Create a `.env` file in the `backend/` directory:
```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
GROQ_API_KEY=your_groq_api_key
GEMINI_API_KEY=your_gemini_api_key
```

### 3. Backend Setup
```bash
cd backend
npm install
# Install Python dependencies for the AI model
pip install torch torchvision torchaudio
pip install ultralytics opencv-python-headless pillow

# Start the server (Dev Mode)
npm run dev
```

### 4. Frontend Environment Variables
Create a `.env` file in the `frontend/` directory (Optional; defaults to localhost):
```env
VITE_API_URL=http://localhost:5000
```

### 5. Frontend Setup
```bash
cd ../frontend
npm install
npm run dev
```
The application will be available at `http://localhost:5173`.

---

## 🌐 Deployment (Production)

*   **Frontend**: Hosted on Vercel. Ensure `VITE_API_URL` is set to the live backend URL in the Vercel project settings.
*   **Backend**: Hosted on Render (Web Service). Ensure all environment variables from `.env` are added securely in the Render dashboard.
*   **Database**: MongoDB Atlas. Ensure the deploy server IP ranges (0.0.0.0/0) are whitelisted in your Atlas Network Access settings.

---

## 🛡️ License & Acknowledgements
Built for the AI BY HER initiative.
