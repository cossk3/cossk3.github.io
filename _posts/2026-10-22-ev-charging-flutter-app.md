---
layout: post
title: "[2024.09 ~ 2026.09] 전기차 충전 서비스 Flutter 앱 개발"
date: 2026-10-22
categories: ["Portfolio", "Trism"]
---

## 프로젝트 개요

- **프로젝트:** 전기차 충전 서비스 Flutter 앱 개발
- **개발 기간:** 2024.09 ~ 2026.09
- **프로젝트 상태:** 약 80% 개발 진행, 정식 미출시
- **담당 업무:** 전달받은 디자인 시안 기반 Flutter UI 구현, 안전진단 그래프 커스터마이징, 카드 OCR 정보 추출, 충전소 지도 마커 처리, 서버 REST API 연동
- **주요 기술:** Flutter, Dart, Riverpod, Flutter Hooks, `fl_chart`, `camera`, Google ML Kit Text Recognition, `flutter_naver_map`, REST API

> 디자인 시안을 Flutter 화면으로 구현하는 작업을 중심으로, 카드 정보 인식과 충전소 지도 등 기능 개발을 병행한 프로젝트. 정식 출시 전 개발 단계에서 종료.

<!-- 이미지 사용 전 실제 파일명을 확인하고, 아래 이미지 URL의 REPLACE_* 부분을 변경하세요. -->

## 주요 구현 내용

### 1. 등록 차량 기반 메인 화면 UI 구현

- 로그인 사용자의 등록 차량 정보를 표시하는 메인 화면 구현
- `PageView`를 이용한 등록 차량별 프로필 전환 및 차량 추가 페이지 구성
- `Stack`, 그라데이션, 내부 그림자, 페이지 인디케이터 등을 조합해 디자인 시안의 세부 표현 구현
- 차량 등록 개수에 따라 카드 높이 및 페이지 구성을 변경하는 UI 상태 처리

**구현 포인트:** 단순 정보 목록이 아닌, 차량 정보·탐색 버튼·차량 추가 상태를 하나의 카드형 화면에 구성하고 세부 스타일을 맞추는 작업.

<div style="max-width: 420px; margin: 16px auto;">
  <img src="https://cossk3.github.io/assets/evcharge_main.jpg" style="width: 100%; height: auto; border-radius: 8px;">
</div>

**등록 차량 프로필 UI 코드**

<script src="https://gist.github.com/cossk3/6ffc29fb8ed008c8553950b20d82b5c0.js"></script>

### 2. 안전진단 화면 및 커스텀 그래프 구현

- fl_chart 기반 다수의 안전진단 그래프 UI 구현
- 디자인 시안에 맞춘 그래프 형태, 색상, 축 및 데이터 포인트 스타일 커스터마이징
- 차트 라이브러리에서 제공하는 스타일 설정의 한계를 고려한 세부 UI 조정
- 그래프별 디자인 요구사항에 맞춰 차트 설정과 Flutter 위젯 구성을 개별 조정

##### 구현 과정의 어려움

- 차트 라이브러리의 기본 스타일 및 커스터마이징 범위만으로 디자인 시안을 그대로 재현하기 어려운 문제
- 그래프마다 서로 다른 시각적 요구사항이 있어 개별적인 설정과 UI 조정 필요
- 라이브러리에서 지원하는 표현 범위 안에서 디자인 시안과의 차이를 최소화하는 방향으로 구현

**구현 포인트:** 기본 차트 위젯을 그대로 사용하지 않고 그래프 축·간격·선·포인트 표현을 조정해 요구된 UI 구성.

<div style="display: grid; grid-template-columns: repeat(5, minmax(0, 1fr)); gap: 8px; width: 100%; margin: 16px 0;">
  <img src="https://cossk3.github.io/assets/evcharge_batterymain1.jpg" style="width: 100%; height: auto;">
  <img src="https://cossk3.github.io/assets/evcharge_batterymain2.jpg" style="width: 100%; height: auto;">
  <img src="https://cossk3.github.io/assets/evcharge_batterymain3.jpg" style="width: 100%; height: auto;">
  <img src="https://cossk3.github.io/assets/evcharge_batterymain4.jpg" style="width: 100%; height: auto;">
  <img src="https://cossk3.github.io/assets/evcharge_batterymain5.jpg" style="width: 100%; height: auto;">
</div>

**안전진단 그래프 커스터마이징 코드**

<script src="https://gist.github.com/cossk3/b2406c1507418420171288689f0b654b.js"></script>

### 3. 카메라 기반 카드 정보 인식 기능 구현

- `camera` 및 Google ML Kit Text Recognition 기반 카드 스캔 기능 구현
- Android/iOS별 카메라 이미지 포맷을 OCR 입력 이미지로 변환하는 처리
- 화면의 스캔 영역과 OCR 텍스트 블록 영역의 겹침 여부를 확인하여 대상 텍스트 선별
- 카드사마다 카드번호·유효기간의 배치와 줄 순서가 다른 점을 고려해 **고정 위치 대신 정규표현식(Regex) 기반 추출** 적용
- 카드번호와 유효기간을 추출한 뒤 결과 확인 UI에 표시하고 다음 입력 단계로 연결

**구현 포인트:** 카드 표면의 고정 좌표나 텍스트 줄 순서에 의존하지 않고, 지정된 스캔 영역에서 인식한 문자열의 패턴을 기준으로 필요한 정보 구분.

> 공개한 코드 예시는 공백으로 구분된 16자리 카드번호 및 `MM/YY` 형태의 유효기간을 대상으로 하는 발췌본. 모든 카드 형식에 대한 지원을 의미하지 않음.

<div style="max-width: 420px; margin: 16px auto;">
  <img src="https://cossk3.github.io/assets/evcharge_cardscan.jpeg" style="width: 100%; height: auto; border-radius: 8px;">
</div>


**OCR 텍스트 선별 및 정규표현식 추출 코드**

<script src="https://gist.github.com/cossk3/08bf0db228519d2e33d37826066145fa.js"></script>

### 4. 충전소 지도 및 커스텀 마커 구현

- `flutter_naver_map` 기반 충전소 지도 화면 구현
- 충전 가능 여부, 마커 선택 상태, 즐겨찾기 여부 및 충전 가능한 충전기 수에 따라 달라지는 마커 UI 구성
- `NOverlayImage.fromWidget`을 이용한 Flutter 위젯 기반 마커 이미지 생성
- 마커 상태 조합을 캐시 키로 사용해 생성된 마커 이미지 재사용
- 선택된 마커의 이미지 디코딩 완료를 기다리는 처리 적용
- 좌표 버킷 기반 인접 충전소 그룹화 및 충전 가능 여부·충전 속도·커넥터 유형 등에 따른 필터링 구현
- Reverse Geocoding을 통한 지역 코드 조회 및 충전소 목록·상세 REST API 연동

**구현 포인트:** 단순 기본 마커 표시가 아닌, 충전소의 상태와 수량을 시각적으로 구분하는 커스텀 마커 생성 및 중복 이미지 생성을 줄이기 위한 캐싱 처리.

<div style="display: grid; grid-template-columns: repeat(5, minmax(0, 1fr)); gap: 8px; width: 100%; margin: 16px 0;">
  <img src="https://cossk3.github.io/assets/evcharge_map.jpg" style="width: 100%; height: auto;">
  <img src="https://cossk3.github.io/assets/evcharge_map2.jpg" style="width: 100%; height: auto;">
  <img src="https://cossk3.github.io/assets/evcharge_map3.jpg" style="width: 100%; height: auto;">
</div>

**충전소 마커 생성 및 이미지 캐싱 코드**

<script src="https://gist.github.com/cossk3/7c0ab89592ccf5097df719937dd4799c.js"></script>

## 정리

- 복잡한 디자인 시안 기반 Flutter UI 및 차량 프로필 화면 구현 경험
- `fl_chart`를 활용한 다수 그래프의 세부 표현 커스터마이징 경험
- OCR 텍스트에서 정규표현식으로 카드번호·유효기간을 구분하는 기능 구현 경험
- 충전소 상태별 커스텀 지도 마커 생성, 이미지 캐싱, 그룹화 및 API 연동 경험
- **정식 미출시 프로젝트**로, 상용 서비스 출시 성과와 구분하여 기재
