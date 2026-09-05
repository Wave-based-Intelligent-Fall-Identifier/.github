## Introduce our project wify! 👋

### wify — Watch Intelligent Fall for You

[🇰🇷 한국어](./README.md) | 🇬🇧 English

> No camera, no wearable — a privacy-friendly real-time Edge AI system that detects bathroom falls among older adults using WiFi signals alone

## About the Project

**wify** is an Edge AI–based safety system that automatically detects falls in bathrooms used by older adults without any camera, and immediately sends an alert to a caregiver.

Conventional emergency call buttons are useless the moment a person loses consciousness or cannot move, and camera-based monitoring is hard to deploy in a bathroom because of the privacy concerns inherent to the space. wify solves both problems at once with **WiFi CSI (Channel State Information)**.

> An entry in the 2026 Global PIUDA Project (Korea–Japan ICT Competition for Elderly Care).

## Why It Matters

- Falls are the leading cause of injury-related death among adults aged 65 and over, and the bathroom is one of the highest-risk spaces.
- **Emergency call buttons** — useless if the person is unconscious or cannot reach them
- **CCTV** — privacy intrusion so severe that installation in a bathroom is barely feasible
- **Staff-based care** — limited by workforce shortages, the psychological embarrassment felt by older adults, and the practical impossibility of constant accompaniment

## Core Technology: WiFi CSI

CSI (Channel State Information) quantifies the changes in amplitude and phase that occur as a WiFi signal travels through a space and is reflected and scattered by walls, objects, and human bodies. When a person moves, the propagation paths shift slightly, and those shifts are recorded directly in the CSI data. wify uses the WiFi signal itself as an invisible sensor.

```
WiFi transmission (Tx) → signal changes caused by human movement → CSI extraction
→ real-time Edge AI analysis → fall decision and caregiver alert
```

## Key Differentiators

| Aspect | Description |
|---|---|
| Contactless · camera-free | Works with just two ESP32 boards. No wearable, no camera, no user interaction required |
| Environment-independent | At installation, 10 seconds of empty-room data builds a baseline, which is subtracted from live data to isolate the movement signal |
| Fully on-device AI (TinyML) | Inference runs inside the ESP32 with TensorFlow Lite Micro and an INT8-quantized 1D-CNN → no cloud, no latency, no personal data transmitted, no server cost |
| Real-time alerts | On detection, an instant alert reaches the caregiver's smartphone; early warning of abnormal behavior is also performed |

## System Architecture

- **Transmitter (Tx)** — sends CSI probing ping packets to the receiver over ESP-NOW
- **Receiver (Rx)** — receives pings → extracts CSI (amplitude/phase) → removes background noise (baseline) → extracts features and loads them into a ring buffer → classifies falls with the TinyML model → publishes alerts to the server and app over MQTT

```
ESP32(Rx) → Server → App
     App → Server → ESP32
```

The fall decision is made by **on-device AI on the ESP32**, while **MQTT** handles remote alerting and extension paths.

## Tech Stack & Development Stages

| Stage | Description | Technology |
|---|---|---|
| 1 | Building ESP32 CSI communication (Tx/Rx) | C/C++, ESP32, ESP-NOW |
| 2 | Data collection and labeling | Python |
| 3 | AI model training | TensorFlow, Keras |
| 4 | TinyML quantization | TensorFlow Lite |
| 5 | Edge AI integration and alert system | TFLite Micro, RTOS, MQTT |

## Deployment Environments

- **Care facilities** — nursing homes, senior welfare centers, disability care facilities, hospital bathrooms
- **Ordinary homes** — households of older adults living alone, families living with elderly parents, smart-home environments

Because the system consists of only two ESP32 boards, it can be installed without building any separate infrastructure.

-----

## 🏆 Awards & Activities

<p align="center">
<img alt="Encouragement Prize, 2026 Global PIUDA Project" src="assets/피우다_상.jpg" width="330" />
&nbsp;&nbsp;
<img width="330" alt="wify exhibition" src="https://github.com/user-attachments/assets/bc48f1fc-0439-45bf-a978-42e0c82da5f8" />
</p>

- **Encouragement Prize (3rd place) at the 2026 Global PIUDA Project (SW Innovation Day)** — KRW 1,000,000 prize; team Nonamed (Lee Dayeon, Yeom Sehyeon, Lee Jiwoo).
- Exhibited the 'wify' project as the 'Nonamed' club of Daedeok Software Meister High School at Daefcon.
