# AGENTS.md

## Project Purpose

This repository contains YOLO and VLM learning examples, mainly for Google Colab workflows using KITTI-format data, webcam inference, YouTube video inference, segmentation, and OpenAI vision analysis.

Treat this as a computer-vision learning repository, not a benchmark repository.

## Rules For Future Codex Work

- Do not commit API keys, tokens, dataset credentials, model weights, or private Colab secrets.
- Use environment variables or Colab Secrets for `OPENAI_API_KEY`.
- Do not claim trained model performance unless result logs, metrics, or output artifacts are committed.
- Keep Colab cell-style scripts readable and runnable in sequence.
- Document expected external datasets instead of committing large raw datasets.
- Keep sample images only when they are intentionally used for demos.

## README Rules

- Separate object detection, segmentation, webcam inference, VLM, and YouTube workflows.
- List required packages and external data sources.
- Mark missing metrics, trained weights, and demo outputs as `TBD`.
- Include warnings for generated outputs such as `runs/`, `*.pt`, and raw KITTI data.

## Validation Rules

- If Python dependencies are available, run syntax checks on script-like files.
- If a workflow requires Google Colab, say so explicitly.
- Never fabricate mAP, FPS, accuracy, or model comparison numbers.

