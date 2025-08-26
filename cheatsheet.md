# 📑 Azure Certification Cheat Sheet (링크 포함)

## 🟦 Cloud Service Architecture

* **Region & DR**

  * Pair Region, VM SKU 가용성 확인
  * DNS Private Resolver 한계 → VM DNS 구성
  * 🔗 [Azure Well-Architected Framework – Reliability](https://learn.microsoft.com/azure/architecture/framework/resiliency/)
  * 🔗 [Azure Regions and Availability Zones](https://learn.microsoft.com/azure/reliability/)

* **MSSQL PaaS 제약**

  * Azure SQL DB vs Managed Instance
  * DBLink, TimeZone, Auto Failover Group (O), Active Geo-Replication(X)
  * 🔗 [Azure SQL Managed Instance Overview](https://learn.microsoft.com/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview)
  * 🔗 [Auto-failover groups](https://learn.microsoft.com/azure/azure-sql/database/auto-failover-group-overview)

* **Storage Account 관리**

  * Public Access 차단: Policy, Event Grid, Logic Apps
  * Private Endpoint ≠ Public Access 차단
  * 🔗 [Azure Storage security guide](https://learn.microsoft.com/azure/storage/blobs/security-recommendations)
  * 🔗 [SAS (Shared Access Signatures)](https://learn.microsoft.com/azure/storage/common/storage-sas-overview)

* **Availability Zone**

  * VM: AZ 지정 시 Availability Set 불가
  * LB: Zone 지정 가능, Backend Zone 불일치 허용
  * VPN Gateway: Zone-Redundant SKU O
  * 🔗 [Azure Availability Zones](https://learn.microsoft.com/azure/availability-zones/az-overview)

* **스토리지 아키텍처**

  * Azure Files → 랜덤 액세스
  * Azure Blob → 대용량/시퀀스(동영상, Raw Data) + SAS 권장
  * 🔗 [Choose between Blob and Files](https://learn.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)

* **Load Balancer**

  * TCP Probe → 단순 연결 확인
  * HTTP/HTTPS Probe → 상태 코드 기반 (200 OK만 정상)
  * 🔗 [Load Balancer health probes](https://learn.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

* **Managed Disk**

  * Resize(확장) O, Shrink(축소) X → 새 디스크+마이그레이션
  * 🔗 [Azure Managed Disks Overview](https://learn.microsoft.com/azure/virtual-machines/managed-disks-overview)

* **ExpressRoute**

  * Global Reach 필요, QoS 없음
  * Standard/Premium SKU 구분
  * 🔗 [ExpressRoute Introduction](https://learn.microsoft.com/azure/expressroute/expressroute-introduction)

---

## 🟦 Cloud Native Architecture

* **K8S 무중단 서비스**

  * PreStop Hook → Graceful Shutdown
  * ReadinessProbe → 정상 후 서비스 등록
  * StartupProbe → 초기 기동 보장
  * Retry: 500 Internal Error 해결 불가
  * 🔗 [Kubernetes Probes](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)

* **Redis 캐시**

  * Cache/세션/Queue 일부 대체 O
  * 로컬 캐시 > Redis 속도 (네트워크 영향)
  * 🔗 [Azure Cache for Redis Overview](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-overview)

* **Pod 메모리 관리**

  * JVM Heap > Limits.memory → OOMKilled 발생
  * Pod 자원(Limits) 반드시 Heap 고려해 설정
  * 🔗 [Configure resource limits in AKS](https://learn.microsoft.com/azure/aks/operator-best-practices-resource-management)

* **IaC & AKS**

  * Terraform: os\_sku = VM OS (컨테이너 OS 아님)
  * Calico = Network Policy
  * 🔗 [Terraform with Azure AKS](https://learn.microsoft.com/azure/developer/terraform/)
  * 🔗 [Network policies with Calico](https://learn.microsoft.com/azure/aks/use-network-policies)

* **NodePool 업그레이드 절차**

  * 신규 NodePool 추가 → Drain → 업그레이드 → Pod 복귀 → 삭제
  * 🔗 [Upgrade an AKS node pool](https://learn.microsoft.com/azure/aks/upgrade-cluster)

* **DevOps CI/CD**

  * GitHub Actions: Job 간 Artifact → upload/download-artifact
  * Docker Build → Push → Helm Deploy
  * 🔗 [GitHub Actions Docs](https://docs.github.com/actions)
  * 🔗 [Azure DevOps Pipelines](https://learn.microsoft.com/azure/devops/pipelines/)

* **소스코드 보안**

  * Secret/Key 코드 내 저장 금지
  * Managed Identity + KeyVault 활용
  * 🔗 [Managed Identity Overview](https://learn.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
  * 🔗 [Azure Key Vault Overview](https://learn.microsoft.com/azure/key-vault/general/overview)

---

## 🟦 Cloud Security Architecture

* **보안 요구사항**

  * JIT Access (Defender) → RDP/SSH 제한
  * NSG Flow Logs → Port 로그
  * UDR + Azure Firewall → Outbound 제어
  * ExpressRoute + VPN Gateway → 전용선 암호화
  * 🔗 [Just-in-Time VM Access](https://learn.microsoft.com/azure/security-center/security-center-just-in-time)
  * 🔗 [NSG Flow Logs](https://learn.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)
  * 🔗 [Azure Firewall Overview](https://learn.microsoft.com/azure/firewall/overview)

* **NSG & ASG**

  * 최소 권한 법칙 적용
  * ASG = 동일 VNET 내만 유효
  * 🔗 [Application Security Groups](https://learn.microsoft.com/azure/virtual-network/application-security-groups)

* **데이터 암호화**

  * 전송: TLS/mTLS
  * 저장: TDE, Always Encrypted, Dynamic Data Masking
  * SSE with CMK (Files/Blob) → KeyVault 연동
  * 🔗 [Encryption in Azure](https://learn.microsoft.com/azure/security/fundamentals/encryption-overview)
  * 🔗 [Always Encrypted](https://learn.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)
  * 🔗 [Dynamic Data Masking](https://learn.microsoft.com/sql/relational-databases/security/dynamic-data-masking)

* **AKS 보안**

  * PSA(Pod Security Admission)
  * runAsUser ≠ 0 → Root 실행 방지
  * Calico Network Policy
  * Secret Store > Secret(Base64 저장)
  * 🔗 [Pod Security Admission (PSA)](https://kubernetes.io/docs/concepts/security/pod-security-admission/)
  * 🔗 [Use Azure Key Vault Provider for Secrets Store CSI Driver](https://learn.microsoft.com/azure/aks/csi-secrets-store-driver)

---

## 🟦 Troubleshooting & New Tech

* **운영 이슈**

  * NSG Rule 우선순위 → LB Health Probe 차단 주의
  * Backup 복제 유형 변경 불가 → 새 정책 필요
  * Private AKS Cluster 중지/재시작 시 Private Link 재생성
  * 🔗 [Azure Backup Documentation](https://learn.microsoft.com/azure/backup/)
  * 🔗 [Troubleshoot Azure Networking](https://learn.microsoft.com/troubleshoot/azure/virtual-network/welcome-azure-network)

* **ExpressRoute Metro**

  * 동일 Region 내 Multi Location 연결 → Location 이중화 제공
  * 🔗 [ExpressRoute Metro](https://learn.microsoft.com/azure/expressroute/expressroute-metro)

---

👉 이 버전을 쓰시면 오픈북 시험에서 **키워드 검색 + 바로 문서 링크 열람**이 가능하니 훨씬 빠르게 답을 찾으실 수 있을 거예요.

원하시면 제가 이걸 **PDF 한 장짜리(2단 편집)** 형태로도 만들어 드릴 수 있습니다.
