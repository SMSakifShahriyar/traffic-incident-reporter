traffic-incident-reporter

This project creates structured incident reports from fixed CCTV video
Flow is ingest segment detect track perception assemble report and involvement grounding

Main entry points
app.py.txt Streamlit app for full workflow and review
desktop_app.py.txt desktop launcher for reviewer UI
src/cli.py.txt command line entry with subcommands for each stage

Core source files
src/config.py.txt loads and validates config from env values
src/ingest.py.txt probes video and extracts sampled frames
src/segment.py.txt computes frame energy and proposes candidate segments
src/detect_track.py.txt runs detection tracking and exports tracks summary and annotated video
src/perception.py.txt selects keyframes and runs perception calls
src/normalize.py.txt normalizes perception output fields
src/openai_client.py.txt wraps model api calls and extracts usable outputs
src/assemble.py.txt builds schema valid incident report objects
src/involvement.py.txt grounds claim involvement links with ai or fallback logic
src/schema.py.txt defines strict pydantic schema for report json
src/reporter.py.txt validates report json files from cli
src/app_services.py.txt orchestrates pipeline stages for app and cli
src/desktop_ui.py.txt contains desktop reviewer widgets and interactions
src/__init__.py.txt package level exports

Test files
tests/test_ingest_segment.py.txt tests ingest and segmentation logic
tests/test_detect_track.py.txt tests detect track outputs and cli parsing
tests/test_assemble_report.py.txt tests report assembly behavior and edge cases
tests/test_involvement.py.txt tests involvement normalization and linking
tests/test_schema_validation.py.txt tests schema validation pass and fail cases
tests/test_app_services.py.txt tests service layer orchestration and file handling

Typical run flow
Use app.py.txt or src/cli.py.txt for ingest and segment
Run detect track and perception
Assemble reports and review outputs
Run involvement grounding and verify final report json

Practical note
For demos I usually run the Streamlit app then inspect generated artifacts in data outputs
