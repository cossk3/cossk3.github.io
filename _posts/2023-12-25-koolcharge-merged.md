---
layout: post
title: "[2022.02 ~ 2023.03] 전기차 충전 서비스 개발 (충전기 임베디드 SW · HMI · Flutter 앱)"
date: "2023-12-26 19:00:00"
categories: ["Portfolio", "KoolSign"]
---

# 전기차 충전 서비스 개발

충전기 내부 제어보드(MCU) 펌웨어와 HMI Android 앱, 사용자용 Flutter 앱을 개발한 프로젝트입니다. 충전기 내부의 하드웨어 제어부터 사용자 앱의 충전소 조회·결제·실시간 충전 상태 확인까지 서로 다른 영역을 담당했습니다.

> **개발 기간** : 2022.02 ~ 2023.03  
> **기술 스택** : C, Java(Android), Flutter(Dart), Swift, CAN, UART/Serial, Modbus, OCPP, NATS, HTTP REST API  
> **개발 도구** : Keil µVision, Android Studio, Xcode  
> **참여 인원** : 충전기 제어 SW 2명 / HW 1명, HMI Android 앱 및 사용자 Flutter 앱 단독 개발  
> **진행 결과** : 급속 충전기 개발 및 시험 설치 단계 참여 / 사용자 Flutter 앱 Google Play·App Store 출시

<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/e883c076-6a6f-4d7c-a3a6-274319e7ae6b" width="100%" alt="충전기 프로젝트 이미지">
</p>

<p width="100%">
  <img src="https://github.com/cossk3/allseoul/assets/44231144/3befe1fe-4007-4a43-8b1b-2bb13026cad8" width="100%" alt="사용자용 Flutter 앱 이미지">
</p>

- [Google Play](https://play.google.com/store/apps/details?id=net.koolsign.charge_app&hl=ko-KR)
- [App Store](https://apps.apple.com/kr/app/koolcharge-%EC%BF%A8%EC%B0%A8%EC%A7%80-%EB%82%98%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-ai%EC%B6%A9%EC%A0%84/id1663150470)

---

## 담당 범위

### 충전기 임베디드 SW (C)

- 팀장이 구축한 MCU 프로젝트 기본 구조 및 포트별 통신 기반을 바탕으로 충전기 제어 기능 개발
- PLC·Power·절연 모듈의 통신 프로토콜 문서 분석, 메시지 처리 및 실제 하드웨어 연동 테스트
- 차량과 충전기 간 통신 상태에 따라 각 모듈을 제어하는 충전 시퀀스 로직 설계 및 구현
- CAN 기반 모듈 통신, GPIO 제어, UART/Serial 기반 HMI 연동 및 RTX 태스크 활용

### 충전기 HMI Android 앱 (Java) — 단독 개발

- 충전기 내부 제어보드와 UART/Serial 통신 및 RAPI 명령·응답 처리
- 충전 상태별 초기 UI 및 기능 개발
- OCPP 오픈소스 연동 및 서버 통신, HTTP REST API 처리

### 사용자용 Flutter 앱 — 단독 개발

- 충전소 지도·주소 조회, 길 안내, 결제, 푸시 알림 기능 개발
- NATS 기반 충전 상태 데이터 실시간 수신 및 연결 상태 확인
- 로그인 상태 관리 및 Android NFC 네이티브 연동
- Google Play 및 App Store 출시

---

## 1. 충전기 임베디드 SW

### 1-1. PLC 모뎀 기반 충전 시퀀스 제어

PLC 모뎀은 전기차와 충전기 간 충전 관련 데이터를 교환하는 모듈입니다. 모듈의 통신 프로토콜을 분석하고, 충전 단계별로 차량의 요청 값과 장비 상태를 확인하면서 필요한 모듈을 동작시키는 제어 로직을 개발했습니다.

- **차량 연결 및 통신 준비** : 커넥터 연결 상태 확인, SLAC 통신 대기 및 세션 진행 상태 처리
- **인증 및 충전 조건 교환** : 인증 단계 및 충전 파라미터 교환에 따른 상태 처리
- **CableCheck** : 파워 모듈 출력·정지, 릴레이 및 절연 감시 등 충전 전 안전 확인 단계에 필요한 제어
- **PreCharge** : 차량이 요청한 목표 전압에 맞춰 파워 모듈 출력을 제어하고 측정값 확인
- **Current Demand** : 차량의 요청 전압·전류에 따라 파워 모듈 출력 제어
- **충전 종료** : 충전 종료 상태에 맞춰 모듈 동작 및 세션 종료 처리

**주요 구현 및 해결 사항**

- 단순한 CAN 데이터 수신·파싱을 넘어, 충전 시퀀스의 상태와 타이밍에 맞춰 모듈별 명령을 수행하도록 로직 구성
- 프로토콜 문서에 정의된 상태·명령·응답을 실제 하드웨어에서 반복 테스트하며 제어 흐름 검증
- PLC 모뎀, 파워 모듈, 절연 모듈의 상태를 함께 고려해야 하는 충전 과정 구현

**참고** : [충전 시퀀스 및 시스템 흐름 정리 자료](https://docs.google.com/presentation/d/1UUmemIBY4EYIbs9rpcYbMk17CcuOO-gqQz7jT60LAlk/edit?usp=sharing)

### 1-2. CAN 통신 및 하드웨어 모듈 연동

- **PLC 모뎀** : 차량–충전기 통신 단계 및 오류·상태 데이터 처리
- **Power 모듈** : 전원 ON/OFF, 출력 전압·전류 설정 및 출력 상태 확인
- **절연 모듈** : 절연 저항·상태·오류 등 수신 데이터 처리
- **GPIO** : FAN 등 충전기 내부 제어 신호 처리
- **DC Meter** : Modbus 통신으로 전압·전류·전력·전력량 계측 데이터 조회

CAN 메시지 처리 및 모듈별 데이터 해석 로직을 개발하고, 각 모듈이 충전 시퀀스의 적절한 시점에 동작하도록 연결했습니다.

### 1-3. MCU–HMI 간 UART/Serial 통신

- MCU 제어보드와 Android HMI 보드 간 UART 기반 Serial 통신
- RAPI 형식의 명령·응답 처리, 시작·종료 문자 및 체크섬 처리
- 충전 시작·종료 관련 명령과 연결 상태, 전압·전류, 전력량, SoC, 온도 등 상태 데이터 교환

### 1-4. 2채널 충전 구조에서의 기술적 과제

- 충전 커넥터가 2개로 확장될 때 PLC 모뎀과 Power 모듈이 추가되는 구조 검토
- 두 차량이 동시에 충전하는 경우 Power 모듈을 나누어 사용하는 제어 조건 고려
- 단일 채널과 달리 채널별 충전 상태와 모듈 할당을 함께 관리해야 하는 복잡성 경험

> **개발 범위** : 2채널 동시 충전 및 Power 모듈 배분 관련 작업은 개발 진행 중 퇴사하여 최종 완성·상용화 실적으로 기재하지 않음.

### 1-5. RTX 기반 태스크 처리

- MCU 환경에서 RTX 라이브러리를 활용한 제어·통신 작업 처리

---

## 2. 충전기 HMI Android 앱

충전기 내부 디스플레이에서 동작하는 Android 앱을 단독 개발했습니다.

- MCU와 Serial 통신으로 충전기 제어 명령 전달 및 상태 데이터 수신
- RAPI 프로토콜을 이용한 메시지 구성·전송·파싱
- 충전 상태에 따른 초기 UI 및 기능 구현
- OCPP 오픈소스 연동을 통한 서버 통신 기능 개발
- HTTP REST API 기반 서버 통신 기능 구현

**주요 구현 및 해결 사항**

- MCU와 HMI 사이에서 충전 상태와 제어 명령이 일치하도록 Serial 메시지 처리
- 수신 데이터를 명령 종류에 따라 분기하고 충전기 상태를 앱에서 활용하도록 연동

**참고** : [기존 HMI UI 및 기능 자료](https://docs.google.com/presentation/d/1FS-3UUCQwKY5WW1QUwINaVWJJ-Si_H5xQmyy1miCMAE/edit?usp=sharing)

---

## 3. 사용자용 Flutter 앱 (쿨차지)

충전소 검색부터 결제와 충전 상태 확인까지 사용자에게 필요한 기능을 구현한 Flutter 앱입니다. Android·iOS 앱을 단독 개발하고 양대 스토어에 출시했습니다.

### 3-1. 충전소 지도 및 길 안내

- Naver Map API 기반 충전소 위치 표시
- Google Map API 연동 기능 구현
- Naver Reverse Geocoding을 이용한 현재 위치 주소 조회
- Kakao Navi 연동으로 선택한 목적지까지 길 안내

### 3-2. 결제 및 NFC

- Smartro Pay API와 WebView를 이용한 결제 기능 연동
- Flutter `MethodChannel`을 통한 Android 네이티브 NFC 기능 호출
- NFC 기능은 Android에 한정하여 구현

### 3-3. NATS 기반 실시간 충전 상태 수신

- `dart_nats` 라이브러리를 활용한 서버 통신 모듈 개발
- 충전 진행 상태 데이터를 실시간으로 수신하기 위한 NATS 통신 적용
- 약 3초 간격의 연결 상태 확인 로직 구현

**주요 구현 및 해결 사항**

- 기존에 사용해보지 않은 NATS 통신을 학습하고 Flutter 앱에 직접 적용
- 실시간 충전 상태 확인을 위한 서버 연결 상태 점검 로직 구성

### 3-4. 푸시 알림 및 상태 관리

- Firebase Cloud Messaging(FCM) 포그라운드·백그라운드 메시지 처리
- 앱 종료 상태의 백그라운드 핸들러에서 필요한 데이터 보관을 위해 `shared_preferences` 사용
- `provider` 기반 로그인·자동 로그인·로그아웃 상태와 관련 UI 처리
- HTTP REST API 통신 및 오류 처리

---

## 4. 개발 결과 및 적용 범위

- 급속 충전기 MCU 제어 로직 및 HMI Android 앱 개발 참여, 실제 하드웨어 연동 테스트 수행
- 급속 충전기 시험 설치 단계 참여 (재직 기간 내 정식 상용 설치 실적은 없음)
- 사용자용 Flutter 앱 Google Play·App Store 출시
- 임베디드 제어, 장비–HMI 통신, 사용자 모바일 앱까지 서로 다른 소프트웨어 계층을 개발한 경험

---

## 5. 주요 구현 코드 (Gist)

아래 코드는 당시 프로젝트에서 정리한 구현 예제입니다.

### 임베디드 SW — CAN 통신


<script src="https://gist.github.com/cossk3/76b6a96f4f63a1973dde9bd2ee7321f0.js"></script>

### 임베디드 SW — UART/Serial 통신


<script src="https://gist.github.com/cossk3/bf69c29f002c375b2e3022c16d4d1da6.js"></script>

### 임베디드 SW — GPIO 제어


<script src="https://gist.github.com/cossk3/d0e1a0d30b2a231c655bbd425446f107.js"></script>

### 임베디드 SW — 절연 모듈


<script src="https://gist.github.com/cossk3/141d5284d9088a4f968ce6a349b844d9.js"></script>

### 임베디드 SW — Power 모듈


<script src="https://gist.github.com/cossk3/301f5b69fb746e5e966125147c383f64.js"></script>

### 임베디드 SW — PLC 모듈


<script src="https://gist.github.com/cossk3/ab1e2299534a0b8a7e8b16d44335663b.js"></script>

### 임베디드 SW — RTX


<script src="https://gist.github.com/cossk3/6e3a946ad621ac1f5c81ba6d7ead384b.js"></script>

### HMI Android — OCPP


<script src="https://gist.github.com/cossk3/b817d2d798ba710595b3606ea847a057.js"></script>

### HMI Android — Serial 통신


<script src="https://gist.github.com/cossk3/04e1900b1ef1f1a05e6bac32ff6f5d74.js"></script>

### HMI Android — HTTP 통신


<script src="https://gist.github.com/cossk3/2c19f5fd3e420d235e19608efc33bc45.js"></script>

### Flutter — FCM


<script src="https://gist.github.com/cossk3/7466831e519ae53f3672afc814f8dd15.js"></script>

### Flutter — Naver Map


<script src="https://gist.github.com/cossk3/9e11b0f036d062f8f7c0e1518f559f9f.js"></script>

### Flutter — Google Map


<script src="https://gist.github.com/cossk3/75452729892ff253de961e60a437e3e9.js"></script>

### Flutter — Reverse Geocoding


<script src="https://gist.github.com/cossk3/a8abf808b5342e4c83ca854b4be22d30.js"></script>

### Flutter — Kakao Navi


<script src="https://gist.github.com/cossk3/d364b77c531ec56b1599fece92247a65.js"></script>

### Flutter — Smartro Pay


<script src="https://gist.github.com/cossk3/0db1e2bb640e57fc9e6e8be6585dba28.js"></script>

### Flutter — HTTP REST API


<script src="https://gist.github.com/cossk3/a82c874eaa7da16bfdddbf887c59f0f3.js"></script>

### Flutter — NATS


<script src="https://gist.github.com/cossk3/9906d43cf4199c88201f8fcd23c7a64c.js"></script>

### Flutter — Provider


<script src="https://gist.github.com/cossk3/cb9f18dc6a16c179d34a7390a71f8afa.js"></script>

### Flutter — Android NFC


<script src="https://gist.github.com/cossk3/6602fdd88d289d7bf7c320b8c54b2109.js"></script>
