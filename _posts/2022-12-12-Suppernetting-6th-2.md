---
title: "Suppernetting"

#categories:
# - HRD
tags:
 - [Subnetting]


toc: true
toc_sticky: true

date: 2022-12-12
last_modified_at: 2022-12-12

#img: ":aws 1.png"

---

<!-- outline-start -->


### Supernetting
<br/>

```yaml
192.168.10.1/24 → 255.255.255.0
192.168.10.1/16 → 192.168.0.0
192.168.10.1/8 →192.0.0.0
```
<br/><br/><br/>

- Supernetting Ex.
<br/><br/>

```yaml
다음을 Supernetting 해 보자.

192.168.1(00000 001).0
192.168.2(00000 010).0
192.168.3(00000 011).0
192.168.4(00000 100).0
192.168.5(00000 101).0
==========================
255.255.11111 000.0 (축약한 네트워크 주소)


축약된 Subnet MAsk = 255.255.248.0
축약된 Network 주소 = 192.168.0.0
축약된 Broadcast 주소 = 192.168.7.255
축약된 것 중 할당 가능한 주소 영역 = 192.168.0.1 - 192.168.7.254
축약된 주소 개수 = 2^11 - 2
```
축약의 결론 : 같으면 1 , 다르면 0

<br/><br/><br/>


### 연습 1
<br/><br/>
- interface fa0/0<br/>
ip address 211.175.185.1 255.255.255.0<br/>
ip address 211.175.186.1 255.255.255.0 secondary (두 번째부터는 secondary 필수)<br/>
ip address 211.175.186.1 255.255.255.0 secondary<br/><br/>

이렇게 하면 주소가 3개가 됨.<br/>
라우터 인터페이스에 주소를 여러 개 줄 때, 두 번째부터는 꼭 `secondary`를 써야 함. 안 그러면 앞의 주소를 덮어 쓰게 됨.<br/>
지금 이 경우는 축약을 하지 않은 경우임.<br/><br/><br/>


### 연습 2
<br/><br/>
- 같은 네트워크로 맞추면 라우터가 없어도 ping이 다 감.<br/>
게이트웨이 주소도 다 같음.<br/>
단지 서브넷 마스크만 다른 것임.<br/>
<br/><br/><br/>


### Ex.
<br/><br/>
![5](https://user-images.githubusercontent.com/117553252/211530355-7356f4ee-c8fb-43a0-83d7-3b58d3e99580.png)
<br/><br/>
![6](https://user-images.githubusercontent.com/117553252/211530357-0931d66a-7597-4959-89eb-9be68abfc59c.png)
<br/><br/>
![7](https://user-images.githubusercontent.com/117553252/211530353-6eb987fe-7f97-4886-bbd5-5816694f984d.png)
<br/><br/>

R1) ip route 명령어 2개<br/>
R2) ip route 명령어 3개<br/>
R3) ip route 명령어 2개<br/>
R4) ip route 명령어 2개<br/><br/>

Serial 에서 255.255.255.0 으로 안 주면 자꾸 overlap이 걸림.<br/>