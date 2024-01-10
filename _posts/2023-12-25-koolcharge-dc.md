---
layout: post
title: "[2022.02 ~ 2023.03] 중속 전기차 충전기 임베디드 소프트웨어 및 Android 앱"
date: "2023-12-26 19:00:00"
categories: ["Portfolio", "KoolSign"]
---

# 중속 전기차 충전기 (30/40kWh) SW 및 앱 개발
<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/e883c076-6a6f-4d7c-a3a6-274319e7ae6b" width="100%">
</p>

> 개발 언어 : C, Android-java   
> 개발 도구 : uVision(Keil MDK), Android Studio   
> 개발 환경 : Windows   
> 개발 기간 : 2022.02 ~ 2023.03   
> 개발 인원 : SW-2명, HW-1명   

---
# 담당 역할 (개발 기능)
> **[임베디드 소프트웨어]**   
> <a href="#a1-1" style="text-decoration:none;color:black;">● 통신 모듈 개발</a>   
> > <a href="#a1-1" style="text-decoration:none;color:black;">- CAN 통신 (power 모듈, 절연모듈, PLC 모듈과의 통신 위함)</a>    
> > <a href="#a1-2" style="text-decoration:none;color:black;">- Serial 통신 (android와의 통신 위함)</a>    
>    
> <a href="#a2-1" style="text-decoration:none;color:black;">● 모듈 및 센서 데이터 처리</a>   
> > <a href="#a2-1" style="text-decoration:none;color:black;">- gpio handler 기능 개발</a>    
> > <a href="#a2-2" style="text-decoration:none;color:black;">- isolation 모듈 handler 기능 개발</a>    
> > <a href="#a2-3" style="text-decoration:none;color:black;">- power 모듈 handler 기능 개발</a>    
> > <a href="#a2-4" style="text-decoration:none;color:black;">- PLC 모듈 handler 기능 개발</a>    
>    
> <a href="#a3" style="text-decoration:none;color:black;">● RTX 라이브러리 사용</a>  
>    
> **[안드로이드 개발]**   
> <a href="#a4" style="text-decoration:none;color:black;">● OCPP Open Source 연동 및 서버와의 통신 모듈 개발</a>   
> <a href="#a5" style="text-decoration:none;color:black;">● Serial 통신 기능 개발</a>   
> <a href="#a6" style="text-decoration:none;color:black;">● 서버와의 Http Rest API 통신 기능 개발</a>   
> <a href="https://docs.google.com/presentation/d/1FS-3UUCQwKY5WW1QUwINaVWJJ-Si_H5xQmyy1miCMAE/edit?usp=sharing" style="text-decoration:none;color:black;">● 충전 상태에 따른 초기 UI 및 기능 개발</a>  
>    
    
   
---
<h2 id="a1-1">● CAN 통신</h2>
Power 모듈, Isolation 모듈, PLC 모듈 등과의 통신을 위해 CANn_IRQHandler 함수를 구현하여 데이터를 처리했다.
<script src="https://gist.github.com/cossk3/76b6a96f4f63a1973dde9bd2ee7321f0.js"></script>


ㅤㅤ   
<h2 id="a1-2">● Serial 통신</h2>
Android board와의 통신을 위해 Serial 통신 모듈을 구현했다. RAPI 프로토콜을 정의하여 token과 eoc를 체크해 들어온 RAPI에 따라 데이터를 처리했다.
<script src="https://gist.github.com/cossk3/bf69c29f002c375b2e3022c16d4d1da6.js"></script>


ㅤㅤ   
<h2 id="a2-1">● gpio handler</h2>
<script src="https://gist.github.com/cossk3/d0e1a0d30b2a231c655bbd425446f107.js"></script>


ㅤㅤ   
<h2 id="a2-2">● isolation 모듈 handler</h2>
<script src="https://gist.github.com/cossk3/141d5284d9088a4f968ce6a349b844d9.js"></script>


ㅤㅤ   
<h2 id="a2-3">● power 모듈 handler</h2>
<script src="https://gist.github.com/cossk3/301f5b69fb746e5e966125147c383f64.js"></script>


ㅤㅤ   
<h2 id="a2-4">● PLC 모듈 handler</h2>
<script src="https://gist.github.com/cossk3/ab1e2299534a0b8a7e8b16d44335663b.js"></script>


ㅤㅤ   
<h2 id="a3">● RTX 라이브러리 사용</h2>
<script src="https://gist.github.com/cossk3/6e3a946ad621ac1f5c81ba6d7ead384b.js"></script>


ㅤㅤ   
<h2 id="a4">● OCPP</h2>
<script src="https://gist.github.com/cossk3/b817d2d798ba710595b3606ea847a057.js"></script>


ㅤㅤ   
<h2 id="a5">● Serial 통신</h2>
<script src="https://gist.github.com/cossk3/04e1900b1ef1f1a05e6bac32ff6f5d74.js"></script>


ㅤㅤ   
<h2 id="a6">● Http 통신</h2>
<script src="https://gist.github.com/cossk3/2c19f5fd3e420d235e19608efc33bc45.js"></script>