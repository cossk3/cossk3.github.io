---
layout: post
title: "[2021.03 ~ 2021.10] SI 프로젝트 - 홈 트레이닝 영상 재생 앱"
date: "2023-12-26 15:00:00"
categories: ["Portfolio", "KoolSign"]
---

# SI 프로젝트 - 홈 트레이닝 영상 재생 앱 개발
<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/7c92015f-b5c7-4fe8-892b-18569461bc75" width="70%">
</p>

> 개발 언어 : Swift   
> 개발 도구 : Xcode
> 개발 환경 : MacOS   
> 개발 기간 : 2021.03 ~ 2021.10  
> 개발 인원 : 1명   

---
# 담당 역할 (개발 기능)
> <a href="#a1" style="text-decoration:none;color:black;">● 셋탑과의 통신하기 위한 Wi-Fi 관련 기능 개발</a>   
> > <a href="#a1" style="text-decoration:none;color:black;">- wifi connect, get list, connection monitoring 기능 개발</a>   
>    
> <a href="#a2" style="text-decoration:none;color:black;">● 셋탑(홈트레이닝 구동 시스템)과의 TCP/UDP Socket 통신 모듈 개발</a>   
> <a href="#a3" style="text-decoration:none;color:black;">● 서버와 Http 통신 모듈 개발</a>   
> <a href="#a4" style="text-decoration:none;color:black;">● 서버와 Stomp WebSocket 통신 모듈 개발</a>   
> <a href="#a5" style="text-decoration:none;color:black;">● 영상 컨트롤 위한 AVKit 라이브러리 사용 및 기능 개발</a>   
> <a href="#a6" style="text-decoration:none;color:black;">● Firebase Cloud Messaging 기능 개발</a>   
> 

---
# 성과 / 기여도
+ 타 업체와의 협업, 커뮤니케이션 능력을 향상시킬 수 있는 계기가 됨
   
    
---
<h2 id="a1">● Wi-Fi 통신</h2>
셋탑과의 Wi-Fi 통신을 위해 Wifi List를 스캔하고, 연결한다. 중간에 끊기는 경우를 대비하여 wifi 연결 체크 모니터링 로직도 추가했다.
<script src="https://gist.github.com/cossk3/87177fcef2192b1ce2bdfec439c50f24.js"></script>


ㅤㅤ   
<h2 id="a2">● Socket 통신</h2>
셋탑과의 첫 통신 시, 셋탑 ip 등에 대한 정보가 없기 때문에 UDP 통신을 이용하여 데이터를 주고 받은 후 TCP 통신으로 전환하기 위해 UDP/TCP 둘 다 사용했다.
<script src="https://gist.github.com/cossk3/7db6557927612ad5728204a75a579986.js"></script>


ㅤㅤ   
<h2 id="a3">● Http 통신</h2>

`Alamofire` Http 네트워킹 라이브러리를 사용하여 Http 통신 모듈을 구현했다. request에 실패한 경우, 네트워크 연결 여부에 따라 retry 로직을 구현했다.
<script src="https://gist.github.com/cossk3/7a0248fdd18c111e8ea8a826bc6551f5.js"></script>


ㅤㅤ   
<h2 id="a4">● Stomp WebSocket 통신</h2>
시나리오 중 친구를 초대해 함께 영상에 참여할 수 있는 기능을 위해 실시간 양방향 통신이 필요해 웹소켓을 사용했다.
<script src="https://gist.github.com/cossk3/6fbc6a2841eef33db89cc4b6c7673e91.js"></script>


ㅤㅤ   
<h2 id="a5">● AVKit 기능</h2>
영상 play, pause, backward, forward, buffering 등 `AVKit` 라이브러리를 사용하여 기능을 구현했다.
<script src="https://gist.github.com/cossk3/7dcd5d4e0d3d7dc22cfbdc56295382c6.js"></script>


ㅤㅤ   
<h2 id="a6">● Firebase Cloud Messaging</h2>
iOS에서의 FCM 기능 추가는 생각보다 간단했다. `AppDelegate.swift`에 Firebase에 대한 초기 설정 코드를 넣어주고, `MessagingDelegate`의 `messaging(...)` 함수를 오버라이딩하여 메세지 처리를 해주었다.
<script src="https://gist.github.com/cossk3/f2116132af41bc5fd05c09fbd9e7c61b.js"></script>