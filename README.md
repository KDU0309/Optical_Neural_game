# Optical Neural Breakout

**Standalone Browser Optical Computing Game**

**EN:** Optical Neural Breakout is a browser-based numerical optical-computing game that combines a Breakout environment with a single complex optical field, phase modulation, interference, optical hidden nodes, optical detection, and online optical parameter updates.

**KR:** Optical Neural Breakout은 Breakout 게임 환경에 하나의 복소 광학장, 위상 변조, 간섭, 광학 Hidden Node, 광학 검출 및 온라인 광학 파라미터 업데이트를 결합한 브라우저 기반 수치 광컴퓨팅 게임입니다.

---

## Demo / 데모

**Hugging Face**

[https://huggingface.co/spaces/kdu0309/Optical_Neural_game](https://huggingface.co/spaces/kdu0309/Optical_Neural_game)

**GitHub**

[https://github.com/KDU0309/Optical_Neural_game](https://github.com/KDU0309/Optical_Neural_game)

---

# Overview / 개요

**EN:** The game represents the controller using an optical-computing pipeline rather than a conventional neural-network implementation.

**KR:** 이 게임은 일반적인 신경망 구현 대신 광컴퓨팅 파이프라인으로 게임 컨트롤러를 표현합니다.

```text
BREAKOUT STATE
      │
      ▼
ONE COMPLEX OPTICAL FIELD
      │
      ▼
OPTICAL PHASE MODULATION
      │
      ▼
INTERFERENCE
      │
      ▼
3 OPTICAL HIDDEN NODES
      │
      ▼
3 OPTICAL DETECTORS
      │
      ▼
LEFT / RIGHT / STAY
      │
      ▼
PADDLE CONTROL
      │
      ▼
BREAKOUT
      │
      ▼
REWARD
      │
      ▼
ONLINE OPTICAL UPDATE
```

---

# Game / 게임

Breakout 환경은 다음으로 구성됩니다.

```text
┌──────────────────────────────────────────────┐
│ ========== BRICK FIELD ====================  │
│ ========== BRICK FIELD ====================  │
│ ========== BRICK FIELD ====================  │
│                                              │
│                     O                        │
│                    ╱                         │
│                                              │
│                  =========                   │
└──────────────────────────────────────────────┘
```

게임 요소:

```text
Ball
Paddle
Bricks
Walls
Lives
Score
Reward
```

목표:

```text
50 BRICKS
   ↓
CLEAR
```

실패 시:

```text
3 LIVES
   ↓
RESET
```

---

# Optical Computing Pipeline / 광학 계산 파이프라인

게임 상태는 하나의 복소 광학장으로 변환됩니다.

```text
GAME STATE
   │
   ├── Ball X
   ├── Ball Y
   ├── Ball VX
   ├── Ball VY
   ├── Paddle Position
   └── Remaining Bricks
             │
             ▼
      PHASE ENCODING
             │
             ▼
       COMPLEX FIELD
```

기본 형태:

```text
E = A · exp(iφ)
```

여기서:

```text
A = optical amplitude
φ = optical phase
E = complex optical field
```

---

# Phase Modulation / 위상 변조

광학 파라미터는 입력 광장에 위상 변조를 적용합니다.

```text
Eout = Ein · exp(iΔφ)
```

여러 개의 광학 경로가 서로 다른 phase를 적용합니다.

```text
                 ONE FIELD
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      PATH 1      PATH 2      PATH 3
        │           │           │
        └───────────┼───────────┘
                    ↓
               INTERFERENCE
```

---

# Optical Hidden Nodes / 광학 Hidden Nodes

첫 번째 광학 계층에서 3개의 Hidden Optical Nodes를 생성합니다.

```text
ONE COMPLEX FIELD
        │
        ├──── Phase Path 1 ──── H1
        │
        ├──── Phase Path 2 ──── H2
        │
        └──── Phase Path 3 ──── H3
```

각 노드는 coherent interference 이후 detector signal을 생성합니다.

```text
H1
H2
H3
```

---

# Optical Detection / 광학 검출

Balanced coherent detection을 사용합니다.

```text
I+ = |E + R|²
I- = |E - R|²
```

검출 신호:

```text
D = (I+ - I-) / 4
```

이를 통해 광학 필드의 위상/간섭 정보를 출력 신호로 변환합니다.

---

# Optical Output / 광학 출력

두 번째 광학 계층은 세 가지 출력을 생성합니다.

```text
LEFT
RIGHT
STAY
```

구조:

```text
H1 ─┐
H2 ─┼── OPTICAL LAYER 2 ── LEFT
H3 ─┘

H1 ─┐
H2 ─┼── OPTICAL LAYER 2 ── RIGHT
H3 ─┘

H1 ─┐
H2 ─┼── OPTICAL LAYER 2 ── STAY
H3 ─┘
```

이 결과는 패들의 제어 신호로 사용됩니다.

---

# Optical Ray Prediction / 광학 경로 예측

현재 공의 위치와 속도를 이용하여 예상 경로를 계산합니다.

```text
BALL
  ↓
CURRENT VELOCITY
  ↓
WALL REFLECTION
  ↓
PADDLE INTERSECTION
  ↓
OPTICAL TARGET
```

화면에서는 예상 광학 경로를 점선으로 표시합니다.

```text
O
 ╲
  ╲
   ╲
    ╲
     ╲
      V
=============
```

빨간 표시가 광학적으로 계산된 목표 위치입니다.

---

# Paddle Control / 패들 제어

최종 control은 광학 출력과 계산된 목표 위치를 함께 사용합니다.

```text
OPTICAL DETECTORS
       +
OPTICAL TARGET
       ↓
    CONTROL
       ↓
    PADDLE
```

Control 범위:

```text
-1.0  ←────────→  +1.0
LEFT                 RIGHT
```

Breakout에서는 패들이 좌우로 이동합니다.

---

# Online Optical Learning / 온라인 광학 학습

게임 이벤트에 따라 reward가 발생합니다.

예:

```text
BRICK HIT
   ↓
POSITIVE REWARD
```

```text
PADDLE HIT
   ↓
POSITIVE REWARD
```

```text
BALL MISS
   ↓
NEGATIVE REWARD
```

```text
GAME WIN
   ↓
LARGE POSITIVE REWARD
```

```text
GAME LOSS
   ↓
NEGATIVE REWARD
```

Reward는 광학 파라미터의 작은 업데이트에 사용됩니다.

```text
REWARD
  ↓
LOCAL OPTICAL UPDATE
  ↓
PHASE PARAMETERS
  ↓
NEXT OPTICAL COMPUTATION
```

---

# Visualization / 시각화

게임 화면과 광학 계산을 동시에 보여줍니다.

왼쪽:

```text
BREAKOUT
```

오른쪽:

```text
OPTICAL COMPUTER
```

광학 패널:

```text
ONE COMPLEX OPTICAL FIELD
          ↓
PHASE MODULATION
          ↓
INTERFERENCE
          ↓
H1   H2   H3
          ↓
LEFT RIGHT STAY
```

실시간으로 표시되는 값:

```text
Score
Lives
Active Bricks
Paddle X
Ball Position
Optical Target
Control
Complex Optical Field
Hidden Node Values
Detector Values
Inference Count
Optical Update Count
Reward
```

---

# Browser Implementation / 브라우저 구현

이 프로젝트는 Static 웹 환경을 목표로 설계되었습니다.

```text
HTML
CSS
JavaScript
Canvas
```

외부 머신러닝 라이브러리를 필요로 하지 않습니다.

```text
Python      : NO
NumPy       : NO
PyTorch     : NO
DPC         : NO
Gradio      : NO
External JS : NO
```

전체 게임과 광학 시뮬레이션은 `index.html` 하나에서 실행됩니다.

---

# Project Structure / 프로젝트 구조

```text
Optical_Neural_game/
│
├── index.html
│
└── README.md
```

---

# Run Locally / 로컬 실행

저장소를 가져옵니다.

```bash
git clone https://github.com/KDU0309/Optical_Neural_game.git
cd Optical_Neural_game
```

간단한 로컬 서버:

```bash
python -m http.server 8000
```

브라우저에서:

```text
http://localhost:8000
```

를 엽니다.

또는 `index.html`을 직접 브라우저에서 열 수도 있습니다.

---

# Hugging Face Static Space

Static Space에서는 Python 서버 없이 브라우저에서 직접 실행됩니다.

```text
index.html
   ↓
Browser
   ↓
Canvas
   ↓
Optical Simulation
   ↓
Breakout
```

Demo:

[https://huggingface.co/spaces/kdu0309/Optical_Neural_game](https://huggingface.co/spaces/kdu0309/Optical_Neural_game)

---

# GitHub Pages

GitHub Pages를 활성화하면 동일한 `index.html`을 웹에서 실행할 수 있습니다.

```text
GitHub Repository
        ↓
      main
        ↓
   index.html
        ↓
 GitHub Pages
        ↓
 Web Browser
```

Repository:

[https://github.com/KDU0309/Optical_Neural_game](https://github.com/KDU0309/Optical_Neural_game)

---

# Optical Parameters / 광학 파라미터

주요 시뮬레이션 파라미터:

```javascript
WAVELENGTH = 632.8e-9
SLM_PIXEL = 8.0e-6

OPTICAL_PHASE_SCALE = 0.31
REFERENCE_AMPLITUDE = 1.0

LEARNING_RATE = 0.0007
WEIGHT_LIMIT = 4.0
```

게임 파라미터:

```javascript
BRICK_ROWS = 5
BRICK_COLS = 10

PADDLE_WIDTH = 18

BALL_MIN_SPEED = 0.78
BALL_MAX_SPEED = 1.30

MAX_LIVES = 3
```

---

# Architecture / 전체 구조

```text
                    BREAKOUT
                       │
                       ▼
               STATE ENCODING
                       │
                       ▼
             COMPLEX OPTICAL FIELD
                       │
                       ▼
                PHASE MODULATION
                       │
              ┌────────┼────────┐
              ▼        ▼        ▼
             H1       H2       H3
              │        │        │
              └────────┼────────┘
                       ▼
                   INTERFERENCE
                       │
                       ▼
               OPTICAL DETECTION
                       │
              ┌────────┼────────┐
              ▼        ▼        ▼
             LEFT     RIGHT    STAY
              │        │        │
              └────────┼────────┘
                       ▼
                 PADDLE CONTROL
                       │
                       ▼
                    BREAKOUT
                       │
                       ▼
                     REWARD
                       │
                       ▼
              OPTICAL PARAMETER
                   UPDATE
```

---

# Scientific Scope / 과학적 범위

**EN:** This project is a numerical simulation of optical computation implemented in JavaScript. It does not represent measurements from physical laser, SLM, lens, or CMOS hardware.

**KR:** 이 프로젝트는 JavaScript로 구현된 광컴퓨팅 수치 시뮬레이션입니다. 실제 레이저, SLM, 렌즈 또는 CMOS 하드웨어에서 측정한 결과를 의미하지 않습니다.

```text
Numerical Optical Simulation
             ≠
Physical Optical Hardware
```

특히:

```text
Browser execution time
        ≠
Physical propagation time of light
```

따라서 이 프로젝트의 목적은 실제 하드웨어 성능을 주장하는 것이 아니라 **광학적 표현과 계산 구조를 인터랙티브하게 탐구하는 것**입니다.

---

# Features / 기능

```text
✓ Breakout environment
✓ One complex optical field
✓ Phase modulation
✓ Coherent interference
✓ 3 optical hidden nodes
✓ 3 optical detectors
✓ LEFT / RIGHT / STAY outputs
✓ Optical ray prediction
✓ Online optical learning
✓ Real-time optical visualization
✓ Real-time telemetry
✓ Browser-only execution
✓ Hugging Face Static compatible
✓ GitHub Pages compatible
✓ No external ML framework
```

---

# License / 라이선스

MIT License

```text
Copyright (c) 2026 KDU0309

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```

---

# Summary / 요약

**EN:** Optical Neural Breakout is a standalone browser demonstration that transforms a Breakout game state into a numerical optical computation and uses the resulting detector signals to control the paddle.

**KR:** Optical Neural Breakout은 Breakout 게임 상태를 수치적인 광학 계산으로 변환하고, 광학 검출 결과를 이용하여 패들을 제어하는 독립형 브라우저 데모입니다.

```text
BREAKOUT
   ↓
ONE COMPLEX FIELD
   ↓
PHASE
   ↓
INTERFERENCE
   ↓
OPTICAL HIDDEN NODES
   ↓
OPTICAL DETECTORS
   ↓
ACTION
   ↓
PADDLE
   ↓
REWARD
   ↓
OPTICAL UPDATE
```

**Optical Neural Breakout — Numerical Optical Computing in the Browser**
