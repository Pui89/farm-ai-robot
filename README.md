# Farm AI Robot

AI-powered robot for farm and orchard monitoring with a human-friendly multimodal interface, flood assistance, crop health analysis, and autonomous decision support.

This repository provides a starter architecture and working scaffold for a farm/orchard robot using:
- YOLO-based vision detection
- multimodal image interpretation
- LLM-based human interaction
- action planner for flood and field assistance
- ROS2-ready robotics backend design

## Robot in the orchard: concept view

```text
                         ┌─────────────────────────────┐
                         │      FARM AI ROBOT         │
                         │   Orchard Safety Monitor    │
                         └──────────────┬──────────────┘
                                        │
          ┌────────────────────────────┼────────────────────────────┐
          │                            │                            │
          │                            ▼                            │
          │                 ┌──────────────────────┐               │
          │                 │   Computer Vision    │               │
          │                 │ YOLO + camera +     │               │
          │                 │ sensors + depth     │               │
          │                 └──────────┬───────────┘               │
          │                            │                           │
          │                            ▼                           │
          │                ┌──────────────────────────┐             │
          │                │  Multimodal AI Layer     │             │
          │                │  flood / crop / worker   │             │
          │                │  scene understanding     │             │
          │                └──────────┬───────────────┘             │
          │                           │                             │
          │                           ▼                             │
          │                ┌──────────────────────────┐             │
          │                │    LLM Assistant        │             │
          │                │   "Check flood risk"    │             │
          │                │   "Which rows are wet?" │             │
          │                └──────────┬───────────────┘             │
          │                           │                             │
          │                           ▼                             │
          │                ┌──────────────────────────┐             │
          │                │ Action Planner / Alerts  │             │
          │                │ warn humans / pump /    │             │
          │                │ irrigation control      │             │
          │                └──────────────────────────┘             │
          │
          ▼

         [Farmer / worker]        [Orchard trees / rows / soil]         [Flooded low area]
                 │                           │                             │
                 │                           │                             │
                 └─────────── interacts with robot ─────── communicates ───┘

                Example human interaction:
                "Robot, check the west orchard for flood risk."
                → robot scans row images
                → detects water, worker, blocked drain
                → alerts farmer and recommends action

                     ┌────────────────┐
                     │   Robot base   │
                     │  with camera    │
                     │  + LiDAR +     │
                     │  sensors       │
                     └────────────────┘
```

## Core use cases
- Flood awareness and early warning
- Orchard row inspection
- Worker safety monitoring
- Irrigation and drainage recommendations
- Human conversation via voice/text
- Visual scene understanding from image + sensor context

## Best-version AI stack
- Vision: YOLOv8 / YOLO11
- Multimodal model: Qwen-VL, LLaVA, GPT-4o vision, or similar
- LLM: Llama 3, Mistral, Qwen, GPT-4o
- Robot runtime: ROS2
- Edge compute: NVIDIA Jetson Orin / Xavier
- Backend: Python + FastAPI
- Database: PostgreSQL + time-series storage

## Flow of operation

```text
Farmer / worker asks
        ↓
Robot captures image + sensor data
        ↓
YOLO detects objects / water / worker / crop
        ↓
Vision-language model interprets scene
        ↓
LLM creates actionable summary
        ↓
Action planner decides alert or robot action
        ↓
Farm manager receives warning and response plan
```

## Example conversation

```text
Farmer: "Check the orchard for flood risk near the west side."
Robot: "I found standing water in rows 7-10 and one blocked drainage channel. Two workers are near the edge of the wet area. Recommend stopping irrigation in that section and moving workers to the safe route."
```

## Repository structure
- `src/farm_ai_robot/` - main Python package
- `app.py` - FastAPI service entry point
- `requirements.txt` - project dependencies
- `docs/ARCHITECTURE.md` - architecture and design notes

## Quick start

1. Create a virtual environment
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

2. Install dependencies
   ```bash
   pip install -r requirements.txt
   ```

3. Run the API server
   ```bash
   uvicorn farm_ai_robot.api.app:app --reload --host 0.0.0.0 --port 8000
   ```

4. Example health check
   ```bash
   curl http://localhost:8000/health
   ```

## Example request

```bash
curl -X POST "http://localhost:8000/analyze" \
  -F "image=@examples/orchard_sample.jpg" \
  -F "prompt=Check for flood risk and worker safety issues in this orchard image."
```

## Recommended next steps
- Add real YOLO model weights and tuning for orchard data
- Connect to cameras, sensors, and drone imagery
- Add ROS2 navigation and actuator control
- Add human speech interface with local language support
- Add a farmer dashboard for alerts and recommendations

## License
This project is intended as an open starter for agricultural AI research and field experimentation.
