# MPC-based Adaptive Cruise Control — Simulink Models

> MATLAB/Simulink와 PreScan 환경에서 ACC(Adaptive Cruise Control) 제어기를 구성하고, MPC 기반 제어 모델을 코드 생성 및 외부 통신 단계까지 확장한 프로젝트 저장소입니다.

이 저장소는 **MPC 기반 ACC 제어기 설계 및 차량용 MCU 연동 검증 프로젝트**에서 사용한 Simulink 모델을 용도별로 정리한 것입니다.

관련 연구에서는 PI 제어기와 MPC 제어기를 비교했으며, PreScan 기반 주행 시나리오에서 선행 차량 속도 추종과 차량 간 거리 유지 성능을 확인했습니다. 이후 Simulink 모델을 Embedded Coder로 코드 생성하여 Infineon AURIX 프로젝트에 통합하고, Ethernet 기반으로 시뮬레이터와 MCU를 연동해 검증했습니다.

---

## Project Flow

```text
PreScan Driving Scenario
        │
        ▼
MATLAB / Simulink
        │
        ├─ Baseline ACC model
        ├─ MPC-based ACC model
        │
        ▼
Embedded Coder
        │
        ▼
Generated ACC code
        │
        ▼
Infineon AURIX integration
        │
        ▼
Ethernet-based simulator ↔ MCU validation
```

---

## Repository Structure

```text
acc-mpc-simulink/
├── models/
│   ├── non_ACC.slx       # MPC ACC 블록을 사용하지 않은 기준 모델
│   ├── acc_mpc.slx       # Adaptive Cruise Control System(MPC) 블록 적용 모델
│   └── tcp_ip.slx        # TCP/IP 통신을 포함한 시뮬레이션 모델
├── codegen/
│   └── Generation.slx    # 코드 생성을 위해 분리한 ACC System 모델
├── .gitignore
└── README.md
```

---

## Simulink Models

### 1. `models/non_ACC.slx`

MPC 기반 Adaptive Cruise Control 블록을 적용하지 않은 기준 Simulink/PreScan 모델입니다.

### 2. `models/acc_mpc.slx`

Simulink의 **Adaptive Cruise Control System** 블록을 적용한 MPC 기반 ACC 모델입니다.

연구에서 사용한 MPC는 차량 간 거리와 상대 속도를 입력으로 사용해 목표 거리를 유지하도록 구성했습니다.

### 3. `codegen/Generation.slx`

ACC 제어 로직을 별도로 구성해 **Embedded Coder 기반 Autocode Generation**에 사용한 모델입니다.

프로젝트에서는 생성된 코드를 AURIX Development Studio의 Infineon AURIX 프로젝트에 통합해 MCU 환경에서 검증했습니다.

### 4. `models/tcp_ip.slx`

외부 시스템과의 연동을 위해 **TCP/IP Receive** 등 통신 블록을 포함한 Simulink/PreScan 모델입니다.

AURIX와 시뮬레이션 환경 간 데이터 연동을 위한 통신 구성을 확인할 수 있습니다.

---

## Control Design Context

관련 연구에서는 ACC 제어 성능 비교를 위해 PI 제어기와 MPC 제어기를 구성했습니다.

- **PI Controller**
  - 차량 간 거리와 목표 거리의 오차를 기반으로 제어 입력 계산
  - 비례항과 적분항을 이용해 지속적인 거리 오차 보정

- **MPC Controller**
  - 차량 간 거리와 상대 속도를 입력으로 사용
  - 미래 상태를 고려해 제어 입력을 계산하는 방식으로 ACC 구성

PreScan 주행 시나리오에서는 선행 차량에 시간에 따라 변화하는 속도 프로파일을 적용하여 추종 성능을 비교했습니다.

---

## Validation Flow

프로젝트의 검증 흐름은 다음과 같습니다.

1. PreScan에서 선행·후행 차량 기반 ACC 주행 시나리오 구성
2. MATLAB/Simulink에서 PI 및 MPC 기반 ACC 모델 구성
3. 시뮬레이션에서 속도 추종, 차량 간 거리 및 제어 입력 비교
4. Embedded Coder를 이용해 ACC 제어 알고리즘 코드 생성
5. 생성 코드를 AURIX Development Studio 프로젝트에 통합
6. Ethernet 통신을 이용해 AURIX MCU와 PreScan 시뮬레이션 환경 연동

연구 결과에서는 MPC가 PI 제어기보다 선행 차량의 속도 변화에 더 빠르게 대응하고 안정적인 거리 유지 성능을 보였으며, throttle/brake 제어 입력의 변화도 줄어드는 경향을 확인했습니다.

---

## Development Environment

- MATLAB / Simulink
- PreScan
- Model Predictive Control
- Embedded Coder
- Infineon AURIX
- AURIX Development Studio
- Ethernet / TCP-IP communication

> 업로드된 `.slx` 파일은 프로젝트 당시 MATLAB/Simulink 환경에서 작성되었습니다. PreScan에서 생성된 차량 및 시나리오 관련 블록이 포함되어 있어 동일한 환경에서 모델을 열기 위해 추가 구성 요소가 필요할 수 있습니다.

---

## Related Research

**모델 예측 제어 기반 적응형 순항 제어기 설계 및 차량용 MCU와 자율주행 시뮬레이터를 통한 검증**  
*Model Predictive Control-Based Adaptive Cruise Controller Design and Validation Using Automotive MCU and Autonomous Driving Simulator*

- 이하정, 박가현, 김정민, 김정윤, 정구민
- PI 기반 ACC와 MPC 기반 ACC의 시뮬레이션 성능 비교
- Embedded Coder를 통한 AURIX 코드 통합
- Ethernet 기반 AURIX–PreScan 연동 검증

---

## Notes

- `.slx`는 binary Simulink model 형식이므로 GitHub에서 코드 형태의 diff는 제한적입니다.
- 외부 PreScan 리소스 및 로컬 시뮬레이션 설정 파일은 이 저장소에 포함되어 있지 않을 수 있습니다.
