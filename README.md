# Traffic Incident Reporter

End-to-end Python pipeline for converting fixed-camera traffic video into structured, evidence-linked incident reports.

## What It Does
- Ingests a CCTV clip and samples frames.
- Proposes candidate incident segments from motion energy.
- Runs local multi-object tracking and exports track artifacts.
- Uses VLM perception on keyframes.
- Assembles strict schema-validated incident reports.
- Grounds claim involvement to tracked actor IDs.
- Supports human review in Streamlit and desktop UI.

## Pipeline
`ingest -> segment -> detecttrack -> perceive -> assemble -> involve -> review`

## Project Structure
- `app.py.txt`: Streamlit app (full workflow + review).
- `desktop_app.py.txt`: Desktop reviewer launcher.
- `src/cli.py.txt`: CLI entrypoint for all stages.
- `src/app_services.py.txt`: Orchestration and shared services.
- `src/ingest.py.txt`: Video probing and frame sampling.
- `src/segment.py.txt`: Motion-energy segment proposal.
- `src/detect_track.py.txt`: Detection/tracking outputs.
- `src/perception.py.txt`: Keyframe perception calls.
- `src/assemble.py.txt`: Report assembly and validation.
- `src/involvement.py.txt`: Actor involvement grounding.
- `src/schema.py.txt`: Strict Pydantic schema.
- `tests/`: Unit and integration-style tests.

Note: source files are stored with `.txt` suffix in this exported repo snapshot.

## Requirements
- Python 3.10+
- `OPENAI_API_KEY` for perception/involvement steps
- Install dependencies from your environment setup (recommended via virtual env)

## Quick Start
### 1) Run Streamlit app
```bash
streamlit run app.py.txt
```

### 2) Run CLI stages
```bash
python -m src.cli ingest --video data/raw_videos/cctv_006.mp4 --out data/frame_cache/cctv_006
python -m src.cli segment --frames-json data/outputs/cctv_006_frames.json --out-csv data/outputs/cctv_006_segments.csv
python -m src.cli detecttrack --video data/raw_videos/cctv_006.mp4 --out-dir data/outputs
python -m src.cli perceive --frames-json data/outputs/cctv_006_frames.json --segments-csv data/outputs/cctv_006_segments.csv --out-json data/outputs/cctv_006_perception.json --model gpt-4.1
python -m src.cli assemble --perception-json data/outputs/cctv_006_perception.json --source-video data/raw_videos/cctv_006.mp4 --out-dir data/outputs/reports
python -m src.cli involve --report-json data/outputs/reports/cctv_006_s1_collision_report.json --perception-json data/outputs/cctv_006_perception.json --frames-json data/outputs/cctv_006_frames.json --tracks-csv data/outputs/cctv_006_tracks.csv --out-json data/outputs/cctv_006_involvement.json
```

### 3) One-click rebuild
```bash
python -m src.cli rebuild --video data/raw_videos/cctv_006.mp4 --out-dir data/outputs
```

## Environment
Set API key before running perception-related stages:

```bash
export OPENAI_API_KEY="your_key_here"
```

PowerShell:
```powershell
$env:OPENAI_API_KEY="your_key_here"
```

## Outputs
Typical artifacts under `data/outputs/`:
- `*_frames.json`
- `*_segments.csv`
- `*_tracks.csv`, `*_tracks_summary.json`
- `*_perception.json`
- `reports/*_report.json`
- `*_involvement.json`
- annotated videos and rendered evidence frames

## Testing
```bash
pytest -q
```

## Notes
- This is a decision-support pipeline, not an autonomous enforcement system.
- Use reviewer workflow for final verification on high-impact incidents.
