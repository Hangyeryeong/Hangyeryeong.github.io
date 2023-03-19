---
title: "Split Horizon 법칙, 해결 3가지 방법"

tags:
 - [Server]
category: L3.IPv4.Routing

toc: true
toc_sticky: true

date: 2023-01-11
last_modified_at: 2023-01-11

#img: ":aws 1.png"

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