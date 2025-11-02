## Meeting Transcription and Entity Extraction

An easy-to-run Streamlit app that transcribes business meeting audio and automatically extracts key entities (Persons, Organizations, Locations) using state-of-the-art NLP models.

### What this app does
- **Transcribe audio** with OpenAI Whisper (Tiny) via Hugging Face Transformers
- **Extract entities** with `dslim/bert-base-NER` (aggregated into Persons, Organizations, Locations)
- **Simple UI**: upload a `.wav` file, view the transcription, and see extracted entities

---

### Tech stack
- **Python** + **Streamlit** for the UI
- **Hugging Face Transformers** for ASR and NER pipelines
- **PyTorch** backend for model inference
- **FFmpeg** required for audio handling

---

### Requirements
- Python 3.10+ (recommended)
- FFmpeg installed and available on your PATH

Python dependencies are pinned in `requirements.txt`:
- `streamlit`, `transformers`, `torch`, `pandas` (pandas is currently not used but included)

FFmpeg is listed in `packages.txt` for convenience.

---

### Installation
1) Clone or download this repository

2) Create and activate a virtual environment
```bash
python3 -m venv .venv
source .venv/bin/activate  # macOS/Linux
# On Windows (PowerShell):  .venv\\Scripts\\Activate.ps1
```

3) Install Python dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

4) Install FFmpeg
- macOS (Homebrew):
```bash
brew install ffmpeg
```
- Ubuntu/Debian:
```bash
sudo apt update && sudo apt install -y ffmpeg
```
- Windows (Chocolatey):
```bash
choco install ffmpeg
```

---

### Run the app
```bash
streamlit run app.py
```
Then open the local URL that Streamlit prints (e.g., `http://localhost:8501`).

Steps in the UI:
1) Upload a `.wav` file of your meeting audio
2) Wait for the transcription (first run will download models)
3) Review the transcript and the extracted entities (Persons, Organizations, Locations)

Notes:
- On first run, models are downloaded from Hugging Face and cached; subsequent runs are faster.
- The app uses `@st.cache_resource` to keep models loaded between interactions.

---

### Configuration tips
- Change Whisper model size (trade accuracy vs. speed) in `load_whisper_model()`:
```python
# app.py
pipeline("automatic-speech-recognition", model="openai/whisper-base")
```
- Allow additional audio formats by editing the uploader types in `app.py`:
```python
st.file_uploader("Choose a file", type=["wav", "mp3", "m4a", "flac"]) 
```
- Current NER model: `dslim/bert-base-NER` with `aggregation_strategy="simple"`. You can swap it with another NER model if needed.

---

### Project structure
```
app.py              # Streamlit app (transcription + NER)
requirements.txt    # Python dependencies
packages.txt        # System dependency hint (ffmpeg)
```

---

### Troubleshooting
- FFmpeg not found: ensure `ffmpeg` is installed and accessible in your PATH (`ffmpeg -version`).
- Slow first run: model downloads can take time; subsequent runs use the local cache.
- Torch install on Apple Silicon: default wheels support CPU and Apple Silicon (MPS) via PyTorch 2.5+. If you hit issues, consult PyTorch install notes for your platform.
- Unsupported audio type: by default only `.wav` is allowed. Add more types in the uploader if required.

---

### Privacy and data
All processing happens locally on your machine. Uploaded audio is not sent to external services; models are downloaded from Hugging Face once and cached locally.

---

### License
Add your preferred license here (e.g., MIT). If you plan to distribute, include a proper `LICENSE` file.

---

### Acknowledgements
- OpenAI Whisper models via Hugging Face `transformers`
- `dslim/bert-base-NER` for named entity recognition
- Streamlit for the rapid UI framework


