## Azure VPN Gateway Zone-Redundant SKU 정리 및 공식 문서 링크

### 주요 안내 내용:

* **Azure VPN Gateway**를 **Zone‑Redundant 방식**으로 배포하려면, **AZ SKUs** (예: `VpnGw2AZ`, `VpnGw3AZ` 등)를 선택해야 합니다.

  * **일반 (non-AZ) SKUs**는 AZ 분산을 제공하지 않습니다.
    ([Reddit][1], [Microsoft Learn][2], [Microsoft Learn][3], [docs.azure.cn][4], [docs.azure.cn][5])
* **Public IP도 Standard SKU여야 하며**, 별도 Zone 지정 없이 생성 시 Zone‑Redundant 공용 IP가 자동 할당됩니다.
  ([new.rebeladmin.com][6])
* AZ SKU를 이용하면 **두 개 이상의 게이트웨이 인스턴스가 서로 다른 AZ**에 자동으로 분산 배치되어 고가용성을 확보합니다.
  ([Microsoft Learn][7])
* 공식적으로 **Zone‑Redundant VPN Gateway 구현 방법**은 "About zone‑redundant virtual network gateway" 문서를 통해 확인할 수 있습니다.
  ([Microsoft Learn][7])

### 추가 정보:

* **SKU 전환 (migration)**: 2025년 6월 이후부터 기존 non-AZ SKUs는 생성 불가이며, 2025년 9월부터 2026년 9월까지 자동 Seamless migration 제공 예정입니다.
  ([docs.azure.cn][4])
* Reddit 사용자들도 "AZ 버전의 Gateway SKU 사용 및 Standard IP 필요"를 언급하며 일치된 정보를 주고 있습니다:

  > "You need to be using the AZ version of the Gateway SKUs such as VpnGw2AZ. You pay this increased rate and the IP Address must be the standard SKU and can be set as zone redundant and does not incur an additional cost."
  > ([Reddit][1])

---

## 요약

* **Availability Set**은 AZ와 함께 사용할 수 없고, 동일 데이터센터 내의 장애 격리 용도입니다.
* **Availability Zone**은 리전 내 물리적으로 분리된 데이터센터 단위의 리소스 배치를 의미합니다.
* **Zone-Redundant 리소스(SKU)**, 특히 VPN Gateway AZ SKU는 자동으로 AZ 간 복제를 수행해 **고가용성 네트워크 인프라**를 구성합니다.
* VPN Gateway AZ SKU 사용 시 **Standard Public IP**가 필요하며, 생성 시 Zone 분산 자동 구성됩니다.

---

필요하시면, 이 내용을 시험 대비 암기용 도식 이미지로도 구성해 드릴 수 있어요. 말씀만 주세요!

[1]: https://www.reddit.com/r/AZURE/comments/1aurut8/azure_virtual_network_gateway_zone_redundancy/?utm_source=chatgpt.com "Azure Virtual Network Gateway Zone Redundancy Pricing"
[2]: https://learn.microsoft.com/en-us/azure/vpn-gateway/create-zone-redundant-vnet-gateway?utm_source=chatgpt.com "Create a zone-redundant virtual network gateway in Azure ..."
[3]: https://learn.microsoft.com/en-us/azure/vpn-gateway/about-gateway-skus?utm_source=chatgpt.com "About gateway SKUs"
[4]: https://docs.azure.cn/en-us/vpn-gateway/gateway-sku-consolidation?utm_source=chatgpt.com "VPN Gateway SKU consolidation and migration"
[5]: https://docs.azure.cn/en-us/vpn-gateway/vpn-gateway-about-vpngateways?utm_source=chatgpt.com "About Azure VPN Gateway"
[6]: https://new.rebeladmin.com/step-by-step-guide-to-setup-zone-redundant-azure-vpn-gateway-in-azure-availability-zone-powershell-guide/?utm_source=chatgpt.com "Step-by-Step Guide to setup Zone-redundant Azure VPN ..."
[7]: https://learn.microsoft.com/en-us/azure/vpn-gateway/about-zone-redundant-vnet-gateways?utm_source=chatgpt.com "About zone-redundant virtual network gateway in Azure ..."
