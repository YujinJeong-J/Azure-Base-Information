문항1) A사는 Azure에 시스템을 구성하여 사용 중이다. 카카오사태로 인하여 DR시스템을 구축할 것을 권고 받아 DR을 구성하려고 한다. 현재 Korea Central Region을 사용 중에 있으며 DR의 경우 Korea South Region에 구성하려고 한다. 보기 중 적절하지 않은 것을 고르시오. \[4점]

① Korea South Region에 VNET를 생성한다.
② Korea South Region에 현 Korea Central에 사용중인 모든 VM SKU가 DR에 사용가능한지 확인한다.
③ Korea South Region에 구성된 Azure DNS Private Resolver에 장애 상황을 대비하여 Korea Central Region에서 사용하는 도메인 정보를 추가한다.
④ Korea South Region에 On-Premise 데이터센터와 ExpressRoute를 구성한다.
⑤ Korea South Region에 사용하는 outbound용 public IP를 한정하기 위하여 Hub VNET에 NAT Gateway를 구성한다.

## 정답

👉 **③ Korea South Region에 구성된 Azure DNS Private Resolver에 장애 상황을 대비하여 Korea Central Region에서 사용하는 도메인 정보를 추가한다.**

## 해설

### ✅ 정답이 맞는 이유

* **Azure DNS Private Resolver**는 현재 일부 리전에만 제공되는 Managed Service이며, **Korea South Region에서는 제공되지 않는다.**
* 따라서 DR 구성을 위해 Korea South Region에서 동일한 Private Resolver를 운영할 수 없으며, **VM 기반의 DNS 서버를 별도로 구축**해야 한다.
* 즉, 보기 ③의 설명은 현실적으로 불가능하므로 **적절하지 않음**이 정답이다.

### ❌ 다른 보기가 오답인 이유

① **VNET 생성**

* DR 사이트 구성을 위해 별도의 Virtual Network(VNet)를 만드는 것은 필수적이며 올바른 접근이다.

② **VM SKU 가용성 확인**

* 리전별로 제공되는 VM SKU가 다르므로, DR 구성 전 반드시 해당 리전에서 동일 VM SKU를 지원하는지 확인해야 한다. 올바른 절차이다.

④ **ExpressRoute 연결**

* DR 사이트와 On-Prem 데이터센터를 연결해야 장애 시 빠른 전환이 가능하다. DR 환경에서는 권장되는 설정이다.

⑤ **NAT Gateway 구성**

* DR 리전에서도 아웃바운드 트래픽의 Public IP를 제한/고정해야 보안 및 접근제어가 가능하다. Hub VNet에 NAT Gateway를 구성하는 것은 바람직하다.

## 핵심 요점

* DR(Disaster Recovery) 구성 시 리전별 리소스 가용 여부를 반드시 확인해야 한다.
* **특히, Azure 서비스는 리전별로 제공 여부가 다르므로** DNS, VM SKU, ExpressRoute, NAT Gateway 등 리소스 지원 범위를 사전에 검토해야 한다.
* DNS Private Resolver는 모든 리전에서 지원되지 않으므로 DR 시에는 **VM 기반 DNS 서버 대안**을 고려해야 한다.

## 추가 학습 가이드

📘 **Microsoft Learn 공식 문서**

* [Azure 서비스 지역별 가용성 확인](https://azure.microsoft.com/explore/global-infrastructure/products-by-region/)
* [Azure DNS Private Resolver 개요](https://learn.microsoft.com/azure/dns/dns-private-resolver-overview)
* [Azure 재해 복구 전략](https://github.com/YujinJeong-J/Azure-Base-Information/blob/main/Azure%EC%9E%AC%ED%95%B4%EB%B3%B5%EA%B5%AC%EC%A0%84%EB%9E%B5.md)

---
문항2)
A사는 Azure로 인프라를 마이그레이션하려고 하며 그 중 Database는 대부분 MSSQL을 사용 중에 있다. Database 마이그레이션 관련되어 아래 고객의 요구사항이 있어 부합되는 Azure의 Database 서비스 및 기능을 선정하려고 한다. 보기의 내용 중 적절한 것을 선택하여 보기 번호를 답안에 작성하시오. \[4점]

**\[요구사항]**

* Region은 Korea Central을 사용해야 한다.
* 주 Region인 Korea Central과 Pair Region인 Korea South에 Read Replica가 필요하다.
* DR 상황 시 Application들의 Endpoint 변경이 없는 단일 Endpoint가 가능해야 한다.
* Timezone 설정을 할 수 있어야 한다.
* DBLink를 사용할 수 있어야 한다.

**\[보기]**
**\[SQL서버 종류 및 Tier]**
가. Azure SQL Database General Purpose
나. Azure SQL Database Business Critical
다. Azure SQL Managed Instance General Purpose
라. Azure SQL Managed Instance Business Critical

**\[고가용성 옵션]**
가. Active Geo-Replication
나. Auto Failover Group

## 정답

👉 **(1) 라. Azure SQL Managed Instance Business Critical**
👉 **(2) 나. Auto Failover Group**

## 해설

### ✅ 정답인 이유

1. **DBLink 지원**

   * Azure SQL Database는 DBLink를 지원하지 않음.
   * Azure SQL Managed Instance는 \*\*DBLink(Linked Server)\*\*를 지원하며, MSSQL 기반 연동 가능.

2. **Timezone 설정**

   * Azure SQL Database는 UTC 고정(Time Zone 변경 불가).
   * Azure SQL Managed Instance는 **서버 Timezone 설정 가능**.

3. **DR 시 단일 Endpoint 지원**

   * **Auto Failover Group**을 사용하면 DR 시에도 **Application에서 Endpoint 변경 없이** 자동 Failover 가능.
   * 반면, **Active Geo-Replication**은 단일 Endpoint 제공 불가 → Application에서 수동으로 연결 변경 필요.

4. **Read Replica (Pair Region 지원)**

   * Auto Failover Group을 이용해 Korea Central ↔ Korea South 리전 간 읽기 전용 복제(Read-only replica) 가능.

5. **Business Critical Tier**

   * 높은 성능과 HA(고가용성) 요구사항을 만족.
   * DR 요구사항까지 포함하면 "라. Managed Instance Business Critical" 선택이 적합.

### ❌ 오답인 이유

* **가/나. Azure SQL Database General Purpose & Business Critical**

  * DBLink 불가, Timezone 설정 불가 → 고객 요구사항 불충족.

* **다. Azure SQL Managed Instance General Purpose**

  * 기능적으로 가능하나, 요구사항 중 성능 및 고가용성 측면에서 Business Critical을 선택하는 것이 맞음.

* **가. Active Geo-Replication (HA 옵션)**

  * Application Endpoint 변경 필요 → 단일 Endpoint 조건 불충족.

## 핵심 요점

* **Azure SQL Database vs Managed Instance** 차이 이해 필요

  * DBLink, Timezone 설정: Managed Instance만 지원
  * Active Geo-Replication: 단일 Endpoint 제공 불가
  * Auto Failover Group: 단일 Endpoint 제공

* DR 환경 설계 시, **Failover Group을 통한 자동 장애조치**가 핵심

## 추가 학습 가이드

📘 **Microsoft Learn 문서**

* [Azure SQL Managed Instance 개요](https://learn.microsoft.com/ko-kr/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview)
* [Failover Group 개요 (Auto Failover Group)](https://learn.microsoft.com/ko-kr/azure/azure-sql/database/auto-failover-group-overview)
* [Azure SQL Database 고가용성 및 DR 전략](https://learn.microsoft.com/ko-kr/azure/azure-sql/database/high-availability-sla)

# 📊 Azure SQL Database vs Azure SQL Managed Instance 기능 차이

| 구분                                                | **Azure SQL Database**                                                                  | **Azure SQL Managed Instance (MI)**                            |
| ------------------------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **배포 모델**                                         | 단일 DB, 탄력적 풀 (Elastic Pool)                                                             | Instance 단위 (온프레미스 SQL Server와 유사)                             |
| **SQL Server 기능 호환성**                             | 일부 기능 제한적 지원                                                                            | 거의 100% SQL Server 엔진 호환                                       |
| **DBLink / Linked Server 지원**                     | ❌ 지원 안 함                                                                                | ✅ 지원 (단, SQL Server ↔ SQL Server)                              |
| **Timezone 설정**                                   | ❌ UTC 고정                                                                                | ✅ Timezone 설정 가능                                               |
| **인스턴스 수준 기능** (SQL Agent, Service Broker, CLR 등) | ❌ 미지원                                                                                   | ✅ 대부분 지원                                                       |
| **네트워크**                                          | Public Endpoint (기본), VNet 통합 옵션 가능                                                     | 반드시 VNet에 배포 (Private IP)                                      |
| **고가용성 옵션**                                       | - Active Geo-Replication (단일 DB 기준, 최대 4개 Secondary)<br>- Auto Failover Group (단일 DB/풀) | - Auto Failover Group (인스턴스 단위)                                |
| **DR 시 단일 Endpoint 지원**                           | Auto Failover Group 사용 시 가능                                                             | Auto Failover Group 사용 시 가능                                    |
| **보안/인증**                                         | - Azure AD 인증<br>- TDE 기본 적용<br>- Key Vault 통합                                          | - Azure AD 인증<br>- TDE 기본 적용<br>- Key Vault 통합<br>- VNet 내부 전용 |
| **사용 사례**                                         | 클라우드 네이티브 신규 앱, SaaS, 단일 DB 또는 다수 소규모 DB 운영                                             | 온프레미스 SQL Server Lift & Shift, 기존 앱 호환성 필요 시                   |
| **비용/과금 모델**                                      | Database 단위 과금 (vCore/DTU 기반)                                                           | Instance 단위 과금 (vCore 기반)                                      |

---

문항3)
김선임은 C고객사의 시스템을 운영 중 Private 네트워크에서만 접근해야 하는 중요 파일 저장용 Storage Account에 퍼블릭 접근 권한을 실수로 부여하였다. 일주일 후 해당 조치가 문제가 있다는 것을 확인하여 퍼블릭 접근 권한을 회수 조치하였다. 추후 동일 이슈가 발생하지 않도록 변경사항에 대해 사전에 자동으로 모니터링 될 수 있도록 방안을 수립하기로 하였다. 다음 중 적절하지 못한 것을 고르시오. \[4점]

① Azure Policy를 적용하여 퍼블릭 접근이 허용된 Storage Account에 대해 Audit Log를 남기고 알람을 발생시키도록 한다.
② Azure Automation을 활용하여 주기적으로 퍼블릭 접근이 허용된 Storage Account을 확인하고 조치할 수 있도록 한다.
③ Azure CLI를 활용하여 Resource Graph에 질의하여 private endpoint가 구성되지 않은 Storage Account을 확인하고 수정할 수 있도록 한다.
④ Azure 활동 로그 이벤트를 수신할 수 있는 Event Grid와 Logic Apps를 활용하여 퍼블릭 접근 권한 부여되는 Storage Account을 확인하고 수정할 수 있도록 한다.
⑤ Storage Account에 진단 설정을 활성화하여 익명 요청이 성공한 Storage Account을 확인하고 수정할 수 있도록 한다.

## 정답

👉 **③ Azure CLI + Resource Graph (private endpoint 여부 확인)**

## 해설

### ✅ 정답이 맞는 이유

* **Private Endpoint 구성 여부와 퍼블릭 접근 차단은 별개의 설정**이다.

  * Private Endpoint가 없다고 해서 반드시 Public Access가 열려 있는 것은 아님.
  * 반대로 Private Endpoint가 있어도 Public Access를 허용해 둘 수 있음.
* 따라서 Resource Graph 질의로 **private endpoint 미구성 여부만 확인하는 것은 퍼블릭 접근 모니터링과 직접적인 관련이 없음**.
* 즉, 보기 ③은 고객 요구사항(퍼블릭 접근 모니터링)과 부합하지 않으므로 적절하지 않다.

### ❌ 다른 보기가 정답이 아닌 이유

① **Azure Policy**

* 특정 Storage Account의 **Public Network Access 설정(Allow/Deny)** 여부를 Audit/Enforce 가능.
* 조건 위반 시 Audit Log 및 경고 발생 → 요구사항 충족.

② **Azure Automation**

* Runbook을 통해 스케줄 기반으로 퍼블릭 접근 허용 여부 확인 및 자동 Remediation 가능.
* 요구사항에 적절함.

④ **Event Grid + Logic Apps**

* Resource Manager 이벤트(예: Storage Account 설정 변경)를 실시간 수신 후 Logic Apps로 워크플로우 실행.
* 퍼블릭 접근 권한 변경 이벤트에 대해 자동 대응 가능.

⑤ **진단 설정(진단 로그 + Metrics)**

* 익명 요청 성공 로그를 수집/분석하여 보안 취약점 탐지 가능.
* 퍼블릭 접근이 실제로 발생했는지 검증할 수 있어 유효.

## 핵심 요점

* **Storage Account 접근 제어 관련 설정**

  * Public Network Access (Allow/Deny)
  * Private Endpoint 여부 → 별개 설정임.
* 모니터링/통제 수단

  * **Azure Policy** → 사전적 제어 및 감사
  * **Event Grid + Logic Apps** → 실시간 이벤트 기반 제어
  * **Azure Automation** → 주기적 점검 및 조치
  * **진단 설정** → 사후 분석 및 감지

## 추가 학습 가이드

📘 **Microsoft 공식 문서**

* [Azure Storage 계정 네트워크 보안](https://learn.microsoft.com/ko-kr/azure/storage/common/storage-network-security)
* [Azure Policy로 리소스 규정 적용](https://learn.microsoft.com/ko-kr/azure/governance/policy/overview)
* [Azure Event Grid + Logic Apps 통합](https://learn.microsoft.com/ko-kr/azure/event-grid/monitor-virtual-machine-changes-logic-app)
* [Azure Storage 진단 로그 및 모니터링](https://learn.microsoft.com/ko-kr/azure/storage/common/monitor-storage)

👉 이 문제의 **출제 의도**는

* "Storage Account 접근 제어 설정을 어떻게 사전/실시간/사후적으로 모니터링할 수 있는가?"
* 특히 **Private Endpoint와 Public Access는 독립 설정임**을 명확히 이해하는지를 확인하는 것입니다.

# 📊 Azure Storage Account 접근 제어 비교표

| 구분                   | Public Network Access                                    | Private Endpoint                                                     | Firewall (IP/Virtual Network 규칙)                                  | 익명 요청(Blob)                                              | 비고                                |
| -------------------- | -------------------------------------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------- | --------------------------------- |
| **설정 위치**            | Storage Account → Networking → **Public Network Access** | Storage Account → Networking → **Private Endpoint Connections**      | Storage Account → Networking → **Firewalls and Virtual Networks** | Storage Account → Configuration → **Blob public access** | 모두 독립적 설정                         |
| **기능**               | 리소스에 **퍼블릭 엔드포인트 허용 여부** 제어 (Allow/Deny)                 | Azure VNet 내부에서만 접근 가능하도록 전용 NIC 생성                                  | 특정 **IP, Subnet, VNet만 허용**                                       | Blob 컨테이너 단위로 **익명(public) 액세스 가능 여부** 제어                | 세부 설정 조합 가능                       |
| **보안 수준**            | 퍼블릭 허용 시 인터넷 어디서든 접근 가능                                  | VNet 내부 통신만 허용 (가장 안전)                                               | 제한된 네트워크만 접근 가능                                                   | 익명 허용 시 인증 없이 Blob 읽기 가능                                 | Private Endpoint + Firewall 조합 권장 |
| **퍼블릭 접근 차단 여부와 관계** | **직접적으로 Public Access 제어**                               | Public Access와 **별개** 설정 → Private Endpoint 있어도 Public Access 열 수 있음 | Public Access와 **보완적 관계**                                         | Public Access와 별개 (Blob 단위)                              | 혼동 주의                             |
| **감사/모니터링 방법**       | Azure Policy, Activity Log, Event Grid                   | Resource Graph로 연결 여부 확인 가능 (단, 퍼블릭 여부와는 무관)                         | Policy 및 Activity Log로 감시 가능                                      | Diagnostic Log로 익명 요청 탐지 가능                              | 보안/컴플라이언스 정책에 따라 모니터링             |

---
문항4)
A사는 차세대 프로젝트를 위해 Azure Public Cloud를 선정하였으며 금융권 Cloud 보안 요건 중 데이터센터 가용성 항목에 대응하기 위해서 Azure의 Availability Zone 기능 검토를 확인 중에 있다. 다음 내용 중 올바른 것을 고르시오. \[4점]

① VM의 경우 Availability Zone 내 Availability Set을 함께 구성하여 VM 가용성을 극대화할 수 있다.
② Standard Load Balancer는 Frontend-ip를 Availability Zone 분산, Availability Zone 지정을 할 수 있으며 Zone 지정을 할 경우 Back-End VM들의 Zone과 동일하지 않아도 LB를 통한 통신을 할 수 있다.
③ Multi Subscription을 사용할 경우 각 Subscription의 Availability Zone Number는 동일한 데이터센터를 의미한다.
④ Azure Virtual Network은 Availability Zone을 사용하기 위하여 Zone별로 Subnet을 생성해야만 한다.
⑤ Azure VPN Gateway는 Availability Zone 분산(Zone Redundant)를 제공하지 않으므로 2개의 Zone을 사용하도록 각각의 VPN Gateway를 구성한다.
## 정답

👉 **② Standard Load Balancer (Zone 지정 및 분산 지원)**
## 해설

### ✅ 정답이 맞는 이유

* **Standard Load Balancer**는 **Zone-redundant** 및 **Zone-specific** 구성을 지원.
* Frontend IP를 **모든 가용 영역에 분산**하거나, 특정 Zone으로 지정할 수 있음.
* Back-End Pool에 연결된 VM 인스턴스가 **다른 Zone에 있어도 정상적으로 부하 분산 가능**.
* 따라서 ②가 올바른 설명이다.
### ❌ 오답인 이유

① **VM + Availability Set**

* VM을 Availability Zone에 배포할 경우 **Availability Set을 동시에 지정할 수 없음**.
* 둘은 별개 개념 → 함께 적용 불가.

③ **Subscription 간 Availability Zone 번호**

* Zone 번호(Zone 1, Zone 2, Zone 3)는 **리전 내 논리적 구분**일 뿐, 물리적으로 동일한 데이터센터를 의미하지 않음.
* 다른 구독(Subscription)에서는 Zone 번호와 실제 물리 위치가 일치하지 않을 수 있음.

④ **VNet과 Subnet**

* Virtual Network 및 Subnet은 **Zone-agnostic 리소스**.
* Zone별로 Subnet을 별도 생성할 필요 없음.
* VM이나 NIC 같은 리소스에서 Zone을 지정하면 해당 VNet/Subnet을 그대로 사용 가능.

⑤ **VPN Gateway**

* 고가용성 SKU에서는 **Zone-redundant VPN Gateway** 제공.
* 하나의 게이트웨이를 배포하면 자동으로 여러 Zone에 인스턴스가 분산됨.
* 따라서 ⑤는 잘못된 설명.
## 핵심 요점

* **Availability Zone**은 Azure 리전 내에서 서로 격리된 데이터센터 그룹.
* **Standard Load Balancer**는 Zone-specific / Zone-redundant 지원.
* VM은 **AZ 지정 시 Availability Set과 동시 사용 불가**.
* **VNet/Subnet은 Zone 독립적 리소스**.
* 일부 네트워크 리소스(VPN Gateway, Application Gateway 등)는 Zone-redundant SKU 제공.
## 추가 학습 가이드

📘 **Microsoft Learn 공식 문서**

* [Azure Availability Zones 개요](https://learn.microsoft.com/ko-kr/azure/reliability/availability-zones-overview)
* [Standard Load Balancer와 가용성 영역](https://learn.microsoft.com/ko-kr/azure/load-balancer/load-balancer-standard-availability-zones)
* [VPN Gateway Zone-redundant SKU](https://github.com/YujinJeong-J/Azure-Base-Information/blob/main/Azure%20VPN%20Gateway%20Zone-Redundant%20SKU.md)
* [Availability Set vs Availability Zone](https://learn.microsoft.com/ko-kr/azure/virtual-machines/availability)

# Azure 리소스 유형별 – **Availability Set, Availability Zone, Zone-Redundant 리소스** 비교표

| 항목              | Availability Set                                                                        | Availability Zone                                                                                        | Zone-Redundant 리소스 (예: VPN Gateway AZ SKU) |
| --------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| **개념**          | 하나 리전 내의 장애 도메인(AD)과 업데이트 도메인(UD) 활용                                                    | 서로 다른 물리적 데이터 센터에 리소스 분산                                                                                 | Availability Zone 내 여러 AZ에 자동 배포           |
| **지원 리소스 예시**   | VM                                                                                      | VM, Load Balancer 등                                                                                      | VPN Gateway (VpnGw1AZ 등), ExpressRoute GW  |
| **병렬 사용 가능 여부** | AZ와 함께 지정 불가                                                                            | VM 배포 시 선택 가능                                                                                            | VPN Gateway 구축 시 AZ‑SKU 사용으로 자동 적용         |
| **가용성 수준**      | 장애 도메인 단위 분산                                                                            | 리전 내 AZ 간 물리적 분리                                                                                         | 완전 자동 AZ 복제 구조 제공                          |
| **설정 방식**       | Availability Set 그룹에 VM 배치                                                              | VM 배포 시 Zone 명시                                                                                          | VPN 게이트웨이 생성 시 AZ‑SKU 선택                   |
| **용도**          | 동일 데이터센터 내의 장애 격리                                                                       | 리전 내 장애 분산                                                                                               | 네트워크 고가용성 및 장애 대응 강화                       |
| **참고 문서**       | [AZ vs AS 비교 문서](https://learn.microsoft.com/ko-kr/azure/virtual-machines/availability) | [Availability Zones 개요](https://learn.microsoft.com/ko-kr/azure/reliability/availability-zones-overview) | 아래 Azure VPN Gateway AZ‑SKU 정리 참고          |

---
문항5)
B사는 VM 기반 WEB / WAS 서버를 이중화 구성하였고, 대용량 파일(동영상 파일 또는 데이터 분석을 위한 로우 파일)의 저장을 위해 Azure Files를 공유 볼륨으로 구성하여 VM에 Mount하였다. 처리하는 파일 개수가 증가함에 따라 응답시간에 대한 지연이 발생하였고, 원인 분석 결과는 많은 대용량 파일의 업로드 / 다운로드로 인한 WAS 서버의 리소스 사용량이 증가였다. Azure Files의 성능 지표에서는 요청에 대한 응답 병목은 발생하지 않았다. WAS 서버의 리소스 사용량 감소를 위한 방안으로 보안을 최대한 준수하면서도 비용 효율적인 방법을 고르시오. \[4점]

① WAS에 클라이언트 인증 서비스를 구성하여 인증된 클라이언트에게 Azure Files의 Access Key를 제공하여 직접 접근하여 사용하도록 한다.
② WAS에 클라이언트 인증 서비스를 구성하여 인증된 클라이언트에게 Azure Files의 SAS (Shared access signature)를 제공하여 클라이언트에서 직접 접근하여 사용하도록 한다.
③ Azure Shard Disk를 생성하여 VM에 할당하여 사용한다.
④ WAS에 클라이언트 인증 서비스를 구성하여 인증된 클라이언트에게 Azure Blob Storage의 SAS (Shared access signature)를 제공하여 클라이언트에서 직접 접근하여 사용하도록 한다.
⑤ 대용량 파일 업로드/다운로드용 Azure Function을 추가로 생성하여 사용한다.
## 정답

👉 **④ Azure Blob Storage + SAS**
## 해설

### ✅ 정답이 맞는 이유

* **Azure Files vs Azure Blob Storage**

  * Azure Files: SMB/NFS 기반 공유 디렉터리 제공 → **랜덤 액세스 파일**(예: 로그 파일, 공유 구성 파일)에 적합.
  * Azure Blob Storage: 대용량, 시퀀스 액세스(동영상, 분석용 원시 데이터 등)에 최적화 + 비용 효율적.

* **SAS (Shared Access Signature)**

  * 클라이언트가 WAS 서버를 거치지 않고 **Blob Storage에 직접 접근 가능**.
  * 최소 권한 부여 + 유효기간 설정 가능 → 보안 강화.
  * WAS 서버의 CPU/메모리 부담 감소 → 성능 개선.

따라서 **④번이 보안, 성능, 비용 모든 측면에서 최적**이다.
### ❌ 다른 보기가 오답인 이유

① **Access Key 제공**

* Storage Account Access Key는 전체 권한을 부여 → 보안상 매우 위험.
* 키 유출 시 계정 전체 데이터 접근 가능 → 금융권 규제 위배 가능성 높음.

② **Azure Files + SAS**

* SAS는 권한 위임 측면에서는 안전하지만, Azure Files는 랜덤 액세스 워크로드에 더 적합.
* 대용량 스트리밍/분석 파일에는 비용 및 성능 측면에서 비효율적.

③ **Azure Shared Disk**

* 주로 클러스터링 환경(예: SQL FCI, SAP ASCS 등)에 사용.
* 고비용 + Blob Storage 대비 성능/비용 효율성이 떨어짐.

⑤ **Azure Function 활용**

* Azure Function 실행 시간 제약 (15분 기본, Premium에서 최대 60분).
* 대용량 파일 처리(수 GB \~ TB 단위)에 적합하지 않음.
* 처리량 확장 시 비용 급증 → **비용 효율성 저하**.
## 핵심 요점

* **Azure Files vs Blob Storage 선택 기준**

  * Files → SMB/NFS 기반, 공유 디렉터리 및 랜덤 액세스에 적합
  * Blob → 대용량 시퀀셜 데이터(동영상, 로그 데이터 레이크 등)에 최적화 + 비용 효율적

* **보안 접근 제어**

  * Access Key는 지양 (전체 권한)
  * SAS 권장 (세분화된 권한 + 만료 시간 설정 가능)

* **WAS 서버 부하 감소** → 클라이언트가 Storage에 직접 접근하도록 설계하는 것이 효율적
## 추가 학습 가이드

📘 **Microsoft Learn 공식 문서**

* [Azure Blob Storage 개요](https://learn.microsoft.com/ko-kr/azure/storage/blobs/storage-blobs-introduction)
* [Azure Files 개요](https://learn.microsoft.com/ko-kr/azure/storage/files/storage-files-introduction)
* [Shared Access Signature (SAS) 보안 모델](https://learn.microsoft.com/ko-kr/azure/storage/common/storage-sas-overview)
* [Azure Files vs Blob Storage 선택 가이드](https://learn.microsoft.com/en-us/azure/storage/common/storage-decide-blobs-files-disks?toc=%2Fazure%2Fstorage%2Ffiles%2Ftoc.json)

# 📊 Azure Storage 서비스 선택 가이드 (Files vs Blob vs Disk)

| 항목             | **Azure Files**                                    | **Azure Blob Storage**                  | **Azure Managed Disks**                             |
| -------------- | -------------------------------------------------- | --------------------------------------- | --------------------------------------------------- |
| **접근 방식**      | SMB / NFS 공유 (네트워크 드라이브 마운트)                       | REST API / SDK / Azure Storage Explorer | VM에 직접 연결되는 Block Storage                           |
| **데이터 액세스 패턴** | 랜덤 액세스 (파일 공유)                                     | 시퀀셜 액세스 (대용량, 스트리밍)                     | Low-latency, 고IOPS (DB/애플리케이션 전용)                   |
| **주요 워크로드**    | 애플리케이션 로그 공유, 구성 파일, 사용자 홈 디렉토리, Lift\&Shift 파일 서버 | 동영상, 이미지, IoT 로그, 빅데이터/AI 원본 데이터, 백업    | 데이터베이스(SQL, Oracle), SAP HANA, VM OS 디스크, 트랜잭션 워크로드 |
| **공유 지원 여부**   | 다수 VM에 마운트 가능                                      | 직접 마운트 불가 (API/SDK 이용)                  | 기본적으로 1:1 연결 (단, Shared Disk 기능으로 다중 VM 연결 가능)      |
| **확장성**        | 최대 수 PB (스토리지 계정 한도)                               | 무제한 (Blob 컨테이너 기반)                      | 디스크 단위 (최대 64TB/디스크)                                |
| **비용 효율성**     | 보통 (Files Premium은 고비용)                            | 가장 저렴, 대규모 저장소에 최적                      | 상대적으로 비쌈 (고성능 보장)                                   |
| **성능**         | 파일 단위 공유 성능 (Premium Files는 SSD 기반 고성능)            | Throughput 최적화 (대용량 데이터 처리)             | 초저지연, 높은 IOPS 보장 (예: Ultra Disk)                    |
| **보안/권한 부여**   | Access Key, SAS, AD 인증(Azure AD DS)                | Access Key, SAS, Azure AD RBAC          | VM 기반 보안 및 관리                                       |
| **적합 사례**      | Lift\&Shift 파일 서버, 공유 폴더                           | 동영상 스트리밍, 백업, 데이터레이크 원본 저장소             | DB, 고성능 앱, VM 운영체제 및 트랜잭션성 워크로드                     |

---
문항6)
VM 기반으로 구축된 웹 서비스를 운영하고 있다. 해당 서비스는 Azure Load Balancer로 이중화 구성이 되어 있다. 서비스에 장애가 발생하여 VM 서버 확인 결과 모든 서버에서 서비스 Port를 정상적으로 Listen 하고 있었다. 하지만 한 대의 서버에서 WAS가 Hang이 걸려 요청을 처리하지 못하는 상태임에도 불구하고 Azure Load Balancer는 요청을 장애가 발생한 서버로 전달하고 있다. 다음 제시된 보기 중 장애가 발생한 VM으로 트래픽 전달을 방지하기 위한 Azure Load Balancer 설정 중 변경해야 하는 항목과 내용이 올바른 것을 고르시오. \[4점]

① Health probe 설정에서 프로토콜 항목이 TCP인 경우 HTTP나 HTTPS로 변경 후 웹 서비스의 동작확인이 가능 경로를 추가로 설정한다.
② Load balancing rule 설정에서 Session persistence 설정이 되어 있는 경우 None으로 변경한다.
③ Health probe 설정에서 서비스의 비정상적인 상태를 빠르게 인지하기 위하여 확인 주기를 최소 주기로 설정한다.
④ Health probe 설정에서 프로토콜 항목이 HTTP나 HTTPS인 경우 정상 응답 HTTP 상태 코드 범위를 변경한다.
⑤ Load balancing rule 설정에서 Idle timeout 값을 10 이하로 변경한다.
## 정답

👉 **① Health probe 프로토콜을 TCP → HTTP/HTTPS로 변경**
## 해설

### ✅ 정답이 맞는 이유

* **현재 문제 상황**:

  * Health Probe가 TCP 기반일 경우 단순히 TCP 핸드셰이크 여부만 확인.
  * VM의 포트가 Listen 상태라면 WAS가 Hang 걸려도 Load Balancer는 정상으로 간주 → 트래픽 전달.

* **해결책**:

  * Health Probe를 **HTTP/HTTPS**로 변경하면, 실제 웹 서비스에 요청을 보내고 HTTP 상태 코드 확인 가능.
  * 응답이 200(OK) 외의 코드(예: 403, 500)일 경우 비정상으로 판별하여 해당 VM으로 트래픽 전달 차단.
### ❌ 다른 보기가 정답이 아닌 이유

② **Session persistence 변경**

* 같은 클라이언트의 요청을 같은 VM으로 보내는 옵션일 뿐, WAS Hang 감지와는 무관.

③ **Probe 주기 단축**

* 주기를 줄여도 TCP 체크 자체가 애플리케이션 레벨 장애를 감지하지 못하므로 효과 없음.

④ **정상 응답 코드 범위 변경**

* Azure Load Balancer는 단일 상태 코드(200)만 정상으로 인식.
* HTTP 상태 코드 범위 지정 기능은 **Application Gateway**에서만 지원.

⑤ **Idle timeout 변경**

* 클라이언트와의 세션 유지 시간만 제어할 뿐, WAS 장애 감지와는 무관.
## 핵심 요점

* **Azure Load Balancer Health Probe 종류**

  * **TCP Probe**: 포트 열림 여부만 확인 (단순, 애플리케이션 장애 감지 불가)
  * **HTTP/HTTPS Probe**: 지정된 경로 호출, 응답 코드 확인 (애플리케이션 상태 감지 가능)

* **시험 포인트**:

  * WAS Hang 같은 **애플리케이션 레벨 장애**는 TCP Probe로는 감지 불가 → 반드시 HTTP/HTTPS Probe 사용해야 함.
## 추가 학습 가이드

📘 **Microsoft Learn 공식 문서**

* [Azure Load Balancer Health Probes](https://learn.microsoft.com/ko-kr/azure/load-balancer/load-balancer-custom-probe-overview)
* [Application Gateway Health Probe (HTTP 상태 코드 범위)](https://learn.microsoft.com/ko-kr/azure/application-gateway/application-gateway-probe-overview)

# 📊 Health Probe 비교: Azure Load Balancer vs Application Gateway

| 항목               | **Azure Load Balancer (LB)**                                                          | **Azure Application Gateway (AppGW)**                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Probe 종류**     | TCP / HTTP / HTTPS                                                                    | HTTP / HTTPS                                                                                                               |
| **동작 방식**        | - TCP: 포트 열림 여부 확인 (3-way handshake)<br>- HTTP/HTTPS: 지정된 경로 요청 → **200 응답만 정상**      | 지정된 경로 요청 → **HTTP 상태 코드 범위 설정 가능** (예: 200–399)                                                                           |
| **애플리케이션 상태 감지** | TCP만 사용 시 불가 (WAS Hang 감지 못함)<br>HTTP/HTTPS 사용 시 200 응답만 확인                           | 응답 코드 범위 지정 가능 → 403, 500 등 세밀한 장애 감지 가능                                                                                   |
| **Probe 설정 옵션**  | - Protocol (TCP/HTTP/HTTPS)<br>- Port<br>- Path (HTTP/HTTPS)<br>- Interval, Threshold | - Protocol (HTTP/HTTPS)<br>- Host name 지정 가능<br>- Path (예: `/healthcheck`)<br>- HTTP 상태 코드 범위 지정 가능<br>- Interval, Timeout |
| **사용 목적**        | 네트워크/서버 단위 가용성 확인 (L4)                                                                | 애플리케이션 레벨 헬스 모니터링 (L7)                                                                                                     |
| **적합 워크로드**      | 단순 웹/VM 이중화, DB 프록시 등 **네트워크 레벨 분산**                                                  | WAF(Web Application Firewall), SSL 오프로드, 애플리케이션 인식 기반 라우팅 등 **애플리케이션 레벨 분산**                                               |
| **시험 포인트**       | - LB의 HTTP Probe는 **200 응답만 정상**<br>- 상태 코드 범위 지정 불가                                  | - AppGW Probe는 상태 코드 범위 지정 가능<br>- 더 정교한 장애 감지 가능                                                                          |

---
문항7)
K 책임은 G 고객사의 클라우드 MSP를 수행하는 담당자이다. 디스크 변경 작업을 검토한 내용 중 잘못 설명된 보기를 고르시오. \[3점]

① VM에 데이터 디스크를 추가로 attach 하기 위해서는 VM을 중지시키지 않고 구성이 가능하기 때문에 별도의 서비스 중단 시간을 고려하지 않고 작업을 진행할 수 있다.
② 데이터 디스크를 동일한 유형의 더 큰 사이즈로 resize 하는 경우 VM을 중지시키지 않고 작업이 가능하여 별도의 서비스 중단 시간을 고려하지 않고 작업을 진행할 수 있다.
③ 데이터 디스크가 공유 디스크로 다수의 VM에 할당된 경우 사이즈 조정을 위해서는 모든 VM에서 detach 이후 진행을 해야 하기 때문에 서비스 중단 시간을 확보한 이후 작업을 진행할 수 있다.
④ VM에 attach되었지만 볼륨을 구성하지 않았던 데이터 디스크에 대한 삭제 요청 시에는 VM을 중지시키지 않고 detach가 가능하기 때문에 별도의 서비스 중단 시간을 고려하지 않고 작업을 진행할 수 있다.
⑤ 할당된 데이터 디스크의 사이즈의 축소를 요청하는 경우 용량 문제가 없다면 해당 데이터 디스크에서 직접 용량을 줄일 수 있으므로 서비스 중단 시간을 고려하지 않고 작업을 진행할 수 있다.
## 정답

👉 **⑤ 데이터 디스크 사이즈 축소 가능하다는 설명 (잘못된 내용)**
## 해설

### ✅ 정답이 맞는 이유

* **Azure Managed Disks**는 **크기 확장(Resize Up)은 지원**,
  하지만 **디스크 크기 축소(Resize Down)는 지원하지 않음**.
* 축소하려면:

  1. 새 디스크를 생성 (더 작은 사이즈),
  2. 기존 데이터를 새 디스크로 복제,
  3. 원본 디스크를 삭제해야 함.
* 따라서 보기 ⑤는 잘못된 설명이다.
### ❌ 다른 보기가 정답이 아닌 이유

① **데이터 디스크 추가 Attach**

* VM 실행 중에도 디스크 추가 가능 → 서비스 중단 없음 → 올바른 설명.

② **디스크 Resize (확장)**

* VM 실행 중에도 **동일 디스크 유형 내에서 확장 가능** → 서비스 중단 없음.

③ **공유 디스크 Resize**

* 공유 디스크는 다수 VM에 연결되어 있으므로 **Resize 전 detach 필요** → 서비스 중단 발생.

④ **미사용 디스크 Detach & 삭제**

* 단순히 attach 상태지만 OS/볼륨으로 사용되지 않은 경우, VM 중지 없이 삭제 가능 → 올바른 설명.
## 핵심 요점

* **Azure Managed Disk Resize 규칙**

  * 확장(크기 증가): 지원, 온라인 작업 가능.
  * 축소(크기 감소): 지원 안 함 → 새 디스크로 마이그레이션 필요.
* **Attach/Detach**: 대부분 온라인 가능.
* **공유 디스크(Shared Disk)**: Resize 시 반드시 detach 필요.
## 추가 학습 가이드

📘 **Microsoft Learn 공식 문서**

* [Azure Managed Disks 개요](https://learn.microsoft.com/ko-kr/azure/virtual-machines/managed-disks-overview)
* [Azure Managed Disks 크기 조정](https://learn.microsoft.com/ko-kr/azure/virtual-machines/disks-resize)
* [Azure VM에 디스크 연결/분리](https://learn.microsoft.com/ko-kr/azure/virtual-machines/attach-disk-portal)
* [Azure Shared Disks 개요](https://learn.microsoft.com/ko-kr/azure/virtual-machines/disks-shared)

# 📊 Azure Managed Disk 작업 가능/불가능 요약표

| 작업 항목                                 | 지원 여부 | VM 실행 중 가능 여부          | 비고                        |
| ------------------------------------- | ----- | ---------------------- | ------------------------- |
| **데이터 디스크 Attach**                    | ✅ 가능  | ✅ VM 중지 불필요            | 실행 중인 VM에 추가 연결 가능        |
| **데이터 디스크 Detach**                    | ✅ 가능  | ✅ VM 중지 불필요            | 단, OS 디스크는 Detach 불가      |
| **디스크 Resize (확장)**                   | ✅ 가능  | ✅ VM 중지 불필요            | 동일 디스크 유형 내 확장만 가능        |
| **디스크 Resize (축소)**                   | ❌ 불가  | ❌ 불가                   | 새 디스크 생성 → 데이터 마이그레이션 필요  |
| **OS 디스크 교체**                         | ✅ 가능  | ❌ VM 재시작 필요            | Managed Disk 스왑 또는 복제 방식  |
| **디스크 유형 변경 (예: Standard → Premium)** | ✅ 가능  | ⚠️ VM 중지 필요할 수 있음      | 일부 시나리오는 VM 중지 필요         |
| **공유 디스크(Shared Disk) Resize**        | ⚠️ 가능 | ❌ VM detach 필요         | Resize 전 모든 VM에서 연결 해제 필요 |
| **스냅샷 생성**                            | ✅ 가능  | ✅ VM 실행 중 가능           | Crash-consistent 스냅샷      |
| **디스크 암호화 (SSE, CMK, ADE)**           | ✅ 가능  | ⚠️ 설정 방식에 따라 VM 재부팅 필요 | 보안 정책 연계                  |

---

문항8)
Azure Public 클라우드와 On-premise 간 전용선 연결 구성을 하려고 한다. 보기의 전용선 구축 전 고려해야 할 내용 중 적절하지 않은 것을 고르시오. \[4점]

① Korea Central Region의 Azure ExpressRoute Circuit(Premium SKU)을 Japan EAST Region에 있는 구축된 ExpressRoute Gateway와 연결 후 On-Premise와 Japan EAST Region의 VNET 간의 통신이 가능하다.
② Korea Central Region의 Azure ExpressRoute Circuit(Standard SKU)을 Korea South Region에 있는 ExpressRoute Gateway와 연결 후 On-Premise와 South Region의 VNET 간의 통신이 가능하다.
③ Korea Central Region의 Azure ExpressRoute Circuit과 연결된 On-Premise와 Korea South Region의 Azure ExpressRoute Circuit과 연결된 On-Premise는 서로 통신이 가능하므로 On-Premise 간 통신을 위해 별도의 전용회선 연결이 불필요하다.
④ Azure ExpressRoute Circuit Private Peering과 ExpressRoute Gateway 간에는 QoS 설정 기능이 없기 때문에 ExpressRoute Gateway에 여러 개의 VNET이 Peering되어 연결된 경우 하나의 VNET에서 전용선 대역폭을 모두 사용하여 다른 VNET에 영향을 끼치지 않도록 설계 시 고려가 필요하다.
⑤ Azure ExpressRoute Circuit과 Azure ExpressRoute Gateway를 통해 두 개의 VNET이 연결되어 있는 경우 ExpressRoute Circuit 자체의 라우팅 기능을 통해 VNET 간 직접 통신이 가능하다.
## 정답

👉 **③ On-Premise 간 통신 가능하다는 설명 (잘못된 내용)**
## 해설

### ✅ 정답이 맞는 이유

* ExpressRoute Circuit은 **On-Premise ↔ Azure** 전용선 연결 제공.
* 단일 Circuit 간에 **다른 On-Premise와 직접 연결 기능은 없음**.
* **On-Premise ↔ On-Premise 간 연결**을 원하면 **Global Reach 옵션**을 추가해야 함.
* 따라서 ③의 설명은 잘못됨.
### ❌ 다른 보기가 정답이 아닌 이유

① **Premium SKU + Cross-region 연결**

* Premium SKU ExpressRoute는 **Cross-region VNet 연결 가능**.
* Korea Central Circuit → Japan East VNet 연결 가능 → 올바른 설명.

② **Standard SKU + 동일 국가 내 Pair Region 연결**

* Standard SKU로도 동일한 리전 내 또는 동일 국가 내 Pair Region VNet 연결 가능.
* Korea Central ↔ Korea South 가능 → 올바른 설명.

④ **QoS 없음 & 대역폭 공유**

* ExpressRoute Gateway에는 QoS 기능 없음.
* 여러 VNet이 Peering되어 Circuit을 공유하면 한 VNet 트래픽이 전체 대역폭을 잠식할 수 있음 → 설계 시 고려 필요 → 올바른 설명.

⑤ **Circuit을 통한 VNet 간 라우팅**

* 동일 ExpressRoute Circuit에 연결된 여러 VNet은 Gateway Transit 기능을 통해 상호 통신 가능.
* ExpressRoute Circuit 자체가 라우팅 테이블을 통해 전달 가능 → 올바른 설명.
## 핵심 요점

* **ExpressRoute SKU 차이**

  * **Standard SKU**: 동일 지역 또는 인접 지역 VNet 연결 지원
  * **Premium SKU**: 글로벌 리전 간 VNet 연결 가능

* **On-Premise ↔ On-Premise 통신**

  * ExpressRoute Circuit만으로는 불가능
  * **ExpressRoute Global Reach** 필요

* **QoS 미지원**

  * Circuit 대역폭은 공유되므로 네트워크 설계 시 트래픽 관리 필요
## 추가 학습 가이드

📘 **Microsoft 공식 문서**

* [Azure ExpressRoute 개요](https://learn.microsoft.com/ko-kr/azure/expressroute/expressroute-introduction)
* [ExpressRoute Global Reach](https://learn.microsoft.com/ko-kr/azure/expressroute/expressroute-global-reach)
* [ExpressRoute FAQ](https://learn.microsoft.com/ko-kr/azure/expressroute/expressroute-faqs)
👉 이 문제의 출제 의도는 \*\*“ExpressRoute는 On-Premise ↔ Azure 전용선이지, On-Premise ↔ On-Premise 연결은 별도 옵션(Global Reach)이 필요하다”\*\*를 구분할 수 있는지 확인하는 것입니다.

# 📊 Azure ExpressRoute 기능 비교표

| 항목               | **Standard SKU**                                                                | **Premium SKU**                                           | **Global Reach (옵션)**                                   |
| ---------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------- |
| **기본 목적**        | On-Prem ↔ Azure 연결 (지역/국가 단위)                                                   | On-Prem ↔ Azure 연결 (글로벌 리전 포함)                            | On-Prem ↔ On-Prem 연결 (ExpressRoute 회선 간 연결)             |
| **VNet 연결 범위**   | - 동일 지역 내 VNet 연결 가능<br>- 일부 Pair Region 연결 가능 (예: Korea Central ↔ Korea South) | - **글로벌 리전 간 VNet 연결 가능** (예: Korea Central ↔ Japan East) | - On-Prem ↔ On-Prem 연결 지원 (Circuit 간 Transit)           |
| **최대 VNet 연결 수** | 최대 10개 (지역 제한)                                                                  | 최대 100개 (Cross-region 지원)                                 | Circuit 2개 연결 시 양쪽 On-Prem 네트워크 간 통신                    |
| **라우팅**          | 지역 내 라우팅                                                                        | 글로벌 라우팅 가능                                                | On-Prem ↔ On-Prem 라우팅 가능                                |
| **비용**           | 기본 요금                                                                           | Standard보다 높은 요금 (Cross-region 포함)                        | 옵션 비용 별도 (Global Reach 추가 요금)                           |
| **주요 사용 사례**     | - 단일 리전 서비스 배포<br>- 동일 국가 내 DR 사이트 연결                                           | - 다국적 기업 글로벌 VNet 연결<br>- 리전 간 DR/BCP 구성                  | - 다수 지사 On-Prem ↔ 본사 On-Prem 연결<br>- MPLS 대체/보완         |
| **제한 사항**        | - Cross-region 불가<br>- 연결 수 제한(10개)                                             | - 비용 증가                                                   | - Azure 리소스 ↔ On-Prem 직접 연결이 아님 (Circuit 간 Transit만 제공) |

---
문항9)
B 사의 김선임은 Kubernetes 기반으로 구축된 홈쇼핑 서비스를 운영하고 있다. 조회용 API 서비스 로그를 확인해보니 간헐적으로 에러가 발생하였으며 원인을 추적한 결과 서비스의 기능 개선 및 오류 수정을 위한 배포 시 POD 시작/종료 시점에 에러가 발생하고 있었다. 조치사항으로 잘못된 것을 고르시오. \[4 점]

① POD 종료 시 에러는 PreStop Hook를 설정하여 Graceful ShutDown을 통해 해결한다.
② ReadinessProbe의 설정을 조정하여 서비스가 정상 기동 후에 K8S Service에 등록되도록 한다.
③ StartUpProbe를 활용하여 LivenessProbe와 ReadinessProbe가 서비스가 정상 기동 후에 동작하도록 한다.
④ Ingress 컨트롤러에서 Internal Error(500) 오류 발생 시 Retry를 적용하여 End-User 에러 발생 빈도를 현저히 감소시킬 수 있다.
⑤ Service Mesh를 사용하고 에러가 발생하는 서버에서 다른 Micro Service Pod의 조회 API를 호출하는 경우 Circuit Breaker의 Retry 설정을 확인하여 다른 Micro Service 배포 시 발생할 수 있는 에러를 줄일 수 있다.
## 정답

👉 **④ Ingress Controller에서 Internal Error(500)에 Retry 적용 (잘못된 조치)**
## 해설

### ✅ 정답이 맞는 이유

* **500 Internal Server Error**는 애플리케이션 내부 로직에서 발생하는 오류.
* 단순히 Ingress Controller에서 Retry를 추가한다고 해서 해결되지 않음.
* Retry는 일시적인 네트워크 오류나 **502/503 Bad Gateway, Connection reset** 같은 경우에는 효과가 있을 수 있지만, **500 오류는 근본적으로 애플리케이션 코드 수정이 필요**.
* 따라서 ④는 잘못된 조치다.
### ❌ 다른 보기가 정답이 아닌 이유

① **PreStop Hook + Graceful Shutdown**

* Pod 종료 전에 애플리케이션이 요청을 정상적으로 마무리할 시간을 확보.
* 무중단 배포 시 필수적인 설정 → 적절한 조치.

② **Readiness Probe 설정**

* Pod이 실제로 정상 기동된 후에만 Service에 트래픽 전달.
* 배포 시 초기 기동 지연으로 인한 오류 방지 → 적절한 조치.

③ **Startup Probe 활용**

* 애플리케이션 초기 기동 시간이 긴 경우 Liveness/Readiness Probe와 충돌을 막아줌.
* 서비스가 안정적으로 올라온 이후에 Probe 동작 → 적절한 조치.

⑤ **Service Mesh Circuit Breaker Retry 설정**

* 다른 Microservice 배포 중 일시적 오류 시 Retry로 안정성 확보 가능.
* 올바른 MSA 패턴 활용 → 적절한 조치.
## 핵심 요점

* **Kubernetes 무중단 배포 핵심 기법**

  * PreStop Hook (Graceful Shutdown)
  * Readiness Probe (정상 기동 후 트래픽 전달)
  * Startup Probe (긴 초기화 시간 보완)
  * Service Mesh Circuit Breaker (MSA 통신 안정화)
* **Ingress Controller Retry 한계**

  * 502/503 등 일시적 네트워크 오류는 Retry로 개선 가능
  * **500 Internal Error는 애플리케이션 코드 문제 → Retry로 해결 불가**
## 추가 학습 가이드

📘 **Microsoft / CNCF 공식 문서**

* [Kubernetes Probes (Liveness, Readiness, Startup)](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [Kubernetes Pod Lifecycle Hooks (PreStop, PostStart)](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/)
* [Service Mesh Circuit Breaker 패턴 (Istio)](https://istio.io/latest/docs/tasks/traffic-management/circuit-breaking/)
* [NGINX Ingress Controller Retry 정책](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#retry)
👉 이 문제의 출제 의도는 \*\*“Kubernetes Pod 배포 시 무중단 서비스 보장을 위한 Probe 및 Hook 활용 이해”\*\*와 **Retry 적용 범위(네트워크 오류 vs 애플리케이션 오류) 구분**을 확인하는 것입니다.

# 🖼️ Kubernetes 무중단 배포 구성요소 관계도

```
[사용자 요청]
      │
      ▼
 ┌───────────────┐
 │ Ingress Controller│
 │ (L7 LB)          │
 └───────────────┘
          │
   ┌──────┴────────┐
   ▼               ▼
[Pod A]          [Pod B]
(구버전)         (신버전)
 │                 │
 │ PreStop Hook     │ Startup Probe
 │ (Graceful        │ (초기화 완료 전
 │  Shutdown)       │  Liveness/Readiness
 │                  │  Probe 지연)
 ▼                  ▼
Readiness Probe   Readiness Probe
(정상 응답 시만   (정상 기동 후에만
 Service 등록)     Service 등록)
```
## 🔑 관계 설명

* **Ingress Controller**

  * 외부 요청을 Service로 라우팅
  * Retry 가능하지만 **500 Internal Error는 해결 불가** (애플리케이션 수정 필요)

* **Pod (구버전 → 신버전)**

  * **PreStop Hook**: 종료 시 기존 요청을 마무리하고 안전하게 내려감
  * **Startup Probe**: 애플리케이션 초기화 완료 전 Liveness/Readiness 검사 지연
  * **Readiness Probe**: 정상 상태가 된 이후에만 Service에 등록, 트래픽 전달

* **Service Mesh (Istio 등)**

  * Microservice ↔ Microservice 호출 시 Circuit Breaker + Retry 적용
  * 배포 중 일시적 장애(502/503) 완화 가능
## ✅ 핵심 요약

* **무중단 배포 4대 요소**

  1. **PreStop Hook** → 종료 시 Graceful Shutdown
  2. **Startup Probe** → 초기화 완료 전 불필요한 Probe 차단
  3. **Readiness Probe** → 정상 상태 이후에만 트래픽 유입
  4. **Service Mesh Circuit Breaker** → MSA 간 통신 오류 완화

* **Ingress Retry 한계**: 네트워크 오류(502/503)는 개선 가능, **500 Internal Error는 불가**

---

문항10)
다음 제시된 보기 중 **Azure Cache for Redis**를 활용하는 예시로 잘못 설명된 보기를 고르시오. \[3점]

① DB에서 데이터를 빈번하게 조회하는 경우 DB에 부하를 주어 성능이 저하될 수 있으므로 해당 서비스를 데이터 캐시로 사용하여 애플리케이션 응답성을 높일 수 있다.
② 해당 서비스를 세션저장소로 사용하여 WAS 간 세션 클러스터링을 구성할 수 있다.
③ 애플리케이션이 비동기 처리를 해야 하나 별도의 Queue가 존재하지 않는 경우 해당 서비스를 활용하여 Queue 서비스를 대체할 수 있다.
④ 해당 서비스는 데이터를 메모리에만 저장하며, Ehcache와 같은 로컬 Cache보다 속도가 빠르므로 애플리케이션에서 데이터 캐시 사용 필요 시 성능 측면에서 해당 서비스를 우선적으로 사용하여야 한다.
⑤ 해당 서비스는 Premium Tier 사용 시 Scale Up/Down뿐 아니라 Scale In/Out도 적용할 수 있어 부하 증가에 따라 유연하게 대응이 가능하다.
## 정답

👉 **④ Ehcache 같은 로컬 캐시보다 Azure Redis Cache가 무조건 더 빠르다** (잘못된 설명)
## 해설

### ✅ 정답이 맞는 이유

* \*\*로컬 캐시(Ehcache)\*\*는 애플리케이션 서버의 메모리에 직접 접근 → 네트워크 구간이 없음 → **속도는 Redis보다 일반적으로 더 빠름**.
* 반면, **Redis는 네트워크 오버헤드**가 있기 때문에 속도는 다소 느리지만, **여러 서버 간 캐시 공유, 데이터 일관성, 영속성 지원** 등의 장점이 있음.
* 따라서 성능만 놓고 보면 로컬 캐시가 더 빠르고, Redis는 **분산 환경, 데이터 동기화, 세션 관리**에 적합.
### ❌ 다른 보기가 정답이 아닌 이유

① **DB 캐시 활용**

* DB 조회 부하를 줄이기 위한 캐시 계층으로 Redis는 대표적인 활용 사례 → 올바른 설명.

② **세션 저장소 활용**

* Redis는 Key-Value 저장소로 세션 공유에 적합 → WAS 간 세션 클러스터링 가능 → 올바른 설명.

③ **간단한 Queue 대체**

* Redis의 **List 자료구조**를 활용하여 Producer/Consumer 패턴 구현 가능 → 기본적인 Queue 역할 가능 → 올바른 설명.

⑤ **Premium Tier Scale In/Out 지원**

* Premium 이상 SKU는 **클러스터링(Sharding)** 지원 → Scale Out 가능 → 부하 증가에 유연 대응 → 올바른 설명.
## 핵심 요점

* **로컬 캐시 vs Redis**

  * 로컬 캐시(Ehcache): 네트워크 구간 없음 → 속도는 가장 빠름, 하지만 **서버별 캐시 동기화 어려움**
  * Redis: 네트워크 오버헤드 있음, 하지만 **분산 캐시, 세션 클러스터링, 고가용성, 영속성** 지원

* **Redis 활용 사례**

  * 캐싱 계층 (DB 부하 완화)
  * 세션 저장소 (WAS 간 공유)
  * 간단한 메시지 Queue
  * 분산 락, Pub/Sub 시스템
## 추가 학습 가이드

📘 **Microsoft Learn 공식 문서**

* [Azure Cache for Redis 개요](https://learn.microsoft.com/ko-kr/azure/azure-cache-for-redis/cache-overview)
* [Azure Cache for Redis 활용 패턴](https://learn.microsoft.com/ko-kr/azure/azure-cache-for-redis/cache-use-cases)
* [Azure Cache for Redis 확장 (Clustering)](https://learn.microsoft.com/ko-kr/azure/azure-cache-for-redis/cache-how-to-premium-clustering)
👉 이 문제의 출제 의도는 \*\*“Redis는 로컬 캐시보다 빠른 게 아니라, 분산 환경에서의 일관성과 고가용성이 장점”\*\*임을 구분하는 데 있습니다.

# 📊 로컬 캐시 vs Redis 캐시 비교표

| 항목          | **로컬 캐시 (예: Ehcache, Caffeine)**   | **Redis 캐시 (Azure Cache for Redis 등)** |
| ----------- | ---------------------------------- | -------------------------------------- |
| **위치**      | 애플리케이션 서버 메모리 내부                   | 외부 독립 캐시 서버 (네트워크 통해 접근)               |
| **속도**      | ✅ 가장 빠름 (네트워크 경로 없음)               | ⚠️ 네트워크 오버헤드 → 로컬 캐시보단 느림              |
| **확장성**     | 서버 단위 (서버 수 늘면 캐시 데이터 분산/불일치 발생)   | 클러스터링/샤딩으로 수평 확장 지원                    |
| **데이터 일관성** | 각 서버마다 별도 캐시 → 데이터 불일치 발생 가능       | 중앙 집중형 캐시 → 다수 서버 간 데이터 일관성 유지         |
| **세션 공유**   | 별도 구현 필요 (Sticky Session 등)        | 기본 지원 (세션 저장소로 활용 가능)                  |
| **영속성**     | ❌ 지원하지 않음 (프로세스 종료 시 캐시 소멸)        | ✅ RDB/AOF 모드로 디스크에 저장 가능               |
| **고가용성**    | ❌ 서버 다운 시 캐시 손실                    | ✅ 복제/페일오버 지원 (클러스터링)                   |
| **주요 활용**   | 단일 서버 환경, 빈번히 사용되는 데이터 캐싱 (속도 최우선) | 분산 환경, 세션 클러스터링, DB 부하 완화, 메시지 큐 대체    |
| **비용**      | 무료 (서버 메모리만 사용)                    | 별도 Redis 인프라 비용 필요                     |

---

문항 11)
A 선임은 Kubernetes 환경의 SpringBoot 2.7.18 (JDK 8u441)로 구성된 Pod 에 장애가 자주 발생하는 것을 조치하기 위해 투입되었다. Deployment.yaml 파일에서 수정되어야 할 부분을 찾아 Line Number 를 작성하시오. \[4 점]

\[추가 확인 사안 및 제한]

* 장애가 발생 중인 Pod 의 SpringBoot 가 사용하는 Max Heap Memory size 는 1GB 이다.
* 원인 파악 진행 중 POD 장애 시 Out Of Memory 관련 로그를 확인하였다.
* **configmap 과 service.yaml 을 수정하지 않는 선에서** 조치를 수행해야 한다.

\[ Pod Describe ]

```
State: Waiting
Reason: CrashLoopBackOff
Last State: Terminated
Reason: OOMKilled
Exit Code: 137
```

`configmap.yaml`

```
apiVersion: v1
data:
  _JAVA_OPTIONS: -Xmx1g
metadata:
  name: cm-backend
  namespace: default
```

`service.yaml`

```
apiVersion: v1
metadata:
  name: svc-backend
  namespace: default
spec:
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: backend
```

`deployment.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: backend:1.14.2
        imagePullPolicy: Always
        envFrom:
        - configMapRef:
            name: cm-backend
        resources:
          requests:
            cpu: 500m
            memory: 500Mi
          limits:
            cpu: 500m
            memory: 500Mi
```

**답 Line : \_\_**
## 정답

👉 **27**
## 해설 (왜 27라인인가?)

* 현재 ConfigMap에서 **`_JAVA_OPTIONS: -Xmx1g`** 로 **JVM 최대 힙(1GiB)** 를 지정했습니다.
* 하지만 `deployment.yaml`에서 **container memory limit = 500Mi** 로 설정되어 있습니다.
* 컨테이너 메모리 제한(500Mi) < JVM 힙(≈1024Mi)이므로 **JVM이 힙을 확보하는 순간 cgroup 제한을 초과**하여 **`OOMKilled (ExitCode 137)`** 이 발생합니다.
* 따라서 **`limits.memory`(문제에서 제시한 27라인)** 을 **힙 + JVM/스레드/네이티브 영역 오버헤드**를 고려해 **충분히 크게** 늘려야 합니다.

  * 실무 권장치: **힙의 1.2\~1.5배 이상**(예: 힙 1Gi → limit **1280Mi\~1536Mi 이상**).
  * 함께 **`requests.memory`** 도 합리적으로 상향(예: 1Gi에 맞춰 1Gi 근처)하는 것이 스케줄링 안정성에 유리합니다.

> 추가 팁
>
> * JDK 8u191+ 에서는 컨테이너 메모리 인식이 기본 활성화(8u441 포함). 그래도 **힙(-Xmx)** 이 **limit** 이하가 되도록 설정하는 것이 원칙입니다.
> * 가능하면 **`-XX:MaxRAMPercentage`** 등 **컨테이너 인식 기반 옵션**으로 힙을 비율로 설정하는 방법도 고려하세요(이번 문제는 configmap/service 수정 금지 조건이므로 제외).
## 핵심 요점

* **컨테이너 memory limit ≥ JVM 힙(-Xmx) + 오버헤드** 가 되도록 설정해야 OOMKilled 방지.
* Spring Boot/Java 컨테이너에서는 **리소스 제한과 JVM 메모리 옵션의 일치**가 매우 중요.
* 장애 메시지 `CrashLoopBackOff` + `OOMKilled(137)` 는 **메모리 제한 불일치**의 전형적인 신호.
## 추가 학습 가이드 (공식 문서)

* AKS 운영 베스트 프랙티스 – 리소스 요청/제한
  🔗 [https://learn.microsoft.com/azure/aks/operator-best-practices-cluster-isolation#resource-requests-and-limits](https://learn.microsoft.com/azure/aks/operator-best-practices-cluster-isolation#resource-requests-and-limits)
* AKS 트러블슈팅(GPU/메모리/OOM 등 일반 가이드 포함)
  🔗 [https://learn.microsoft.com/azure/aks/troubleshooting](https://learn.microsoft.com/azure/aks/troubleshooting)

---

