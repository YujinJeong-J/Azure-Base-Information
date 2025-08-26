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
