---
title: "Med, Origon, As를 이용한 입력 경로 조정"

tags:
 - [Server]
category: L3.IPv4.Routing

toc: true
toc_sticky: true

date: 2023-01-13
last_modified_at: 2023-01-13

#img: ":aws 1.png"

---

<!-- outline-start -->

### Ex. BGP next-hop 해결<br/>

사진첨부
<br/><br/>

- ospf는 항상 `/24`를 잊지 말자.<br/><br/>

#### 방법<br/>

R1)<br/>

```yaml
router bgp 10
bgp router-id 1.1.1.1
neighbor 1.1.12.2 remote-as 20
neighbor 1.1.14.4 remote-as 20
network 1.1.1.0 mask 255.255.255.0
```

<br/><br/>

R2)<br/>

```yaml
router bgp 20
bgp router-id 2.2.2.2
neighbor 1.1.12.1 remote-as 10
neighbor 1.1.3.3 remote-as 20
neighbor 1.1.3.3 update-source lo 0
network 1.1.2.0 mask 255.255.255.0
```

<br/><br/>

R3)<br/>

```yaml
router bgp 20
bgp router-id 3.3.3.3
neighbor 1.1.2.2 remote-as 20
neighbor 1.1.2.2 update-source lo 0
neighbor 1.1.4.4 remote-as 20
neighbor 1.1.4.4 update-source lo 0
network 1.1.3.0 mask 255.255.255.0

neighbor 1.1.2.2 route-reflector-client
neighbor 1.1.4.4 route-reflector-client
```

<br/><br/>

R4)<br/>

```yaml
router ospf 1
no network 1.1.14.0 0.0.0.255 area 0

router bgp 20
bgp router-id 4.4.4.4
neighbor 1.1.14.1 remote-as 10
neighbor 1.1.3.3 remote-as 20
neighbor 1.1.3.3 update-source lo 0
network 1.1.4.0 mask 255.255.255.0

neighbor 1.1.14.1 next-hop-self
```

<br/><br/>

#### 결과<br/>

사진첨부
<br/><br/>


### R2R4 peer-group<br/>

사진첨부
<br/><br/>

#### 방법<br/>

- R3의 명령어를 줄이고 싶을 때,<br/><br/>

R3)<br/>

```yaml
router bgp 20
bgp router-id 3.3.3.3
neighbor R2R4 peer-group
neighbor R2R4 remote-as 20
neighbor R2R4 update-source lo 0
neighbor R2R4 route-reflector-client

neighbor 1.1.2.2 peer-group R2R4
neighbor 1.1.4.4 peer-group R2R4

network 1.1.3.0 mask 255.255.255.0
```

<br/><br/>

- 그룹 이름을 peer-group 이라고 함.<br/>
결과는 같음.<br/>
그냥 명령어를 줄이기 위해 사용하는 것임.<br/><br/>

#### 결과<br/>

사진첨부
<br/><br/>

### MED, Origin, As를 이용한 입력 경로 조정<br/>

- MED = Metric<br/><br/>

### MED 값 조정하기<br/>

사진첨부
<br/><br/>

#### 방법<br/>

R2)<br/>

```yaml
ip as-path access-list 1 permit ^$

route-map INGRESS
match as-path 1
set metric 100

router bgp 20
neighbor 1.1.12.1 route-map INGRESS out

clear ip bgp * soft
```

<br/><br/>

R4)<br/>


```yaml
ip prefix-list AS2-NETWORK permit 1.1.2.0/24
ip prefix-list AS2-NETWORK permit 1.1.3.0/24
ip prefix-list AS2-NETWORK permit 1.1.4.0/24

route-map INGRESS
match ip address prefix-list AS2-NETWORK
set metric 200

router bgp 20
neighbor 1.1.14.1 route-map INGRESS out

clear ip bgp * soft
```

<br/><br/>

#### 결과<br/>

사진첨부
<br/>

경로가 바뀐 것을 확인할 수 있다.<br/>
MED 값이 전부 `100`으로 간 것을 확인할 수 있다.<br/>
`>*` = BEST 경로<br/><br/>

### Origin 타입을 이용한 입력 경로 조정<br/>

#### 방법<br/>

R2)<br/>

```yaml
ip prefix-list AS2-NETWORK permit 1.1.2.0/24
ip prefix-list AS2-NETWORK permit 1.1.3.0/24
ip prefix-list AS2-NETWORK permit 1.1.4..0/24

route-map CHANGE-ORIGIN-TYPE
match ip address prefix-list AS2-NETWORK
set origin incomplete

router bgp 20
neighbor 1.1.12.1 route-map CHANGE-ORIGIN-TYPE out

clear ip bgp * soft
```

<br/><br/>

#### 결과<br/>

사진첨부
<br/><br/>

- MED보다 Origin이 우선순위가 더 높다.<br/>
AS를 많이 거쳤다 = 좋은 경로가 아니다.<br/><br/>


### AS 경로 추가를 이용한 입력 경로 조정<br/>

R4)<br/>

```yaml
ip prefix-list AS2-NETWORK permit 1.1.2.0/24
ip prefix-list AS2-NETWORK permit 1.1.3.0/24
ip prefix-list AS2-NETWORK permit 1.1.4.0/24

route-map MORE-AS
match ip address prefix-list AS2-NETWORK
set as-path prepend 2 2

router bgp 20
neighbor 1.1.14.1 route-map MORE-AS out

clear ip bgp * soft
```

<br/><br/>

#### 결과<br/>

사진첨부
<br/><br/>

### 출력 경로 조정<br/>