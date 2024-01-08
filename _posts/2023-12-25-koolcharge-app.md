---
layout: post
title: "[2022.02 ~ 2023.03] 전기차 충전기 Flutter 앱"
date: "2023-12-26 18:00:00"
categories: ["Portfolio", "KoolSign"] 
---

# 전기차 충전기 앱 개발
<p width="100%">
  <img src="https://github.com/cossk3/allseoul/assets/44231144/3befe1fe-4007-4a43-8b1b-2bb13026cad8" width="100%">
</p>

> 개발 언어 : Flutter, java, swift   
> 개발 도구 : Android Studio, XCode
> 개발 환경 : Windows   
> 개발 기간 : 2022.02 ~ 2023.03   
> 개발 인원 : 1명   
> > <a href="https://play.google.com/store/apps/details?id=net.koolsign.charge_app&hl=ko-KR" style="text-decoration:none;color:blue;">Google Play 링크</a>   
> > <a href="https://apps.apple.com/kr/app/koolcharge-%EC%BF%A8%EC%B0%A8%EC%A7%80-%EB%82%98%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-ai%EC%B6%A9%EC%A0%84/id1663150470" style="text-decoration:none;color:blue;">App Store 링크</a>   

---
# 담당 역할 (개발 기능)
> **[외부 API]**   
> <a href="#a1" style="text-decoration:none;color:black;">● Firebase Cloud Messaging 기능 개발</a>   
> <a href="#a2-1" style="text-decoration:none;color:black;">● Map API : 전기차 충전소 표시 기능 개발</a>    
> > <a href="#a2-1" style="text-decoration:none;color:black;">- Naver Map API</a>    
> > <a href="#a2-2" style="text-decoration:none;color:black;">- Google Map API</a>    
> > <a href="#a2-3" style="text-decoration:none;color:black;">- Naver Reverse-Geocode API</a>    
> > > <a href="#a2-3" href="#a2-1" style="text-decoration:none;color:black;">- 해당 API 이용하여 현 위치 주소 얻어오는 모듈 개발</a>    
>    
> <a href="#a3" style="text-decoration:none;color:black;">● Kakao Navi API : 네비 연결 기능 개발</a>   
> <a href="#a4" style="text-decoration:none;color:black;">● Smartro Pay : 결제 기능 연동 및 개발</a>   
>    
> **[서버 통신]**   
> <a href="#a5" style="text-decoration:none;color:black;">● Http Rest API 통신 모듈 개발</a>   
> <a href="#a6" href="#a3" style="text-decoration:none;color:black;">● NATs 통신 모듈 개발</a>   
>    
> **[라이브러리]**   
> <a href="#a7" style="text-decoration:none;color:black;">● provider 라이브러리 사용하여 상태 변경 데이터 처리</a>   
> <a href="#a8-1" style="text-decoration:none;color:black;">● NFC 라이브러리 사용</a>   
> > <a href="#a8-1" style="text-decoration:none;color:black;">- MethodChannel 이용한 Navite Code Handler 기능 개발</a>   
>    
   
    
---
<h2 id="a1">● Firebase Cloud Messaging</h2>
서버에서 전송된 메세지를 통해 앱 실행 시 백그라운드, 포그라운드 별로 데이터를 처리했다.    

앱 종료 상태에서 메세지 받을 시 실행되는 백그라운드 핸들러 함수에서 데이터 처리 시, 데이터를 저장하고 전달하기 위해 `shared_preferences` 라이브러리를 사용했다.
<script src="https://gist.github.com/cossk3/7466831e519ae53f3672afc814f8dd15.js"></script>


ㅤㅤ   
<h2 id="a2-1">● Naver Map API</h2>

`Naver Map API`를 선택하게 된 이유는 일부 기능 제외하고는 대부분의 기능이 무료로 제공되었기 때문이다. `naver_map_plugin` 라이브러리를 사용하여 네이티브 코드로 직접 구현할 필요 없이 쉽고 간편하게 기능을 구현했다.
<script src="https://gist.github.com/cossk3/9e11b0f036d062f8f7c0e1518f559f9f.js"></script>


ㅤㅤ   
<h2 id="a2-2">● Google Map API</h2>
naver 지도는 국내 위주의 서비스가 잘 되어있고, 추후 국외 서비스를 고려했을 때 사용하기 위해 구현해보았다.
<script src="https://gist.github.com/cossk3/75452729892ff253de961e60a437e3e9.js"></script>


ㅤㅤ   
<h2 id="a2-3">● Naver Reverse-Geocode API</h2>
Naver Map API 중 Reverse GeoCode API를 사용하여 현 위치 주소를 얻어왔다.
<script src="https://gist.github.com/cossk3/a8abf808b5342e4c83ca854b4be22d30.js"></script>


ㅤㅤ   
<h2 id="a3">● Kakao Navi API : 네비 연결 기능 개발</h2>
KaKao Navi API를 사용해 목적지 위치 좌표를 보내 카카오내비 앱과 연동해 길 안내 서비스를 구현했다.
<script src="https://gist.github.com/cossk3/d364b77c531ec56b1599fece92247a65.js"></script>


ㅤㅤ   
<h2 id="a4">● Smartro Pay : 결제 기능 연동 및 개발</h2>
스마트로페이 API와 WebView를 사용하여 결제 기능을 구현했다.
<script src="https://gist.github.com/cossk3/0db1e2bb640e57fc9e6e8be6585dba28.js"></script>


ㅤㅤ   
<h2 id="a5">● Http Rest API 통신 모듈 개발</h2>
서버와의 통신을 위해 Reat API를 지정하여 Http 통신 모듈을 구현했다. error handler가 다소 까다로웠지만, 최대한 모든 경우의 에러를 처리하려고 했다.
<script src="https://gist.github.com/cossk3/a82c874eaa7da16bfdddbf887c59f0f3.js"></script>


ㅤㅤ   
<h2 id="a6">● NATs 통신 모듈 개발</h2>
`dart_nats` 라이브러리를 사용하여 NATs 통신 모듈을 구현했다. 3초마다 서버 연결 체크 로직을 넣어 끊김 없이 통신이 되게끔 하였다.
<script src="https://gist.github.com/cossk3/9906d43cf4199c88201f8fcd23c7a64c.js"></script>


ㅤㅤ   
<h2 id="a7">● provider</h2>
login 관련 변수들을 provider로 처리하여 자동 로그인 또는 로그인, 로그아웃 등의 상태 변화 UI를 처리했다.
<script src="https://gist.github.com/cossk3/cb9f18dc6a16c179d34a7390a71f8afa.js"></script>


ㅤㅤ   
<h2 id="a8-1">● NFC</h2>
Android NFC 기능을 외부 라이브러리를 사용하여 구현했다. iOS에서는 NFC 기능을 구현하지 않아 Android에서만 구현되어 있다.
<script src="https://gist.github.com/cossk3/6602fdd88d289d7bf7c320b8c54b2109.js"></script>