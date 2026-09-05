# Zenith-Vector

## JARVIS-Omni Autonomous Operating Shell (PRD)

**Artifact:** Portable self-contained binary (`JARVIS-Omni.exe`)  
**Footprint Targets:** `<= 220 MB RAM` idle / `<= 300 MB RAM` active  
**Design Inspiration:** OpenJarvis (agentic loops), Avinashb722/JARVIS (vision/biometrics/phone link), Iron Man F.R.I.D.A.Y. (tactical HUD & diagnostics), Ultron (autonomous multi-threaded self-execution & system introspection)

---

## 1) Multi-Model Task-Specialized Architecture (MoE Orchestrator)

Tasks are routed to specialized endpoints based on token footprint, latency, and capability.

```text
                          User Input / Trigger
                                   │
                                   ▼
                   [ Intent & Complexity Router ]
                  (Local Trie / Regex / Micro-SLM)
                                   │
  ┌──────────────────┬─────────────┴─────────────┬──────────────────┐
  │ (< 5ms)          │ (Free/Instant)            │ (High Reasoning) │ (Vision/Multimodal)
  ▼                  ▼                           ▼                  ▼
[ Local Engine ] [ Router Tier 1 ]         [ Router Tier 2 ]  [ Router Tier 3 ]
- Rule regex     - Groq / Llama-3-8B       - DeepSeek-V3 /    - Qwen2-VL /
- Win32 API      - Fast function calling      Claude-3.5         Gemini Flash
- SQLite FTS5    - Slot filling                Sonnet           - OCR / UI targeting
```

- **Router Tier 0 (Local Engine, 0 MB network, <5 ms):**
  - Handles regex commands, app lifecycle requests, volume/system controls, and offline notes directly.
- **Router Tier 1 (Low-latency action engine):**
  - Dedicated to real-time function calling, parameter extraction, and slot filling for MicroDroid automation.
- **Router Tier 2 (Deep reasoning):**
  - Used for architecture planning, code synthesis, multi-hop retrieval, and step-by-step solving.
- **Router Tier 3 (Perception/vision):**
  - Processes desktop captures to locate controls, read dialogs, and navigate visually when accessibility metadata is absent.

---

## 2) Self-Awareness, Telemetry & Autonomy Subsystems

- **Autonomous self-monitoring daemon (Ultron Core):**
  - Tracks CPU temperature, memory leaks, disk I/O, battery health, and active network connections.
  - Performs proactive intervention (alerts and optional cleanup) when thresholds are exceeded.
- **Context/environment awareness (F.R.I.D.A.Y. protocol):**
  - Foreground app watcher adapts persona/mode (e.g., coding persona for VS Code, silent gaming mode for full-screen DirectX/Vulkan).
  - System event triggers for USB attach, audio route changes, and AC power transitions.
- **Persistent memory vault (vectorless episodic memory):**
  - Embedded SQLite for history, preferences, aliases, and local secrets.
  - AES-GCM-256 encryption for sensitive records.
  - Retrieval via BM25 + reciprocal rank fusion (RRF), avoiding vector-DB memory overhead.

---

## 3) Visual UI/UX & Sci-Fi Rendering Modes

- **Rendering stack:** Hardware-accelerated Canvas/WebGL through Microsoft Edge WebView2 (`~30 MB` base RAM).
- **Modes:**
  - **J.A.R.V.I.S. Mode:** cyan holo visualizer with audio-reactive arc reactor animation.
  - **F.R.I.D.A.Y. Mode:** tactical HUD, diagnostics gauges, CPU/RAM charts.
  - **Ultron Mode:** crimson node-graph visualization for workers/automations.
- **Window behavior:**
  - Frameless, transparent floating HUD with click-through toggle.
  - Dockable to tray or screen edges.
  - Frame caps: 60 FPS active, throttled to 5 FPS (or sleep) when minimized.

---

## 4) Visual Automation Engine (MicroDroid Desktop)

Low-code automation canvas with trigger → constraint → action flow.

```text
+-------------------------------------------------------------+
|                     AUTOMATION CANVAS                       |
|                                                             |
|  [ TRIGGER ]               [ CONSTRAINT ]       [ ACTION ]  |
|  USB Inserted (D:)  ────►  Free Space > 1GB ──► Run Cmd     |
|  Hotkey (Ctrl+Alt+J) ───► Time 22:00-06:00 ──► Night Mode   |
+-------------------------------------------------------------+
```

- **Triggers:** hotkeys, wake-words, filesystem watcher, process lifecycle, inbound local webhooks (`http://localhost:8765/trigger/<id>`).
- **Actions:** Win32 input automation, app window/process control, script execution (PowerShell/CMD/Python), local credential injection without sending secrets to remote LLM context.

---

## 5) Multi-Language Build + Single-EXE Bundling Pipeline

```text
Frontend (Vite/WebGL) ─► static assets
Rust core (windows-rs, tokio, axum) ─► native engine
Gateway (Go/Rust) ─► embedded HTTP/WS server
Embed assets (include_dir!) ─► self-contained package
Tauri v2 / Cargo packager ─► JARVIS-Omni.exe (~25-35 MB)
```

| Step | Operation | Output |
|---|---|---|
| 1. UI Assets | Vite compiles Svelte/Vanilla JS + Lottie into minified bundle | `/dist` (HTML, JS, shaders) |
| 2. Core Engine | Rust (`windows-rs`, `tokio`, `axum`) compiles system controllers and API router | Native object code |
| 3. Embedding | Asset files embedded via `include_dir!` | Self-contained executable |
| 4. Packaging | Linker produces standalone binary with zero external DLL requirements | `JARVIS-Omni.exe` |

---

## 6) Non-Functional Specs & Guardrails

- **Cold startup:** `<= 1.2s` to interactive HUD.
- **Minimized footprint target:** `<= 45 MB` RAM and near-zero CPU during idle.
- **Network isolation:**
  - Sensitive keys and automation scripts remain local.
  - Outbound requests must use user-configured base URLs/endpoints only.
