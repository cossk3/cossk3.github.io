---
layout: post
title: "[2020.02 ~ 2023.03] 열화상 카메라"
date: "2023-12-26 14:00:00"
categories: ["Portfolio", "KoolSign"]
---

# AI 열화상 카메라 제품 개발
<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/8658181c-c5c9-481e-8f7d-e8568d22fd7e" width="100%">
</p>

> 개발 언어 : Android-java, C++, SQLite   
> 개발 도구 : Android Studio   
> 개발 환경 : Windows, Android   
> 개발 기간 : 2020.02 ~ 2023.03   
> 개발 인원 : SW-2명, HW-2명   

---
# 개발 기능
> **[카메라]**   
> <a href="#a1" style="text-decoration:none;color:black;">● 열화상 카메라 연동 및 데이터 처리</a>   
> > <a style="text-decoration:none;color:black;">- 온도 보정 로직 개발</a>   
> > <a style="text-decoration:none;color:black;">- RGB 카메라와 열화상 카메라의 화각 조정 기능 개발</a>   
>    
> <a href="#a2" style="text-decoration:none;color:black;">● 거리 카메라 연동 및 데이터 처리</a>     
> > <a style="text-decoration:none;color:black;">- 거리에 따른 열화상 카메라 온도 보정 로직 개발</a>   
> > <a style="text-decoration:none;color:black;">- RGB 카메라와 거리 카메라의 화각 조정 기능 개발</a>   
> 
> <a href="#a3" style="text-decoration:none;color:black;">● 딥러닝 학습 모델 적용 및 데이터 처리</a>   
> > <a style="text-decoration:none;color:black;">- 얼굴, 헬멧, 바디, 마스크 등 여러 학습 모델 tflite파일 적용 및 트래킹 기능 개발</a>   
> > <a style="text-decoration:none;color:black;">- 얼굴 랜드마크 데이터를 이용한 온도 보정 로직 개발</a>   
>    
> <a href="#a4" style="text-decoration:none;color:black;">● QR Reader 모듈 연동 및 데이터 처리</a>   
> > <a style="text-decoration:none;color:black;">- QR 코드 인식을 통한 출입문 통제 기능 개발</a>   
>    
> <a href="#a5" style="text-decoration:none;color:black;">● 혈압 측정계 연동 및 데이터 처리</a>   
>    
> **[서버 통신]**   
> <a href="#a6" style="text-decoration:none;color:black;">● Mqtt 통신 모듈 개발</a>   
> <a href="#a7" sstyle="text-decoration:none;color:black;">● Http 통신 모듈 개발</a>   
> 
   
    
---
<h2 id="a1">● 열화상 카메라</h2>
초창기엔 해상도가 작은 Flir 열화상 카메라를, 그 후엔 해상도가 좀 더 큰 Seek 열화상 카메라를 사용하여 그에 따른 온도 보정과 데이터를 처리했다.
<script src="https://gist.github.com/cossk3/24874e27e2742cb5b00b010e7b7734dc.js"></script>


ㅤㅤ   
<h2 id="a2">● 거리 카메라</h2>
거리에 따른 온도 보정을 위해 거리 카메라를 연동했고, 그에 따른 데이터를 처리했다.
<script src="https://gist.github.com/cossk3/9cf63354c8aadb629fcb6e63a5822593.js"></script>


ㅤㅤ   
<h2 id="a3">● 딥러닝 학습 모델 적용</h2>
학습한 얼굴 모델 데이터를 불러와 그에 따른 데이터를 처리했다. 또, 헬멧, 바디, 마스크 등 다른 여러 학습 모델들도 마찬가지다.
<script src="https://gist.github.com/cossk3/38691286d0afb030ffd02b8281fcf759.js"></script>


ㅤㅤ   
<h2 id="a4">● QR Reader</h2>
QR Reader는 Serial 통신으로 데이터를 읽어와 처리했다.
<script src="https://gist.github.com/cossk3/9a3ac38924efdeb68156f0142dc0ad7e.js"></script>


ㅤㅤ   
<h2 id="a5">● 혈압 측정계 연동</h2>
혈압 측정계도 마찬가지로 USB를 연결하여 Serial 통신으로 데이터를 읽어와 처리했다.
<script src="https://gist.github.com/cossk3/72dee073655a9baee542f22b9d38a579.js"></script>


ㅤㅤ   
<h2 id="a6">● Mqtt</h2>
서버와의 통신을 위해 Mqtt 통신 모듈을 구현했다.
<script src="https://gist.github.com/cossk3/5a9936a9d325766bbda42a29924c8e89.js"></script>


ㅤㅤ   
<h2 id="a7">● Http</h2>
타 업체 서버와의 통신을 위해 Http 통신 모듈을 구현했다.
<script src="https://gist.github.com/cossk3/c90c1773a509045e61c766c214592334.js"></script>