# Team FollowArmMe - Sensor-Based Real-Time Remote Robotic Arm Control

# 위치 센서를 이용한 로봇 팔 실시간 원격 제어
“사람 손의 정밀한 움직임을 로봇 팔로 재현하여 원격 제어를 실현하다.”

---

## 🎯 목표와 개요

### 프로젝트 목표
- 위치 센서(MPU9250)와 압력 센서를 기반으로 손의 움직임과 그리퍼 제어를 실시간으로 로봇 팔에 반영
- 의료, 재활, 산업 자동화 등 다양한 분야에 응용 가능한 직관적인 원격 로봇 제어 기술 구현

### 개요
- IMU 센서를 활용한 손의 roll, pitch, yaw 측정 및 보정
- 압력 센서를 이용한 그리퍼(집게) 정밀 제어
- PCA9685를 통한 다채널 서보 모터 제어
- ESP32 ↔ Raspberry Pi 간 Bluetooth 실시간 통신

---

## 🛠 시스템 구성

### 하드웨어
- **MPU-9250**: 9축 IMU 센서 (가속도, 자이로, 자기계)
- **압력 센서**: 손가락 굽힘 정도 측정
- **서보 모터 (MG996R)**: 로봇 팔 관절 및 그리퍼 제어
- **PCA9685**: 16채널 PWM 드라이버로 다채널 모터 제어
- **ESP32**, **Raspberry Pi**, **MeanWell LRS-75-5 파워 서플라이**

### 소프트웨어
- ESP32: IMU 및 압력 센서 데이터 수집, EMA 필터 적용, Bluetooth 전송
- Raspberry Pi: 수신 데이터 기반 실시간 서보 모터 제어 (ServoKit, Python)
- Fusion 360: 로봇 팔 모델링 및 3D 출력 설계
- CircuitPython ServoKit 라이브러리

<p align="center">
  <img src="./images/circuit_setup_real.png">
  <br>
  <em>실제 회로 및 서보 모터 전체 구성 모습</em>
</p>

<p align="center">
  <img src="./images/circuit_diagram.png">
  <br>
  <em>회로 다이어그램 및 전원/모터 연결 흐름</em>
</p>

---

## ⚙️ 기능 및 흐름

### 핵심 기능
- IMU 기반 손 움직임 각도 추정 (roll, pitch, yaw) 및 실시간 반영
- 압력 센서 기반 그리퍼 강도 및 각도 제어
- Bluetooth 무선 통신을 통한 실시간 제어
- 부드러운 모션 구현 (step 함수 + 시간 지연)
- 안전성 강화를 위한 각도 제한 및 필터링

### 동작 흐름
1. ESP32가 센서 데이터를 실시간으로 수집
2. EMA(이중 지수이동평균) 및 Complementary Filter 적용으로 값 안정화
3. Bluetooth로 Raspberry Pi에 전송
4. Raspberry Pi가 PCA9685와 연동해 다채널 서보 모터 제어
5. 그리퍼 포함 로봇 팔 실시간 제어

<p align="center">
  <img src="./images/cad_model.png">
  <br>
  <em>Fusion 360 기반 로봇 팔 전체 CAD 모델</em>
</p>

---

## 🗂 프로젝트 구조
robot-arm-project/
├── software/
│   ├── glove/        # IMU 및 압력 센서 장갑 코드
│   └── controller/   # Raspberry Pi 기반 모터 제어 코드
├── hardware/
│   ├── cad-model/    # Fusion 360 3D 모델 및 출력 설계 파일
│   └── 회로 및 배선 자료
├── docs/            # 보고서 및 발표 자료
└── README.md

---

## 👥 팀명 및 역할
- **팀명**: 팔로미
- **팀원**
  - 신통화: Bluetooth 통신 모듈, 압력 센서 제어
  - 박준호: ESP32–Raspberry Pi 통신, 압력 센서 및 서보 모터 제어, 전체 제어 로직 개발
  - 최찬희: 서보 모터 연동, 로봇 집게 테스트 및 통합
  - 이행승: Fusion 360 설계, 3D 출력, 최종 조립

---

## 🛠 제작 과정 및 문제 해결

### 하드웨어 문제 해결
- 출력 중 스파게티 오류 → 프린터 베드 온도 상승 및 환경 개선
- 초기 그리퍼 설계 불안정 → 삼각형 끝 → 직사각형, 고무 팁 추가 등 구조 개선
- 모터 초기 각도 문제 → 각 채널별 초기값 보정

### 소프트웨어 문제 해결
- IMU 데이터 필터링 → Complementary Filter + EMA 적용
- Bluetooth 송수신 불안정 → 통신 주기 최적화 (100ms), 예외 처리 강화
- 압력 센서값 반전 및 범위 제한 → 0~1700으로 제한, 각도 매핑 정밀화

---

## 🎥 최종 결과 및 시연

- 직관적인 손 움직임과 그리퍼 동작의 실시간 반영
- 무선 제어 환경에서 로봇 팔이 물체를 집고 놓는 등 실제 작업 시연
- 위험하거나 사람이 들어가기 어려운 장소에서 응용 가능한 가능성 입증

<p align="center">
  <img src="./images/glove_demo_1.png">
  <br>
  <em>장갑 착용 및 그리퍼 제어 첫 시연 모습</em>
</p>

<p align="center">
  <img src="./images/glove_demo_2.png">
  <br>
  <em>장갑 및 그리퍼 제어 추가 시연 (각도 테스트)</em>
</p>

<p align="center">
  <img src="./images/final_demo.jpg">
  <br>
  <em>최종 데모: 물체를 집는 로봇 팔 동작</em>
</p>

---

## 💎 Key Value
- **Precision Control**: IMU 기반 미세한 손 움직임 반영
- **Intuitive Operation**: 누구나 직관적으로 제어할 수 있는 사용자 경험
- **Real-time Safety**: 실시간 반응 및 각도 제한으로 안전성 강화
- **Scalability**: 의료, 재활, 산업 등 다양한 분야 확장 가능

---

## 📄 추가 자료
[🔗 발표 자료 보기](https://your-presentation-link)

[💻 코드 및 CAD 파일 보기](https://your-repo-link)

---

> **"정밀한 센서 기반 제어로 사람과 로봇의 경계를 허물다."**

---
