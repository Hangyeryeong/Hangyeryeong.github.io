---
title: "Split Horizon 법칙, 해결 3가지 방법"

tags:
 - [Server]
category: L3.IPv4.Routing

toc: true
toc_sticky: true

date: 2023-01-11
last_modified_at: 2023-01-11

img: ":26일차.png"

---

<!-- outline-start -->


### Split Horizon 법칙
<br/><br/>
: iBGP로 광고받은 네트워크는 iBGP로 광고하지 못한다.
<br/>
: bgp 는 neighbor 구성이 되어도 bgp 3대 조건을 만족하지 않으면 정상적으로 라우팅 정보가 교환되지 않는다.<br/>
    -> 3대 조건을 만족하면 정상적으로 라우팅 정보가 교환된다.
<br/><br/><br/>


### 해결 01. Full Mesh  설정 (모든 라우터와 neighbor 구성)
<br/><br/>
사진첨부
<br/><br/>

#### 기본 구성.<br/>


R1)<br/>

```yaml
router bgp 100
bgp router-id 1.1.1.1
neighbor 1.1.12.2 remote-as 100
network 1.1.1.0 mask 255.255.255.0
```
<br/><br/>

R2)<br/>

```yaml
router bgp 100
bgp router-id 2.2.2.2
neighbor 1.1.12.1 remote-as 100
neighbor 1.1.23.3 remote-as 100
network 1.1.2.0 mask 255.255.255.0
```

<br/><br/>


R3)<br/>

```yaml
router bgp 100
bgp router-id 1.1.3.3
neighbor 1.1.23.2 remote-as 100
neighbor 1.1.34.4 remote-as 100
network 1.1.3.0 mask 255.255.255.0
```

<br/><br/>


R4)<br/>

```yaml
router bgp 100
bgp router-id 1.1.4.4
neighbor 1.1.34.3 remote-as 100
network 1.1.4.0 mask 255.255.255.0
```

<br/><br/>

### Split Horizon 법칙<br/>

- BGP 정상 작동 요건<br/>
: bgp는 neighbor 구성이 되어도 bgp 3대 조건을 만족하지 않으면 정상적으로 라우팅 정보가 교환되지 않는다.→ 3대 조건을 만족하면 정상적으로 라우팅 정보가 교환된다.<br/><br/>

### Ex. Split Horizon<br/>

사진첨부
<br/><br/>

#### 방법 - BGP AS 100<br/>

R1)<br/>

```yaml
router bgp 100
bgp router-id 1.1.1.1
neighbor 1.1.12.2 remote-as 100
network 1.1.1.0 mask 255.255.255.0
```

<br/><br/>

R2)<br/>

```yaml
router bgp 100
bgp router-id 1.1.2.2
neighbor 1.1.12.1 remote-as 100
neighbor 1.1.23.3 remote-as 100
network 1.1.2.0 mask 255.255.255.0
```

<br/><br/>

R3)<br/>

```yaml
router bgp 100
bgp router-id 1.1.3.3
neighbor 1.1.23.2 remote-as 100
neighbor 1.1.34.4 remote-as 100
netowrk 1.1.3.0 mask 255.255.255.0
```

<br/><br/>

R4)<br/>

```yaml
router bgp 100
bgp router-id 1.1.4.4
neighbor 1.1.34.4 remote-as 100
network 1.1.4.0 mask 255.255.255.0
```

<br/><br/>

#### Split Horizon 법칙<br/>

ex. 1.1.1.1을 본다고 가정해보자.<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

- 상대 원칙 : 같은 AS내에서의 문제.<br/>

해결하는 방법.<br/>
1. `Full Mesh` 설정<br/>
2. `Route Reflector`<br/>
3. `Confederation`<br/><br/>

### Full Mesh 설정<br/>

- 모든 라우터 간 neighbor를 다 맺어 주는 것.<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

잘 모르겠다.ㅋ<br/><br/>

### Route Reflector<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

### Confederation<br/>

사진첨부
<br/><br/>

#### 방법 - no router bgp 100<br/>

R1)<br/>

```yaml
router bgp 10
bgp confederation identifier 100
bgp confederation peers 20
neighbor 1.1.12.2 remote-as 20
network 1.1.1.0 mask 255.255.255.0
```

<br/><br/>


R2)<br/>

```yaml
router bgp 20
bgp confederation identifier 100
bgp confederation peers 10 30
neighbor 1.1.12.1 remote-as 10
neighbor 1.1.23.3 remote-as 30
network 1.1.2.0 mask 255.255.255.0
```

<br/><br/>

R3)<br/>

```yaml
router bgp 30
bgp confederation identifier 100
bgp confederation peers 20 40
neighbor 1.1.23.2 remote-as 20
neighbor 1.1.34.4 remote-as 40
network 1.1.3.0 mask 255.255.255.0
```

<br/><br/>

R4)<br/>

```yaml
router bgp 40
bgp confederation identifier 100
bgp confederation peers 30
neighbor 1.1.34.3 remote-as 30
network 1.1.4.0 mask 255.255.255.0
```

<br/><br/>

- 결과<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

### Ex.<br/>

#### Full Mesh<br/>

R1)<br/>

```yaml
router bgp 100
bgp router-id 1.1.1.1
neighbor 1.1.12.2 remote-as 100
network 192.168.10.0 mask 255.255.255.0
neighbor 1.1.23.3 remote-as 100
```

<br/><br/>

R2)<br/>

```yaml
router bgp 100
bgp router-id 2.2.2.2
neighbor 1.1.12.1 remote-as 100
neighbor 1.1.23.3 remote-as 100
network 192.168.20.0 mask 255.255.255.0
```

<br/><br/>

R3)<br/>

```yaml
router bgp 100
bgp router-id 3.3.3.3
neighbor 1.1.23.2 remote-as 100
neighbor 4.4.12.6 remote-as 200
network 192.168.30.0 mask 255.255.255.0
neighbor 1.1.12.1 remote-as 100
```

<br/><br/>

#### Router Reflector<br/>

R4)<br/>

```yaml
router bgp 200
bgp router-id 4.4.4.4
neighbor 2.2.12.5 remote-as 200
neighbor 5.5.12.7 remote-as 300
network 192.168.60.0 mask 255.255.255.0
```

<br/><br/>

R5)<br/>

```yaml
router bgp 200
bgp router-id 5.5.5.5
neighbor 2.2.12.4 remote-as 200
neighbor 2.2.23.6 remote-as 200
network 192.168.50.0 mask 255.255.255.0
neighbor 2.2.12.4 route-reflector-client
neighbor 2.2.23.6 route-reflector-client
```

<br/><br/>

R6)<br/>

```yaml
router bgp 200
bgp router-id 6.6.6.6
neighbor 2.2.23.5 remote-as 200
neighbor 4.4.12.3 remote-as 100
network 192.168.40.0 mask 255.255.255.0
```

<br/><br/>

#### Confederation<br/>

R7)<br/>

```yaml
router bgp 10
bgp confederation identifier 300
bgp confederation peers 20
neighbor 3.3.12.8 remote-as 20
neighbor 5.5.12.4 remote-as 200
network 192.168.70.0 mask 255.255.255.0
```

<br/><br/>

R8)<br/>

```yaml
router bgp 20
bgp confederation identifier 300
bgp confederation peers 10 30
neighbor 3.3.12.7 remote-as 10
neighbor 3.3.34.9 remote-as 30
network 192.168.80.0 mask 255.255.255.0
```

<br/><br/>

R9)<br/>

```yaml
router bgp 30
bgp confederation identifier 300
bgp confederation peers 20
neighbor 3.3.34.8 remote-as 20
network 192.168.90.0 mask 255.255.255.0
```

<br/><br/>


- 결과<br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

