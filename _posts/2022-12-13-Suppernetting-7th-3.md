---
title: "DNS/HTTP(Web) Server (ft.www.x.com)"
#excerpt: "국비 수업 복습"

#categories:
# - HRD
tags:
 - [Server]


toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2022-12-13

#img: ":aws 1.png"

---

<!-- outline-start -->


DHCP 서버 : IP 주소 제공

DNS 서버 : 도메인 이름 → IP 주소

                   ex. [naver.com](http://naver.com) → IP 주소 → 웹 서버에 접근

[ DNS 서버]

[naver.com](http://naver.com) = 223.130.195.200

응답은 DNS 서버가 해 주는 것임.

클라이언트가 이름 질의 → 셋팅 되어있는 주소를 DNS 서버가 알려줌 → 본인 pc 에서 ping을 4번 던짐.

그럼 어느 DNS 서버가 하는 걸가 ?

ipconfig (cmd 창에서)

이더넷 어댑터 이더넷 : 자신의 램카드의 실제

ipconfig /all : ip 구성에 대한 정보를 모두 보여달라는 명령어.





# DNS 서버 / HTTP 서버(웹 서버)

x.com

www = 192.168.20.1






# 웹 서버 Setting

설치는 항상 구성요소 추가 /제거에서










R1) DHCP 서버
ip dhcp pool cisco
network (넽웤 단위로 줌) 192.168.10.0 255.255.255.0
이렇게만 줘도 ip, subnet mask는 잘 받아 감.
그러면 내부끼리만 통신이 되는 것임.
그래서 필수적으로 또 옵션이 들어감.
default-router 192.168.10.254 (PC 입장)
dns-server 192.168.20.1
ip dhcp excluded-address (제외할 주소) 고정으로 준 것/ 자동으로 할당하기 싫은 주소를 제외 192.168.10.1 192.168.10.99 ( 영역 제외 주소)
ip dhcp excluded-address 192.168.10.254


정리) R1 기준
#ip dhcp pool cisco
#network 192.168.10.0 255.255.255.0 
#default-router 192.168.10.254
#dns-server 192.168.20.1
#ip dhcp excluded-address
#ip dhcp excluded-address 192.168.10.254











DHCP 클라이언트 설정



이 4줄만 제외하고 다 지우면 됨.
dhcp 자동으로 받아오겠다는 뜻.
하드웨어주소 = 물리적인 주소
행 지울 때 리눅스 - dd













### # DHCP 클라이언트 명령어

윈도우)

```yaml
>ipconfig release → 주소 반납
>ipconfig renew → DHCP에서 주소 요청
>ipconfig /all → 할당 받은 주소 확인

최종적으로 인터넷 잘 되는지 확인.
>ping 192.168.20.1
(IP 주소로는 무조건 ping이 가야함)

>ping www.x.com
(DNS 잘 되는지 확인 / 192.168.20.1 을 받아와야 함)

>웹브라우저로 가서 http://www.x.com
```



리눅스)

```yaml
>dhclient -r → 주소 반납
>dhclient → 주소 요청
>ifconfig → 자신의 ip 주소 확인
>netstat -rn → 게이트웨이 주소 확인
>cat /etc/resolv.conf → 주소 받은 것 확인

cf. 리눅스 인터넷 할 때, startx (그래픽으로 갈 때)
```