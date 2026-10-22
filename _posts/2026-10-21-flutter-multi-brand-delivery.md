---
layout: post
title: "[2024.01 ~ 2026.10] Flutter 기반 멀티 브랜드 배달 서비스 앱 개발 및 운영"
date: 2026-10-21
categories: ["Portfolio", "Trism"]
---

# Flutter 기반 멀티 브랜드 배달 서비스 앱 개발 및 운영

## 프로젝트 개요

- **소속:** 트라이즘
- **기간:** 프로젝트별 수행 기간 확인 후 기재
- **플랫폼:** Android, iOS
- **기술:** Flutter, Dart, Riverpod, REST API, WebView, JavaScript Handler, Kakao SDK, Naver Map SDK
- **대상 서비스:** 픽오더, 직배, 지역직배, 온동네 등
- **담당:** Flutter 화면 및 기능 구현, 기존 코드 구조 개선, API·WebView 연동, 브랜드별 앱 빌드·배포 및 유지보수
- **개발 범위:** 기존 멀티 앱 프로젝트 구조를 기반으로 개발. 직배 프로젝트 중반부터 Flutter 앱 개발 단독 담당

여러 브랜드의 배달·포장 서비스를 하나의 Flutter 코드베이스에서 관리하는 프로젝트. 공통 기능을 공유하면서 브랜드·지역별 UI와 서비스 제공 범위를 구분하여 구현.

## 1. 기존 멀티 앱 구조 활용 및 브랜드별 기능 분기

기존 프로젝트의 `apps`, `common`, `core`, `features` 구성을 유지하면서 신규 브랜드 앱 및 기능 개발.

```text
project/
├── apps/
│   ├── brand_a/
│   │   ├── android/
│   │   ├── ios/
│   │   ├── lib/
│   │   └── .env
│   ├── brand_b/
│   └── ...
├── common/
├── core/
└── features/
```

- 브랜드별 독립 Flutter 앱 진입점 및 Android·iOS 프로젝트 유지
- `common`, `core`, `features`의 공통 코드 활용
- 앱별 `.env`에 앱 코드, API 키, Bundle ID, 메인 컬러 등 설정 관리
- 앱 코드를 활용한 브랜드·지역별 기능 노출 제어
- 특정 직배 앱의 마트샵 진입 기능 및 지역별 서비스 기능 조건부 노출

> 전체 멀티 앱 디렉터리 구조는 기존 프로젝트에서 사용하던 구조이며, 신규 설계한 아키텍처가 아님.

## 2. Riverpod 기반 UI 이벤트 처리 구조 개선

### 개선 배경

- 기존 일부 화면에서 UI 처리와 비즈니스 로직 혼재
- 화면 이동, 다이얼로그, Toast 등의 UI 동작을 Controller에서 직접 처리하지 않도록 역할 분리 필요
- 기존 프로젝트 구조를 유지하면서 화면 단위의 점진적 개선 진행

### 구현 내용

- `features/uis` 하위에 `controller`, `events`, `widgets` 영역 구분
- `StateNotifierProvider`로 매장 목록, 로딩, 장바구니 개수 등 화면 상태 관리
- `StateProvider.family`로 화면별 일회성 UI 이벤트 전달
- `sealed class`로 `OpenWeb`, `ShowToast`, `ShowAlert` 등 이벤트 타입 정의
- Controller의 `_emit()`에서 이벤트 발생
- UI의 `ref.listenManual()`에서 이벤트 수신 후 화면 이동 및 알림 처리
- Controller의 `BuildContext` 직접 의존 최소화

### 주요 코드 ① 이벤트 정의 및 Provider 구성

이벤트 타입 정의 및 화면별 이벤트 전달 상태 구성.

<script src="https://gist.github.com/cossk3/cdc27d84d040e4c60564a50280f58f7d.js"></script>

### 주요 코드 ② Controller에서 이벤트 발생

API 응답에 따른 WebView 진입 및 오류 알림 요청을 이벤트로 전달.

<script src="https://gist.github.com/cossk3/f93b7311b3a664e511ee6509763fe69c.js"></script>

### 주요 코드 ③ UI에서 이벤트 수신 및 처리

`ref.listenManual()`을 통한 이벤트 처리 및 WebView 종료 후 데이터 갱신.

<script src="https://gist.github.com/cossk3/9cc68ae0ee824889f1b4efbf98afbb40.js"></script>

### 적용 결과

- 비즈니스 로직과 UI 동작 처리 위치 분리
- 이벤트 유형별 화면 이동 및 알림 처리 일원화
- 화면 상태와 일회성 UI 이벤트의 관리 역할 구분

## 3. 브랜드별 UI 변경 및 신규 기능 구현

### 직배 서비스

- 기획·디자인 자료를 기반으로 메인 UI 약 60~70% 변경
- 기존 공통 코드 활용 및 브랜드별 화면 구성 반영
- 직배 프로젝트 중반부터 Flutter 앱 기능 개발, 배포 및 유지보수 단독 담당

### 온동네 서비스

- 지역 특산물 직거래 서비스의 상품 목록 및 상세 화면을 Flutter UI로 신규 구현
- 관련 REST API 연동 및 화면 데이터 처리
- 메뉴·장바구니·주문·결제 영역은 WebView와 연동
- 기존에 개발한 카드번호 스캔 기능을 온동네 서비스에도 적용

<!-- 스크린샷 추가 예정: 직배 메인 화면, 온동네 특산물 목록 및 상세 화면 -->

## 4. REST API 및 WebView 기반 주문 흐름 연동

- 배달·포장 가맹점 목록 및 상세 화면 등 주요 화면을 Flutter로 구현
- 가맹점 진입 시 회원 정보 및 위치 정보 등을 서버 API에 전달
- API 응답으로 받은 URL을 WebView로 실행
- 메뉴 선택, 장바구니, 주문 및 결제는 웹 서비스에서 처리
- 웹 개발자와 JavaScript Handler 호출 규약 협의
- 결제 완료 이벤트 수신 후 WebView 종료 및 Flutter 이용내역 화면 이동
- WebView 종료 후 장바구니 API 재조회 및 플로팅 장바구니 버튼 노출 상태 갱신

> 결제 기능 자체의 구현이 아닌 Flutter 앱과 웹 주문·결제 화면 간 연동 담당.


### 웹–앱 딥링크 연동 및 로그인 후 목적지 복원

- 웹 가맹점 상세 페이지의 '앱으로 주문하기' 버튼과 Flutter 앱 간 딥링크 연동 구현
- Android 딥링크 및 iOS Universal Links 기반 앱 진입 처리
- 수신한 URI의 스킴 및 파라미터를 파싱하여 요청 유형과 가맹점 코드 식별
- 로그인 상태에 따라 해당 가맹점 WebView로 이동하거나 로그인 화면으로 분기 처리
- 비로그인 상태에서 수신한 가맹점 코드 등 딥링크 정보를 임시 저장
- 로그인 완료 후 저장된 딥링크 정보를 복원하여 해당 가맹점 WebView로 자동 이동
- 저장된 딥링크 요청에 유효시간을 적용하여 만료된 요청 무효 처리
- iOS Universal Links 연동 과정에서 웹 개발자와 협업

**주요 구현 및 해결 사항**

- 웹에서 앱으로 이동하는 과정에 로그인이 필요한 경우에도 최초 요청한 가맹점 정보를 유지하도록 처리
- 로그인 완료 시점에 임시 저장된 딥링크 정보의 유효성을 확인한 후 목적지 이동 처리
- Android와 iOS의 링크 연동 방식 차이를 고려하여 웹–앱 진입 흐름 구현


## 5. 카카오맵 권한 심사 및 멀티앱 적용

### 문제

- 지역직배 앱 추가 과정에서 카카오맵 사용 권한 심사 반복 반려
- 동일 카카오 개발자 계정에 등록된 유사 서비스 및 네이티브 앱 정보 중복 관련 정책 제약 확인

### 대응

- 지도 API 사용 시나리오 및 앱별 서비스 차이 문서화
- 카카오 측 문의를 통해 반려 사유 확인
- 대체 지도 API, 서비스별 독립성 소명, 카카오 멀티앱 기능 적용 방안 비교
- 카카오 멀티앱 기능 심사 신청 및 승인

### 결과

- 지역별 앱의 카카오 SDK 연동 및 신규 배포 진행

## 6. 출시 및 운영

- 여러 브랜드 앱의 Google Play·App Store 빌드 및 배포
- 브랜드별 앱 설정 및 배포 항목 관리
- 심사 반려 및 플랫폼별 연동 이슈 대응
- 기존 운영 앱 유지보수 및 기능 개선

## 담당 업무 요약

- 기존 Flutter 멀티 앱 구조를 활용한 브랜드별 기능 개발
- Riverpod 기반 화면 상태·UI 이벤트 처리 구조 개선
- 직배 메인 UI 변경 및 온동네 특산물 직거래 화면 신규 개발
- REST API 및 WebView 주문 흐름 연동
- 카카오맵 권한 심사 반려 원인 분석 및 멀티앱 승인 대응
- Android·iOS 앱 빌드, 배포 및 운영


## 온동네 앱 주요 화면

<div style="display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 10px;">

  <img src="https://cossk3.github.io/assets/ondongne_splash.jpg" style="width: 100%; height: auto;">

  <img src="https://cossk3.github.io/assets/ondongne_main.jpg" style="width: 100%; height: auto;">

  <img src="https://cossk3.github.io/assets/ondongne_main3.jpg" style="width: 100%; height: auto;">

  <img src="https://cossk3.github.io/assets/ondongne_list.jpg" style="width: 100%; height: auto;">

</div>

<div style="display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 10px;">

  <img src="https://cossk3.github.io/assets/ondongne_detail.jpg" style="width: 100%; height: auto;">

  <img src="https://cossk3.github.io/assets/ondongne_specialtylist.jpg" style="width: 100%; height: auto;">

  <img src="https://cossk3.github.io/assets/ondongne_map.jpg" style="width: 100%; height: auto;">

  <img src="https://cossk3.github.io/assets/ondongne_order.jpg" style="width: 100%; height: auto;">

</div>
