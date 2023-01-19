---
title: "4단계 (Broadcast)"

tags:
 - [Broadcast]


toc: true
toc_sticky: true

date: 2022-12-14
last_modified_at: 2022-12-14

#img: ":aws 1.png"

---

<!-- outline-start -->

### 4단계 (Broadcast) <br/>



DHCP 서버와 Client가 어떻게 주소를 주고 받는지를 이해하자. <br/>

DHCP 서버는 환경에 따라 구성하는 방법만 다르고 나머지는 다 똑같고 매번 나오는 아이임.<br/>
DHCP Client를 킴 (집 컴을 킴) <br/>
키면 DHCP 를 찾는다. → DHCP Discover<br/>
통신하면 전부 브로드캐스트로 전송함.<br/>
DHCP 서버가 돌아가고 있기 때문에 받고, 브로드캐스트해서 또 보낸다.<br/>
여기까지가 컴을 키면 되는 것들.<br/><br/>

DHCP Offer → 서버가 브로드캐스트로 어떤 주소를 쓸래 ? 라고 제안하는 것.<br/>
DHCP Request → offer 한 그 주소를 내가 쓰겠다고 요청하는 것.<br/>
그럼 마지막에 DHCP가 ACK을 함.<br/>
확인 메시지임.<br/>
DHCP ACK<br/><br/>


순서) 항상 이 4단계를 거침.<br/>

```yaml
1. DHCP Discover<br/>
2. DHCP Offer<br/>
3. DHCP Request<br/>
4. DHCP ACK<br/>
```

이 4단계는 전부 브로드 캐스트를 사용함.<br/><br/>


L2 2계층 브로드캐스트 주소 : FFFF.FFFF.FFFF (모든 MAC 주소를 의미함)<br/>
스마트폰 제조 번호랑 비슷함. 전 세계에서 유일함.<br/>

L3 3계층 브로드캐스트 주소 : 255.255.255.255<br/>
2층을 거쳐서 3층으로 올라감. L2 → L3<br/><br/>


#### 1. DHCP Discover <br/>
1. DHCP Discover → 컴 켰을 때<br/>
출발지 : MAC1, 0.0.0.0, 목적지 : FFFF.FFFF.FFFF, 255.255.255.255<br/><br/>

#### 2. DHCP Offer <br/>
2. DHCP Offer → 서버가 이제 오퍼한다.<br/>
출발지 : MAC2, 192.168.10.254, 목적지 : FFFF.FFFF.FFFF(브로드캐스트 사용), 255.255.255.255(아직 할당 안 함 그렇기 때문에 다 받아야 함) / 여기서는 단지 dhcp 서버만 알고 있음., Payload(192.168.10.10/클라이언트에서 제안할 ~ 것들 / 실제 데이터임)<br/><br/>

#### 3. DHCP Request <br/>
3. DHCP Request → 이 정보를 쓰겠다는 것.<br/>
출발지 : MAC1, 0.0.0.0, 목적지 : FFFF.FFFF.FFFF, 255.255.255.255, Payload : 위와 같음.(192.168.10.10)<br/><br/>

#### 4. DHCP ACK <br/>
4. DHCP ACK → 서버에서 ack함.<br/>
출발지 : MAC2, 192.168.10.254, 목적<br/><br/>

payload : 실제 보내려고 하는 정보.<br/><br/>

이 4단계는 모두 브로드캐스트를 이용한다.<br/>
목적지 부분이 모두 브로드캐스트를 사용하는 것을 발견할 수 있다.<br/>
(FFFF.FFFF.FFFF / 255.255.255.255)<br/><br/>

이 4단계를 거쳐야 IP주소가 생기는 것임.<br/><br/>

즉, 4단계를 지나기 전에는 ip주소가 없는 것임.<br/>
4단계를 거쳐야 ip 주소가 생김.<br/><br/>

여기서 중요한 것,<br/>
클라이언트와 서버가 통신을 할 때는 모두 `브로드캐스트`를 사용한다는 것.<br/><br/>


![14](https://user-images.githubusercontent.com/117553252/213336593-c89c9690-50ca-483f-9164-97f26bfadc43.png)
<br/>

이제 이것을 찾아 보는 것임.<br/>
각각의 출발지와 목적지 페이로드까지 찾아보자.<br/><br/>


L10)<br/>
주소 반납, 그리고 오른쪽 마우스 capture<br/>

![15](https://user-images.githubusercontent.com/117553252/213336594-2fbeea12-5b5f-415d-9a79-c9b8c815b940.png)
<br/>

누가 지나갔는지는 첫 번째 칸에서 확인함.<br/>
클릭하면 두 번째 칸에서 안에 세부적인 정보를 제공 함.<br/>
또 두 번째 칸에서 선택하면 이것을 바이트로 바꿔주는 것을 세 번째 칸에서 확인할 수 있음.<br/><br/>


이 상태에서 주소요청을 한다. 그러면 그 4단계가 이루어질텐데 그것을 찾고 discover을 클릭하면 세부 내용을 확인할 수 있다.<br/><br/><br/><br/>



### 실습 과정.<br/>

#### 1. 주소 요청<br/>

![16](https://user-images.githubusercontent.com/117553252/213336596-b9fe6028-868e-4987-86be-99484ec26f0b.png)
<br/>

#### 2. DHCP Discover <br/>

![17](https://user-images.githubusercontent.com/117553252/213336599-1c27b6ae-f936-478b-ad78-dea3775416c8.png)
<br/>
![18](https://user-images.githubusercontent.com/117553252/213336603-a434ba4d-c3d0-406c-a10d-e67a8c71850e.png)
<br/> 출발지 목적지까지 확인 완.<br/>

#### 3. DHCP Offer <br/>

![19](https://user-images.githubusercontent.com/117553252/213336605-49f9c436-bfbd-4363-a4d7-30fd6e50cc58.png)
<br/>

![20](https://user-images.githubusercontent.com/117553252/213336608-e7ae77bd-f27d-4f3d-93cd-56844be84135.png)
<br/> 출발지 목적지까지 확인 완.<br/>
페이로드는 어디에 ? <br/>

![21](https://user-images.githubusercontent.com/117553252/213336610-3786185c-4d3b-498b-8895-695f369e25ea.png)
<br/>
이건가 아무튼 확인 완.<br/>

#### 4. DHCP Request <br/>

![22](https://user-images.githubusercontent.com/117553252/213336612-2b5725ea-2013-481d-b1bb-261b55485e62.png)
<br/>
![23](https://user-images.githubusercontent.com/117553252/213336615-10b71516-df7c-41f5-be6a-011ae6d207d5.png)
<br/>
출발지 목적지까지 확인 완.<br/>
그런데 request의 payload의 값이 offer와 다름.<br/>

왜 그런 걸까 ?
<br/>

#### 5. DHCP ACK <br/>

![24](https://user-images.githubusercontent.com/117553252/213336618-e40d4a9f-7d07-4fe2-8086-3ccd119510b6.png)
<br/>
![25](https://user-images.githubusercontent.com/117553252/213336619-e3413c65-5e81-4304-920c-1ecb7a7863e4.png)
<br/>
![26](https://user-images.githubusercontent.com/117553252/213336620-7dd47be9-6345-4f77-a9b5-5678a541f019.png)
<br/>
출발지 목적지 payload까지 확인 완.<br/>

<br/><br/>

### bootp.dhcp <br/>

![27](https://user-images.githubusercontent.com/117553252/213336622-be0d1484-1360-4b1d-b200-96f89e7d1e48.png)
<br/>

![28](https://user-images.githubusercontent.com/117553252/213336623-2dea4c23-4f1d-4f7d-b0eb-83d69fa9ccde.png)
<br/>
e0/1에서 clear 하고 L10에서 주소 반납 / 주소 받고 S6 샤크에서 확인. <br/>

![29](https://user-images.githubusercontent.com/117553252/213336625-104d1c51-6e1d-497e-ad70-5bb31d41cbe7.png)
<br/>

![30](https://user-images.githubusercontent.com/117553252/213336627-18f7ea12-bca4-463b-b5ff-e2db3714687f.png)

원래는 Destination 에 255.255.255.255가 떠야 하는 데 가상화 환경이라 그런지 주소가 오히려 잡혀버림.<br/><br/><br/>




