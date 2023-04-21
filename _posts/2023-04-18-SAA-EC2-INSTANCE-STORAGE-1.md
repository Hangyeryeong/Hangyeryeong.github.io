---
title: "EC2 INSTANCE STORAGE"

tags:
 - [SAA]

category: AWS Solutions Architect Associate
toc: true
toc_sticky: true

date: 2023-04-18
last_modified_at: 2023-04-18

img: ":EC2 INSTANCE STORAGE.png"

---

<!-- outline-start -->


### EBS 개요<br/>

- EBS Vloume : Elastic Blcok Store<br/>
- 특정 AZ에서만 가능<br/>
- EBS 볼륨 : 네트워크 USB 스틱<br/>
- 스냅샵을 이용하면 다른 AZ로 볼륨을 옮길 수 있음.<br/>
- 용량을 미리 결정해야 함.<br/>
- CCP 레벨의 EBS 볼륨은 1개의 인스턴스만 연결이 가능함.<br/>
- 하나의 인스턴스틑 여러 EBS 볼륨과 연결이 가능함.<br/>
- EBS 볼륨 생성 후 꼭 EC2 인스턴스에 연결될 필요가 없음.<br/>
(필요한 경우에만 연결하면 됨.)<br/><br/>

#### Delete on Termination attribute<br/>

- EBS 볼륨 종료 시 제어할 수 있음.<br/>
- 기본적으로 root EBS vloume은 인스턴스 종료와 함께 삭제되도록 되어 있음.<br/>
- 인스턴스가 종료될 때, root volume을 유지하고자 하는 경우 데이터를 저장하고 싶으면 삭제 속성을 `비활성화`하면 됨.<br/><br/>

#### EBS<br/>

![Untitled](https://user-images.githubusercontent.com/117553252/232615352-3872ac37-b924-49cc-b4b0-ca05b70adefc.png)

<br/><br/>

![Untitled (1)](https://user-images.githubusercontent.com/117553252/232615276-956858e3-b7b1-4400-aea3-e9d4816c9566.png)
<br/><br/>

![Untitled (2)](https://user-images.githubusercontent.com/117553252/232615285-779bcb2b-b0ad-47da-a6b6-6a2b5514cbef.png)
<br/><br/>

### EBS 스냅샵 개요<br/>

- EBS 스냅샵 : EBS 볼륨의 특정 시점에 대한 백업<br/>
- 다른 AZ or Region에 스냅샵을 복사할 수 있다.<br/><br/>

#### EBS 스냅샵 특징<br/>

1. EBS Snapshot Archive<br/>
- 75%까지 저렴하게 가능(archive tier를 이용한다면)<br/>
- 24 ~ 72 시간 내에 복구 가능<br/><br/>

2. Recycle Bin for EBS Snapshots (휴지통)<br/>
- 영구 삭제 대신에 휴지통에 넣을 수 있음.<br/>
- 실수로 삭제하는 경우에 휴지통에서 복원할 수 있음.<br/>
- 휴지통에 보관하는 기간 : 1일 ~ 1년<br/><br/>

3. Fast Snapshot Restore (FSR) - 빠른 스냅샷 복원<br/>
- 완전 초기화해 첫 사용에서의 지연 시간을 없애는 기능<br/>
- 스냅샷이 아주 크고 EBS volume or EC2 인스턴스를 빠르기 초기화해야 할 때 유용.<br/>
- 비용이 많이 듦.<br/><br/>

#### EBS Snapshot<br/>

![Untitled (3)](https://user-images.githubusercontent.com/117553252/232615287-408f224f-b4f6-48cb-a2f6-538e8c566f6b.png)
<br/><br/>

![Untitled (4)](https://user-images.githubusercontent.com/117553252/232615290-5bad2f8f-7f39-4de3-a7d9-e63960fc93ef.png)
<br/><br/>

![Untitled (5)](https://user-images.githubusercontent.com/117553252/232615293-819db2b7-983b-4feb-8ecc-23d2abe580c8.png)
<br/>
만든 스냅샷을 확인할 수 있음.
<br/><br/>

#### EBS 스냅샷 다른 Region으로 복사하기<br/>

![Untitled (6)](https://user-images.githubusercontent.com/117553252/232615297-42c055b7-7a19-4b77-bd6e-2c0ccf7b966c.png)
<br/><br/>

![Untitled (7)](https://user-images.githubusercontent.com/117553252/232615298-b9b2293f-d8b7-4594-87f2-57663dfcd54f.png)
<br/><br/>

![Untitled (8)](https://user-images.githubusercontent.com/117553252/232615303-203ffcd8-efe9-4753-a042-f6f4589ec9b6.png)
<br/>
스냅샷에서 볼륨도 생성할 수 있음.
<br/><br/>

#### EBS 스냅샷 휴지통<br/>

![Untitled (9)](https://user-images.githubusercontent.com/117553252/232615306-fadd7977-7013-4352-8fe7-6a9791172116.png)
<br/><br/>

### AMI 개요<br/>

- EC2 인스턴스의 장좀<br/>
- AMI = Amazon Machine Image<br/>
- EC2 인스턴스를 통해 만든 이미지를 통칭함.<br/>
- A public AMI : AWS provided<br/>
- Your own AMI : you make and maintain them yourself<br/>
- An AWS Marketplace AMI<br/><br/>

#### AMI 생성<br/>

- 신규 EC2 인스턴스 만들기<br/>

```yaml
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
```

<br/><br/>

![Untitled (10)](https://user-images.githubusercontent.com/117553252/232615308-1ec60cd0-0cfe-46ad-9656-3c250cd449f3.png)
<br/><br/>

![Untitled (11)](https://user-images.githubusercontent.com/117553252/232615315-2cd30193-3d71-4ecb-94b0-559e7fd5f532.png)
<br/>
홈페이지가 열리기까지 시간이 조금 걸린다.<br/>
이것이 우리가 `AMI`를 사용하는 이유임.<br/><br/>

![Untitled (12)](https://user-images.githubusercontent.com/117553252/232615318-e2c16956-5e64-4781-990c-5b818e195617.png)
<br/><br/>

![Untitled (13)](https://user-images.githubusercontent.com/117553252/232615322-456de773-d86b-4409-b360-3d6bebc569b5.png)
<br/><br/>

![Untitled (14)](https://user-images.githubusercontent.com/117553252/232615324-8947f6d2-23e4-4094-bc64-fd3ed673aac6.png)
<br/><br/>

![Untitled (15)](https://user-images.githubusercontent.com/117553252/232615325-ff7251fa-11f3-406e-bac4-e6ff38c6f4ab.png)
<br/><br/>

![Untitled (16)](https://user-images.githubusercontent.com/117553252/232615329-bb818cc7-0e29-4dc3-9280-3ff22bad24ab.png)
<br/><br/>

```yaml
#!/bin/bash
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

<br/><br/>

![Untitled (17)](https://user-images.githubusercontent.com/117553252/232615332-e033b971-a34d-4e11-91bc-518c1dbf15da.png)
<br/><br/>

![Untitled (18)](https://user-images.githubusercontent.com/117553252/232615335-b8727459-002f-4ab6-adb4-14b0d6ca1b22.png)
<br/><br/>

- 아까보다 훨씬 빨리 홈페이지가 생겼다.<br/>
- 필요한 소프트웨어가 이미 EC2 인스턴스에 설치되어 있기 때문에 부팅 시간이 훨씬 단축됨.<br/>
- 이를 통해 AMI가 일반적인 소프트웨어나 보안 소프트웨어 방화벽 또는 사용자 지정 구성 등의 작업 시 유용함.<br/><br/>

### EC2 인스턴스 스토어<br/>

- 특정 유형의 EC2 인스턴스는 `EC2 인스턴스 스토어`라고 불리며 이는 해당하는 물리적 서버에 연결된 하드웨어 드라이브를 가리킴.<br/>
- 더 나은 I/O 성능에 사용할 수 있음.<br/>
- EC2 인스턴스를 중지 또는 종료를 하면 `해당 스토리지 또한 손실`됨. (임시 스토리지)<br/>
    -> EC2 인스턴스 스토어가 장기적으로 데이터를 보관할 만한 장소가 될 수 없음.<br/>
- 임시적인 콘텐츠의 경우 사용.<br/>
- 장기적 X<br/>
- 장기 스토리지의 경우 EBS가 적합함.<br/>
- EC2 인스턴스 스토어를 사용할 때는 데이터를 백업하거나 복제해 두어야 함.<br/>
- EC2 인스턴스에 성느이 아주 뛰어난 하드웨어가 연결 된 것.<br/>
    -> 로컬 EC2 인스턴스 스토어<br/>
- 32,000 IOPS 이상을 요할 땐, io1 or io2 볼륨의 EC2 Nitro가 필요<br/><br/>

### EBS 볼륨 유형<br/>

- gp2 / gp3 (SSD)<br/>
- io1 / io2 (SSD)<br/>
- st1 (HDD)<br/>
- sc1 (HDD)<br/><br/>

- IOPS = 초당 I/O 작업 수<br/>
- EC2 인스턴스에는 gp2 / gp3, io1 / io2만이 부팅 볼륨으로 사용될 수 있음.<br/><br/>

#### gp2<br/>

- 짧은 지연 시간<br/>
- 효율적인 비용의 스토리지<br/>
- 1GB - 16TiB<br/>
- gp3보다 gp2가 좀 더 오래된 볼륨.<br/>
- gp2 / gp3가 비용 효과적인 스토리지.<br/>
- gp3 : IOPS와 처리량을 `독자적`으로 설정할 수 있음.<br/>
- gp2 : IOPS와 처리량은 `연결`되어 있음.<br/><br/>

#### Provisioned IOPS (PIOPS) SSD<br/>

- 프로비저닝을 마친 IOPS<br/>
- IOPS 성능을 유지할 필요가 있는 주요 비즈니스 애플리케이션이나 16,000 IOPS 이상을 요하는 애플리케이션에 적합함.<br/>
- 일반적으로 DB 워크로드에 적합.<br/>
- io1 / io2 : 4GiB - 16TiB<br/>
- io2 장점 : io1과 동일한 비용으로 내구성과 기가 당 IOPS의 수가 더 높음.<br/><br/>

#### Hard Disk Drives (HDD)<br/>

- st1 / sc1<br/>
- st1 : 빅데이터, 데이터 웨어하우징 로그 처리에 적합<br/>
- sc1 : 접근 빈도가 낮은 데이터에 적합<br/><br/>

### EBS 다중 연결<br/>

- EBS 볼륨의 다중 연결 기능<br/>
- 다중 연결 기능 : 하나의 EBS 볼륨을 같은 가용 영역에 있는 여러 EC2 인스턴스에 연결할 수 있게 해 줌.<br/>
- ONLY io1, io2 제품에서만 사용가능.<br/>
- 한 번에 `16개`의 인스턴스만 같은 볼륨에 연결할 수 있음.<br/>
- 반드시 `클러스터 인식` 파일 시스템을 사용해야 함.<br/><br/>

### EBS 암호화<br/>

- 암호화를 사용해야 하는 이유 : 지연 시간에는 영향이 거의 없고 KMS에서 암호화 키를 생성해 AES-256 암호화 표준을 가짐.<br/><br/>

![Untitled (19)](https://user-images.githubusercontent.com/117553252/232615339-49855828-4f0e-4c07-8dfb-149252b01c05.png)
<br/><br/>

- 암호화 되지 않은 EBS 볼륨에서 생성한 스냅샷은 암호화 되지 않음.<br/>
- 기초가 되는 스냅샷이 암호화 되어 있으면 볼륨도 자동으로 암호화됨.<br/><br/>

### Amazon EFS<br/>

- Elasitc File System<br/>
- 관리형 NFS, 즉 네트워크 파일 시스템<br/>
- 가용성이 높고 확장성도 높음.<br/>
- 가격도 비쌈.<br/>
- Windows가 아닌 `Linux 기반 AMI와 호환`됨.<br/>
- 미리 용량을 계획하지 않아도 됨.<br/>
- 파일 시스템이 자동으로 확장되고 사용량에 따라 요금을 지불함.<br/><br/>

#### Performance mode (set at EFS creation time)<br/>

1. 범용 모드<br/>
- General purpose (default)<br/>
- 최대 I/O : I/O를 최대화하고 싶을 때<br/>
2. 처리량 모드<br/>
- Throughtput mode<br/>
- 버스팅 모드가 기본 값<br/><br/>

#### Storage Classes<br/>

- 스토리지 계층 : 일정 기간 후에 파일을 다른 계층으로 옮기는 기능.<br/>
- Standard 계층 : 액세스가 빈번한 파일<br/>
- EFS-IA(INfrequent Access) 계층 : 파일을 검색할 경우 검색에 대한 비용 발생<br/>
but EFS-IA에 파일을 저장하는 비용은 낮음.<br/>
- EFS-IA를 활성화하려면 `수명 주기 정책`을 사용해야 함.<br/>
- 가용성, 내구성 측면에는 두 가지 옵션이 있음.<br/>
1. Standard 옵션 : Multi-AZ, 프로덕션 사용 가능<br/>
2. One Zone : 개발할 때 좋은 옵션, 기본적으로 백업이 활성화 됨.<br/><br/>

#### Amazon EFS<br/>

![Untitled (20)](https://user-images.githubusercontent.com/117553252/232615343-497c5da5-db78-41e6-9ed8-653b92f53feb.png)
<br/><br/>

![Untitled (21)](https://user-images.githubusercontent.com/117553252/232615346-5f53abb9-424f-4724-bc3b-3ea604fd2123.png)
<br/><br/>

![Untitled (22)](https://user-images.githubusercontent.com/117553252/232615348-1abe69e2-fa94-4039-8671-b096779d5503.png)
<br/><br/>

![Untitled (23)](https://user-images.githubusercontent.com/117553252/232615351-64b9ec21-68c4-4532-9161-e685ff884bf6.png)
<br/><br/>

### EFS vs EBS<br/>

#### EBS Vloumes<br/>

- Elasitc Block System.<br/>
- 한 번에 하나의 인스턴스에만 연결이 가능<br/>
- 특정 AZ에 한정<br/>
- gp2 : 디스크 크기 ↑ → IO ↑<br/>
- io1 : IO를 볼륨 크기와 관계 없이 독립적으로 증가시킬 수 있음.<br/>
(중요한 DB를 실행할 때 좋은 방법)<br/>
- 다른 AZ로 옮기고자 할 때는 가장 먼저 `스냅샷`을 찍어야 함.<br/>
- EC2 인스턴스가 종료되면 인스턴스 내의 root EBS vloume도 기본적으로 종료됨. (원할 경우 비활성화가 가능)<br/><br/>

#### EFS<br/>

- Elastic File System.<br/>
- 여러 개의 AZ에 걸쳐 무수히 많은 인스턴스 들에 연결될 수 있음.<br/>
- ONLY for Linux Instances (POSIX)<br/>
- EBS보다 훨씬 비쌈<br/>
- 비용을 절약하고 싶을 땐 스토리지 티어로 `EFS-IA`를 사용, `제품 수명 정책`을 사용하면 비용을 절감할 수 있음.<br/>
- EFS는 사용한만큼만 비용이 청구됨.<br/>
(EBS : EBS의 드라이브 크기에 따라 정해진 사용량을 지불)<br/>
- 다수의 인스턴스에 걸쳐 연결해야 하는 네트워크 파일 시스템에 적합.<br/><br/>
