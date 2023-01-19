---
title: "DNS/HTTP/DHCP Server, Relay Agent"


tags:
 - [Server]


toc: true
toc_sticky: true

date: 2022-12-13
last_modified_at: 2022-12-13

#img: ":aws 1.png"

---

<!-- outline-start -->


DHCP 서버 : IP 주소 제공<br/>
DNS 서버 : 도메인 이름 → IP 주소<br/>
ex. [naver.com](http://naver.com) → IP 주소 → 웹 서버에 접근<br/><br/>

[ DNS 서버]<br/>
[naver.com](http://naver.com) = 223.130.195.200<br/><br/>

응답은 DNS 서버가 해 주는 것임.<br/>
클라이언트가 이름 질의 → 셋팅 되어있는 주소를 DNS 서버가 알려줌 → 본인 pc 에서 ping을 4번 던짐.<br/><br/>

그럼 어느 DNS 서버가 하는 걸가 ?<br/>
ipconfig (cmd 창에서)<br/>
이더넷 어댑터 이더넷 : 자신의 램카드의 실제<br/><br/>

ipconfig /all : ip 구성에 대한 정보를 모두 보여달라는 명령어.<br/><br/>





### DNS 서버 / HTTP 서버(웹 서버)

x.com<br/>
www = 192.168.20.1<br/>

![4](https://user-images.githubusercontent.com/117553252/213332394-f1ae776e-b4d6-4a6f-aba7-42445a9016d4.png)
<br/>
![5](https://user-images.githubusercontent.com/117553252/213332395-e1cbb56e-2cac-44da-98a1-6152cecb7b0f.png)
<br/>
![6](https://user-images.githubusercontent.com/117553252/213332397-1ebb4b76-5e02-4d9e-b289-70a05f405f6f.png)
<br/>
![7](https://user-images.githubusercontent.com/117553252/213332398-e0034bbd-1f98-40cb-ab44-1d4b96c29314.png)
<br/>
![8](https://user-images.githubusercontent.com/117553252/213332401-32d15bd0-c111-4e0c-9c56-0ab5178066b0.png)
<br/>
![9](https://user-images.githubusercontent.com/117553252/213332403-5365904f-383e-4494-b75c-89c1a851a914.png)
<br/>
![10](https://user-images.githubusercontent.com/117553252/213332404-53c69ea4-3a8b-4337-aea3-4a7c5e582e74.png)
<br/>
호스트 = PC <br/><br/>
![11](https://user-images.githubusercontent.com/117553252/213332406-11073e2a-8263-4e44-a08c-b3ef1ccdf371.png)<br/>
실제 컴퓨터와 관계 없음.<br/><br/>
![12](https://user-images.githubusercontent.com/117553252/213332409-a0954a05-44e3-40e5-a715-ff2a92b089c9.png)<br/>
![13](https://user-images.githubusercontent.com/117553252/213332411-cd1db6f6-0b4b-4da3-93bb-9a48ce27a759.png)
<br/>
항상 이름은 거꾸로 www.x.com <br/><br/><br/>






### 웹 서버 Setting <br/>

설치는 항상 구성요소 추가 /제거에서<br/><br/>

![14](https://user-images.githubusercontent.com/117553252/213332414-bc70a56b-3f9b-482d-af6f-3b0dfb054aaf.png)
<br/>
![15](https://user-images.githubusercontent.com/117553252/213332417-02df9ea5-d45b-427a-9f25-1a55cd74843b.png)
<br/>
리눅스 = 아파치<br/>
윈도우 2003 = world wide web <br/><br/>
![16](https://user-images.githubusercontent.com/117553252/213332421-547f06f6-991c-456d-849c-edb19b7c7dea.png)
<br/>
![17](https://user-images.githubusercontent.com/117553252/213332422-a61d81da-27fd-4e18-b0e7-9c2a597217f0.png)
<br/> 오른쪽 마우스 속성 <br/><br/>
![18](https://user-images.githubusercontent.com/117553252/213332424-3bd0f6f8-cfd1-46f7-87ed-6a3f664a4132.png)
<br/>
![19](https://user-images.githubusercontent.com/117553252/213332428-a96100c6-1183-445d-a8d0-2b75377fee9c.png)
<br/>
![20](https://user-images.githubusercontent.com/117553252/213332431-2f772b7e-465a-4748-91d8-9834f352d38b.png)
<br/>
![21](https://user-images.githubusercontent.com/117553252/213332434-547c92a0-b7f5-4389-bf74-7b8997905624.png)
<br/><br/><br/>







R1) DHCP 서버<br/>
ip dhcp pool cisco<br/>
network (넽웤 단위로 줌) 192.168.10.0 255.255.255.0<br/>
이렇게만 줘도 ip, subnet mask는 잘 받아 감.<br/>
그러면 내부끼리만 통신이 되는 것임.<br/>
그래서 필수적으로 또 옵션이 들어감.<br/>
default-router 192.168.10.254 (PC 입장)<br/>
dns-server 192.168.20.1<br/>
ip dhcp excluded-address (제외할 주소) 고정으로 준 것/ 자동으로 할당하기 싫은 주소를 제외 192.168.10.1 192.168.10.99 ( 영역 제외 주소)<br/>
ip dhcp excluded-address 192.168.10.254<br/><br/>


정리) R1 기준<br/>
#ip dhcp pool cisco<br/>
#network 192.168.10.0 255.255.255.0 <br/>
#default-router 192.168.10.254<br/>
#dns-server 192.168.20.1<br/>
#ip dhcp excluded-address<br/>
#ip dhcp excluded-address 192.168.10.254<br/>











### DHCP 클라이언트 설정 <br/>

![22](https://user-images.githubusercontent.com/117553252/213332435-63ea1edc-dc88-4ca6-9f3a-85ffc1066db1.png)
<br/>


이 4줄만 제외하고 다 지우면 됨.<br/>
dhcp 자동으로 받아오겠다는 뜻.<br/>
하드웨어주소 = 물리적인 주소<br/><br/>

행 지울 때 리눅스 - dd<br/><br/>


![23](https://user-images.githubusercontent.com/117553252/213332437-f021e92f-5261-4dab-bf2b-a031137176b9.png)
<br/><br/><br/>



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
<br/><br/><br/>



### DHCP Relay Agent <br/>

- 브로드캐스트는 라우터를 넘지 못 함. (기억해둘 것)<br/>
- L3(IP주소)에 대한 브로드캐스트 = 255.255.255.255 <- 3계층 <br/>
- L2(MAC주소)에 대한 브로드캐스트 = FFFF.FFFF.FFFF <- 2계층 <br/><br/>

- 브로드캐스트 = 강의장<br/>
- 강의장에 있는 사람은 다 들림.<br/>
- 서버만 응답함. <br/><br/>

- MAC 주소 = 물리적인 주소 (모든 PC에 존재함)<br/>
ex. 핸드폰 사면 제조 번호가 있는 것처럼.<br/>
브로드 캐스트는 2개임<br/>
- L3, L2가 있음. IP는 있을 수도 있고 없을 수도 있지만 물리적 주소는 무조건 존재함.<br/><br/>

- 맥 주소가 있기 때문에 구분이 되어 짐.<br/>
- L2를 주면 L3을 받음.<br/><br/>

- 멀리 떨어져 있는 것 : DHCP Relay Agent <br/>
- R4을 위해 더 필요함.<br/><br/>

- R4를 위해 R1에 작성) <br/>

```yaml
ip dhcp pool cisco2
network 192.168.40.0 255.255.255.0
default-router 192.168.40.254
dns-server 192.168.20.1
```
<br/><br/>

#### Relay Agent 구성하는 명령어 <br/>

```yaml
interface e0/0
ip helper-address 1.1.12.1
(뭘 줘도 관계X / 보통 가까운 것을 씀.)
```
<br/>


![24](https://user-images.githubusercontent.com/117553252/213332439-14e24d16-cf5c-4c84-a786-683f631c309a.png)
<br/>

#### 결과 <br/>


![25](https://user-images.githubusercontent.com/117553252/213332440-a837cc65-21d5-494a-9f9a-70d040109e04.png)
<br/>
![26](https://user-images.githubusercontent.com/117553252/213332441-5a4d6c0c-1f85-42ef-8c76-82bc4f4b2522.png)

<br/><br/><br/><br/>





### Ex. <br/>

![27](https://user-images.githubusercontent.com/117553252/213333174-84585f86-fbca-4ef0-8b95-85519db70864.png)
<br/>
www.<br/>
211.171.186.1 <br/>
cisco 3개 입력 (구역이 3개니깐)<br/>
HTTP ip 설정 잘 하기<br/>




