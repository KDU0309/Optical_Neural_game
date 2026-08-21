# Optical Neural Pong

**Human vs Optical Neural Computer**

브라우저에서 바로 실행되는 단일 파일 기반 광학 신경망 Pong 게임입니다.
사람은 왼쪽 패들을 조작하고, 오른쪽 패들은 **하나의 복소 광학장 → 광학 위상 변조 → 간섭 → 3개 Hidden Optical Nodes → 3개 Optical Detectors → Action** 흐름으로 제어됩니다.

---

## Demo / 데모

**Hugging Face**

[https://huggingface.co/spaces/kdu0309/Optical_Neural_game](https://huggingface.co/spaces/kdu0309/Optical_Neural_game)

**GitHub**

[https://github.com/KDU0309/Optical_Neural_game](https://github.com/KDU0309/Optical_Neural_game)

---

## Game / 게임

```text
                 OPTICAL NEURAL PONG

        HUMAN                    OPTICAL AI
          │                         │
          █          ●              █
          │           →             │
          │                         │
          │                         │

        W / S                  Optical Controller
        ↑ / ↓
```

왼쪽:

```text
HUMAN PLAYER
```

오른쪽:

```text
OPTICAL NEURAL AI
```

---

## Controls / 조작

```text
W       Move Up
S       Move Down

↑       Move Up
↓       Move Down
```

게임 버튼:

```text
START GAME
PAUSE
RESET
```

---

# Optical Controller / 광학 컨트롤러

게임 상태를 하나의 복소 광학장으로 인코딩합니다.

```text
PONG STATE
     ↓
ONE COMPLEX OPTICAL FIELD
     ↓
OPTICAL LAYER 1
     ↓
INTERFERENCE
     ↓
3 OPTICAL HIDDEN NODES
     ↓
OPTICAL LAYER 2
     ↓
LEFT / RIGHT / STAY DETECTORS
     ↓
OPTICAL AI ACTION
```

기본 광학 표현:

```text
E = A · exp(iφ)
```

위상 변조:

```text
E' = E · exp(iΔφ)
```

간섭 검출:

```text
I+ = |E + R|²
I- = |E - R|²
```

Balanced detection:

```text
D = (I+ - I-) / 4
```

---

# AI Decision / AI 결정

광학 시스템은 세 개의 출력 검출기를 사용합니다.

```text
LEFT
RIGHT
STAY
```

검출 결과와 현재 Pong 상태를 이용하여 Optical AI 패들의 움직임을 계산합니다.

```text
BALL STATE
    ↓
OPTICAL FIELD
    ↓
OPTICAL DETECTION
    ↓
TARGET POSITION
    ↓
OPTICAL PADDLE CONTROL
```

---

# Optical Hidden Nodes / 광학 Hidden Nodes

첫 번째 광학 계층에서는 하나의 복소 광학장이 여러 위상 경로로 분기되고 서로 간섭합니다.

```text
                 ONE FIELD
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
        PATH 1    PATH 2    PATH 3
          │         │         │
          └───── INTERFERENCE ─┘
                    │
             H1 / H2 / H3
```

각 Hidden Node는 광학 검출기를 통해 비선형 신호를 생성합니다.

---

# Optical Output / 광학 출력

Hidden Optical Nodes:

```text
H1
H2
H3
```

출력 Detector:

```text
LEFT
RIGHT
STAY
```

최종적으로 광학 검출 결과가 AI Paddle 제어에 사용됩니다.

---

# Collision Detection / 충돌 판정

빠르게 움직이는 공이 한 프레임에서 패들을 통과하는 문제를 줄이기 위해 **swept collision detection**을 사용합니다.

```text
Previous Ball Position
          ↓
Current Ball Position
          ↓
Movement Segment
          ↓
Paddle Plane Intersection
          ↓
Exact Collision Y
          ↓
Paddle Range Test
          ↓
Reflection
```

따라서 단순히 현재 위치만 검사하지 않고 공의 이동 경로와 패들의 교차점을 계산합니다.

패들에서 맞은 위치에 따라 반사각도 달라집니다.

```text
      ↖   ↑   ↗
       \  │  /
        \ │ /
--------- ● ---------
        PADDLE
```

---

# Online Optical Learning / 온라인 광학 학습

게임에서 발생하는 reward가 광학 파라미터 업데이트에 사용됩니다.

```text
GAME EVENT
    ↓
REWARD
    ↓
OPTICAL UPDATE
    ↓
PHASE / OUTPUT PARAMETERS
```

게임 중 현재 광학 계산과 reward를 계속 표시합니다.

```text
Inference Count
Optical Update Count
Reward
Detector Values
Hidden Node Values
```

---

# Visualization / 시각화

게임 화면과 광학 연산 과정이 동시에 표시됩니다.

```text
ONE COMPLEX OPTICAL FIELD
          ↓
PHASE / INTERFERENCE
          ↓
H1    H2    H3
          ↓
LEFT  RIGHT  STAY
```

게임 화면에서:

```text
HUMAN
OPTICAL AI
BALL
AI TARGET
SCORE
```

을 확인할 수 있습니다.

광학 패널에서는:

```text
Complex Field
Hidden Nodes
Optical Detectors
Action
Inference Count
Optical Updates
Reward
```

를 실시간으로 확인할 수 있습니다.

---

# Browser Architecture / 브라우저 구조

```text
index.html
│
├── Pong Environment
│
├── Optical Neural Controller
│   ├── Complex Field
│   ├── Phase Modulation
│   ├── Interference
│   ├── Hidden Optical Nodes
│   ├── Optical Detectors
│   └── Online Optical Update
│
├── Swept Collision Engine
│
├── Canvas Visualization
│
└── Game Loop
```

외부 JavaScript 라이브러리 없이 실행됩니다.

```text
External JavaScript Libraries : 0
Python                       : 0
NumPy                        : 0
PyTorch                      : 0
Backend                      : 0
```

---

# Run Locally / 로컬 실행

저장소를 클론합니다.

```bash
git clone https://github.com/KDU0309/Optical_Neural_game.git
cd Optical_Neural_game
```

`index.html`을 브라우저에서 직접 실행하거나 간단한 로컬 서버를 사용할 수 있습니다.

```bash
python -m http.server 8000
```

브라우저:

```text
http://localhost:8000
```

---

# Hugging Face Static Demo

이 프로젝트는 Static Space에서 실행할 수 있도록 모든 계산을 브라우저 JavaScript로 구현했습니다.

```text
Python Server : ✗
Gradio        : ✗
Backend       : ✗
JavaScript    : ✓
Canvas        : ✓
Static Space  : ✓
```

Demo:

[https://huggingface.co/spaces/kdu0309/Optical_Neural_game](https://huggingface.co/spaces/kdu0309/Optical_Neural_game)

---

# Scientific Scope / 과학적 범위

이 프로젝트는 **수치적으로 구현된 광학 계산 모델**입니다.

```text
Software Optical Simulation
        ≠
Physical Optical Hardware
```

따라서 이 게임의 실행 속도나 AI 성능을 실제 레이저, SLM, 광학 렌즈 또는 CMOS 하드웨어의 성능으로 해석하지 않습니다.

프로젝트의 목적은 다음과 같은 계산 구조를 인터랙티브하게 표현하는 것입니다.

```text
Information
    ↓
Optical Representation
    ↓
Phase
    ↓
Interference
    ↓
Detection
    ↓
Decision
    ↓
Action
```

---

# Project Structure / 프로젝트 구조

```text
Optical_Neural_game/
│
├── index.html
└── README.md
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

**EN:** Optical Neural Pong is a standalone browser demonstration of a numerical optical neural controller playing Pong against a human player.

**KR:** Optical Neural Pong은 수치적으로 구현된 광학 신경망 컨트롤러가 사람과 Pong을 대결하는 브라우저 기반 데모입니다.

```text
HUMAN
  ↓
PADDLE

PONG STATE
  ↓
ONE COMPLEX OPTICAL FIELD
  ↓
OPTICAL LAYER 1
  ↓
INTERFERENCE
  ↓
HIDDEN NODES
  ↓
OPTICAL LAYER 2
  ↓
DETECTORS
  ↓
OPTICAL AI
  ↓
PADDLE
```

**Optical Neural Pong — Human vs Optical Neural Computer**
