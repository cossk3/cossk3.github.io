---
layout: post
title: "[2023.11 ~ 2023.12] 나만의 냉장고 유통기한 관리 Reminder 앱"
date: "2023-12-26 20:00:00"
categories: ["Portfolio", "Study"]
---

# 냉장고 유통기한 관리 앱 개발
<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/ac495c54-f4af-47e2-b7e8-2fa08baf86b0" width="100%">
</p>

> 개발 언어 : Flutter 
> 개발 도구 : Android Studio, XCode
> 개발 환경 : Windows   
> 개발 기간 : 2023.11 ~ 2023.12   
> 개발 인원 : 1명   
> > <a href="https://github.com/cossk3/reminder/tree/main" style="text-decoration:none;color:red;">Github Repository 링크</a>   
> > <a href="https://apps.apple.com/us/app/reminder/id6473826546" style="text-decoration:none;color:red;">App Store 링크</a>   

---
# 개발 기능
> **[라이브러리]**    
> <a href="#a1" style="text-decoration:none;color:black;">● get</a>   
> <a href="#a2" style="text-decoration:none;color:black;">● sqflite</a>    
> <a href="#a3" style="text-decoration:none;color:black;">● flutter_local_notifications</a>   
> 
   
---   

<h2 id="a1">● get</h2>
IconButton을 누를 시 delete 변수의 상태를 변경하고, 그에 따라 일부 UI Widget을 처리하는데 사용했다.
<script src="https://gist.github.com/cossk3/fe5e351670b48ba226ab619ee980d169.js"></script>   

ㅤㅤ   
displaySetting 변수의 값을 저장하고 불러오기 위해 get_storage 라이브러리를 사용했다.
<script src="https://gist.github.com/cossk3/37925fded7139c779c1b35a7ececf32c.js"></script>   

ㅤㅤ   
<h2 id="a2">● sqflite</h2>
Refrigerator 객체를 관리하기 위해 테이블을 생성하고, item을 추가, 조회하는데 사용했다.
<script src="https://gist.github.com/cossk3/7e94252dee207ff14bd16ed4fc25e074.js"></script>   

ㅤㅤ   
<h2 id="a3">● flutter_local_notifications</h2>
특정 날짜와 시간에 notification을 예약하기 위해 사용했다.
<script src="https://gist.github.com/cossk3/e6370a714efcadfc33daa1ba00aa5eb4.js"></script>