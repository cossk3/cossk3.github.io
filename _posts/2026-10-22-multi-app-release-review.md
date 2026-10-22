---
layout: post
title: "[2024.01 ~ 2026.10] 다수 법인 앱 출시 및 스토어 심사 대응"
date: "2026-10-22 09:00:00"
categories: ["Portfolio", "Trism"]
---

## 개요

- **수행 기간:** 2024.01 ~ 2026.10 (트라이즘 재직 기간 기준, 개별 앱 출시 시점 상이)
- **담당 업무:** 외부 법인 개발자 계정 개설, Android·iOS 앱 빌드 및 배포, 스토어 심사 대응, 정책 관련 앱 로직 수정
- **대상 앱:** 상생러너, 상생온동네, 지역직배 앱, 온스토어, 온라이더, 배송원 앱 4종 등
- **기술 및 플랫폼:** Flutter, Dart, Google Play Console, Apple Developer, App Store Connect

## 담당 범위

- 기존 직배 앱을 기반으로 상생러너·상생온동네의 로고, 앱 이름 및 법인 정보 변경 후 신규 출시
- 외부 법인 명의 Google·Apple 개발자 계정 생성 및 앱 등록·배포 진행
- 온스토어·온라이더 및 배송원 앱 4종 등 인계받은 기존 소스의 빌드·배포 담당
- 심사 반려 사유 분석, 앱 로직·문구·화면 수정, 추가 자료 제출 및 재심사 대응
- 기존 앱 전체 구조 설계 또는 최초 개발과 구분되는 출시·운영 중심 업무

## 주요 심사 대응 사례

### 1. 지역직배 — 위치 권한 거부 후 앱 이용 흐름 개선

**문제 상황**
- App Store Guideline 5.1.1 (Privacy – Data Collection and Storage) 관련 심사 반려
- 앱 실행 시 위치 권한 거부 후 설정 이동을 유도하는 팝업 표시
- 팝업 취소 시 스플래시 화면에서 진행되지 않는 기존 흐름
- 설정 이동 안내 문구를 권유형으로 수정하고 메인 화면 진입을 허용했으나 재반려

**수정 내용**
- 위치 권한 거부 직후 재요청·설정 이동 유도 흐름 제거
- 권한 거부 시 기능 제한 안내 후 메인 화면 진입 허용
- 위치 정보가 필요한 기능에 한해 접근 제한 및 안내 팝업 적용
- 권한 사용 목적과 제한 기능을 설명하는 안내 문구로 수정

**결과**
- 권한 거부 여부와 관계없이 메인 화면에 진입할 수 있도록 사용자 흐름 개선
- 수정 및 재심사를 거쳐 App Store 승인

### 2. 지역직배 — 비로그인 사용자 기능 접근 제한 개선

**문제 상황**
- App Store Guideline 5.1.1 (Account Sign-in) 관련 심사 반려
- 카테고리 탐색 등 계정 기반 기능이 아닌 화면에도 로그인 요구

**수정 내용**
- 카테고리 및 가맹점 상세 정보 등 계정 정보가 필요하지 않은 기능의 비로그인 접근 허용
- 고객 정보가 필요한 가맹점 URL 조회 및 검색 결과 조회 API의 이용 목적을 심사팀에 설명
- 로그인 필요 여부를 기능별로 구분하도록 접근 흐름 수정

**결과**
- 비로그인 사용자 접근 범위 확대 및 재심사 승인

### 3. 상생온동네 — Sign in with Apple 인증 및 회원가입 흐름 수정

**문제 상황**
- App Store Guideline 2.1 (App Completeness): Apple 로그인 기능 오류로 반려
- 해당 앱의 인증 응답에서 `email`, `fullName`이 `null`로 전달되는 상황 확인
- 두 값이 모두 존재해야 회원가입이 진행되는 기존 로직
- 후속 심사에서 Guideline 4 (Design): Apple 로그인 후 이름 재입력 요구 관련 반려

**수정 내용**
- 이메일 정보 누락 시 `identityToken`에서 이메일을 확인하도록 처리
- 이름 정보가 없어도 회원가입을 진행할 수 있도록 조건 변경
- 회원가입 단계의 이름 입력 UI 제거
- 임시 사용자 이름을 부여하고, 배달·특산물 가맹점 접근 시 필요한 경우 이름 재설정 절차 적용

**결과**
- 인증 응답의 선택적 사용자 정보 누락에 대응하는 회원가입 흐름 구성
- 반복 심사 대응 후 App Store 최종 승인

> 임시 이름 및 후속 이름 입력 방식은 해당 서비스의 심사 대응을 위해 적용한 구현 사례이며, Apple 로그인 전반에 대한 일반적 권장 구현으로 제시하지 않음.

### 4. 상생온동네 — 신규 개발자 계정 추가 심사 자료 대응

- 신규 개발자 계정의 심사 이력 부족을 이유로 추가 검증 요청
- 최신 iOS 실기기에서 촬영한 앱 사용 영상 및 서비스 설명 자료 준비·제출
- 앱 목적, 대상 사용자, 주요 기능 접근 경로, 외부 서비스, 국가·지역별 차이 등 요청 항목 대응
- 추가 심사 및 후속 수정 과정을 거쳐 최종 승인

### 5. 기타 스토어 정책 대응

| 대상 앱 | 심사 이슈 | 대응 내용 | 결과 |
| --- | --- | --- | --- |
| 상생러너 | Google Play 메타데이터 정책 위반 | 앱 아이콘의 ‘무료배달’ 문구를 ‘착한가격’으로 변경 | 승인 |
| 온동네 | App Store 권한 목적 설명 부족 | 위치 권한의 주변 가맹점 추천·거리 확인 목적 및 카메라 권한의 카드 정보 자동 입력 목적을 구체화 | 승인 |
| 배송원 앱 4종 | 일부 앱의 조직 전용 서비스 관련 App Store 반려 | 서비스 이용 자격 및 계정 신청 절차 설명, 로그인 화면에 계정 신청 안내 버튼·이메일 팝업 추가 | 4종 모두 승인 |

## 업무 결과 및 경험

- 외부 법인별 Android·iOS 앱 출시 환경 구성 및 신규 앱 배포
- App Store·Google Play 정책에 따른 사용자 흐름 및 앱 설정 수정
- 심사 반려 원인 확인부터 코드 수정, 자료 제출, 재심사 승인까지 대응
- 기존 소스 인계 앱의 출시·운영 업무와 직접 구현한 개선 사항을 구분하여 수행


<h3>앱 출시 및 배포 현황</h3>

<div style="display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 12px;">
  <img src="https://cossk3.github.io/assets/appstore_jigbae.png" style="width: 100%; height: auto; border-radius: 6px;">
  <img src="https://cossk3.github.io/assets/appstore_ondelivery.png" style="width: 100%; height: auto; border-radius: 6px;">

  <img src="https://cossk3.github.io/assets/appstore_delivery4.png" style="width: 100%; height: auto; border-radius: 6px;">
  <img src="https://cossk3.github.io/assets/appstore_ssondongne.png" style="width: 100%; height: auto; border-radius: 6px;">

  <img src="https://cossk3.github.io/assets/googleplay_jigbae.png" style="width: 100%; height: auto; border-radius: 6px;">
  <img src="https://cossk3.github.io/assets/googleplay_ondelivery.png" style="width: 100%; height: auto; border-radius: 6px;">

  <img src="https://cossk3.github.io/assets/googleplay_ssrunner.png" style="width: 100%; height: auto; border-radius: 6px;">
  <img src="https://cossk3.github.io/assets/googleplay_ssondongne.png" style="width: 100%; height: auto; border-radius: 6px;">
</div>

