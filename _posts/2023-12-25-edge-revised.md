---
layout: post
title: "[2021.09 ~ 2022.06] RTSP IP 카메라 Android 앱 개발"
date: "2023-12-26 16:00:00"
categories: ["Portfolio", "KoolSign"]
---

# RTSP IP 카메라 Android 앱 개발

ONVIF를 이용한 IP 카메라 제어와 Live555 기반 스트리밍 기능을 연동한 Android 앱 개발 프로젝트입니다. 카메라 회전·줌 제어, 오디오 수신, 딥러닝 모델 기반 객체 추적 및 서버 통신 기능을 개발했습니다.

> **개발 기간** : 2021.09 ~ 2022.06  
> **개발 언어** : Java(Android), C++  
> **개발 도구** : Android Studio, ONVIF Device Manager, VLC  
> **개발 환경** : Windows, Android  
> **개발 인원** : SW 2명, HW 1명

---

## 담당 업무

- ONVIF 오픈소스를 활용한 IP 카메라 PTZ(Pan·Tilt·Zoom) 제어 기능 연동 및 개발
- Live555 오픈소스 분석 및 기존에 지원하지 않던 Opus 오디오 스트리밍 기능 추가
- IP 카메라에서 송출하는 오디오를 Android 앱에서 수신·재생할 수 있도록 연동
- TensorFlow Lite 학습 모델 적용 및 얼굴·헬멧·신체·마스크 등 객체 추적을 위한 데이터 처리 기능 개발
- HTTP 기반 서버 통신 모듈 개발

---

## 1. ONVIF 기반 IP 카메라 제어

- 회전 및 줌 기능을 지원하는 IP 카메라와 Android 보드 연동
- GitHub에 공개된 ONVIF 오픈소스를 활용한 카메라 제어 기능 구현
- ONVIF 기반 PTZ 제어 기능을 앱에서 사용할 수 있도록 연동

**참고 자료**
- [활용한 ONVIF 오픈소스](https://github.com/As772309423/onvif-server-with-rtsp/tree/a1545162ff9e12efcf303cf403cdc4e4ab90ecc8)
- [ONVIF 관련 개발 기록](https://cording-cossk3.tistory.com/164)

## 2. Live555 기반 Opus 오디오 스트리밍 기능 확장

IP 카메라의 오디오를 Android 앱에서 수신하기 위해 Live555 오픈소스를 활용했습니다. 프로젝트에서 필요한 **Opus 오디오 포맷이 기존 사용 코드에 지원되지 않아**, 관련 자료와 소스 코드를 분석한 뒤 Opus 스트리밍 지원 기능을 직접 추가했습니다.

- [Live555](http://www.live555.com/liveMedia/#license) 오픈소스를 활용한 스트리밍 기능 연동
- 기존 오픈소스에 없던 Opus 오디오 스트리밍 지원 기능 추가 개발
- 관련 기술 자료 조사 및 Live555 소스 코드 분석을 통한 기능 확장
- IP 카메라에서 송출하는 Opus 오디오를 Android 앱에서 수신·재생하도록 연동

**주요 구현 및 해결 사항**

- 오픈소스가 기본 제공하지 않는 오디오 포맷을 지원해야 하는 요구사항 대응
- 기존 스트리밍 코드의 동작 구조를 파악하고 필요한 기능을 추가하는 방식으로 구현
- 카메라–Android 보드 연동 환경에서 오디오 수신 기능 구현

> **구현 범위** : 카메라에서 Android 앱으로 전달되는 **단방향 오디오 수신·재생** 기능. 앱 마이크의 오디오를 카메라로 전송하는 양방향 통신 기능은 포함하지 않음.

## 3. TensorFlow Lite 기반 객체 추적 기능

- 학습된 TensorFlow Lite(`.tflite`) 모델을 Android 앱에 적용
- 얼굴·헬멧·신체·마스크 등 모델별 추론 결과 데이터 처리
- 모델 결과를 활용한 객체 추적 기능 개발

<script src="https://gist.github.com/cossk3/38691286d0afb030ffd02b8281fcf759.js"></script>

## 4. HTTP 서버 통신

- Android 앱과 서버 간 데이터 송수신을 위한 HTTP 통신 모듈 개발

<script src="https://gist.github.com/cossk3/c90c1773a509045e61c766c214592334.js"></script>

---

## 주요 기술 경험

- 기존 ONVIF 오픈소스를 활용한 IP 카메라 PTZ 제어 기능 연동
- Live555 오픈소스 소스 코드 분석 및 미지원 Opus 오디오 스트리밍 기능 확장
- Android 앱에서 IP 카메라 오디오 수신·재생 기능 구현
- TensorFlow Lite 기반 객체 추적 및 HTTP 통신 기능 개발
