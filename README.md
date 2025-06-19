# 목차
  
- [목차](#목차)
- [가상 네트워크 지원 개요](#가상-네트워크-지원-개요)
- [가상 네트워크 지원 이점](#가상-네트워크-지원-이점)
- [가상네트워크 서브넷 사이즈 고려사항](#가상네트워크-서브넷-사이즈-고려사항)
- [지원 시나리오](#지원-시나리오)
- [지원하는 리전 및 서비스](#지원하는-리전-및-서비스)
- [고려사항](#고려사항)
- [FAQ](#faq)
- [가상 네트워크 지원 구성하기](#가상-네트워크-지원-구성하기)

# 가상 네트워크 지원 개요
  
이 문서는 마이크로소프트의 공식 문서([링크](https://learn.microsoft.com/en-us/power-platform/admin/vnet-support-overview))를 참고하였습니다.
  
Power Pltform의 virtual network(가상네트워크) 지원 기능을 통해 트래픽을 퍼블릭망에 노출하지 않고도 가상 네트워크 내부의 리소스와 Power Platform을 통합할 수 있습니다. Virtual Network 지원은 Azure subnet delegation(서브넷 위임)을 사용하여 Power Platform의 아웃바운드 트래픽을 관리하며, 프라이빗 망을 통하기 때문에 사용자의 리소스가 노출될 우려가 없다는 장점이 있습니다. 가상 네트워크 지원을 활용하면 Power Platform에서 Azure 혹은 온프레미스에 위치한 리소스를 호출할 수 있습니다. 또한 플러그인 및 커넥터를 사용하여 아웃바운드 호출을 수행할 수 있게 됩니다.  
  
일반적으로 Power Platform은 퍼블릭망을 통해 엔터프라이즈 리소스와 통합해서 사용합니다. 퍼블릭망을 사용하는 경우 사용자의 리소스는 Azure IP 대역대를 방화벽에 추가하거나 혹은 서비스태그 목록을 통해 접근할 수 있어야 합니다. 그러나 Power Platform의 가상 네트워크 지원을 사용하면, 프라이빗망을 통해 엔터프라이즈망에 배포된 리소스와 통합해서 사용할 수 있습니다.  
  
가상 네트워크 내에서는 Power Platform의 아웃바운드 트래픽에 대해 완전한 제어가 가능합니다. 만약 가상 네트워크와 관련된 Azure Policy가 적용돼 있다면, 해당 트래픽 역시 Azure Policy가 적용됩니다.  
  
![Power Platform 가상 네트워크 지원 기능을 나타낸 아키텍쳐쳐](screenshots/ppvnetsupportarchitecture.png)
  
# 가상 네트워크 지원 이점
  
- 데이터 보호: 가상네트워크 지원을 사용하면, Power Platform 서비스가 인터넷에 노출되지 않고 사용자의 프라이빗망에 배포된 리소스에 연결할 수 있습니다.
- 무단 접근 차단: 가상네트워크에 배포된 리소스의 방화벽에 Power Platform의 public IP를 추가하지 않아도 연결할 수 있기 때문에 허가받지 않은 접근에 대한 위험이 없습니다.
  
# 가상네트워크 서브넷 사이즈 고려사항
  
Microsoft에서는 프로덕션 환경에 필요한 가용 ip를 25-30개, 개발 환경에 필요한 가용 ip는 6-10개를 권고합니다. 만약 여러 Power Platform 환경에서 동일한 서브넷을 사용한다면 권고 사항보다 더 많은 ip가 필요할 수 있습니다. 주의할 점은, Azure에서 서브넷을 만들 때 5개의 ip는 미리 예약돼 있는 주소라는 점입니다. 서브넷 위임을 설정한 후 서브넷 사이즈를 변경하게 되면 예상과 다르게 동작할 수 있습니다. 만약 서브넷 사이즈 변경이 필요할 경우 우선 위임 기능을 해제한 후 재설정이 필요합니다.  
  
# 지원 시나리오
  
Power Platform에서 가상 네트워크 지원 기능은 Dataverse 플러그인과 커넥터를 모두 지원합니다. Dataverse 플러그인과 커넥터는 Power Apps, Power Automate, Dynamics 365앱에서 외부 데이터 소스로 연결할 때 향상된 데이터 통합 보안 기능을 제공합니다. 
  
- Dataverse 플러그인을 사용하여 Azure SQL, Azure Storage, Blob Storage, Azure Key Vault 등과 같은 클라우드 데이터 소스에 연결할 수 있습니다. 이를 통해 데이터 유출 및 기타 사고로부터 데이터를 보호할 수 있습니다.
  
- Dataverse 플러그인을 사용하여 Azure 내 프라이빗 엔드포인트로 보호된 리소스 또는 프라이빗 망에 배포된 SQL, Web API 등과 같은 리소스에 안전하게 연결할 수 있습니다. 이를 통해 데이터 유출 및 외부 위협으로부터 데이터를 보호할 수 있습니다.
  
- 사용자 지정 커넥터를 사용하여 Azure의 프라이빗 엔드포인트로 보호된 서비스 또는 프라이빗 망에 배포된 서비스에 안전하게 연결할 수 있습니다.
  
- 이 외에도 커넥터를 지원하는 Azure 리소스에 대해 안전하게 연결할 수 있습니다. 아래 지원하는 리전 및 서비스 문단에 지원하는 커넥터가 기재돼있습니다.
  
# 지원하는 리전 및 서비스  
  
**한국 중부 리전 및 한국 남부 리전**을 지원합니다. 이 경우 가상 네트워크 지원 기능을 사용하려면, 가상네트워크(+ 서브넷 위임 기능을 지정할 서브넷)는 한국 남부 리전 및 한국 중부 리전에 각각 배포돼 있어야 합니다. 지원 리전 및 최신 서비스 리스트는 [링크](https://learn.microsoft.com/ko-kr/power-platform/admin/vnet-support-overview#supported-services)를 참고하세요.

| Area      | Power Platform services | Virtual Network support availability|
|-----------|-------------------------|-------------------------|
| Dataverse | [Dataverse plug-ins](/power-apps/developer/data-platform/plug-ins) | Generally available |
| Connectors | <ul><li>[SQL Server](/connectors/sql/)</li><li>[Azure SQL Data Warehouse](/connectors/sqldw/)</li><li>[Azure Queues](/connectors/azurequeues/)</li><li>[Custom connectors](/connectors/custom-connectors/)</li><li>[Azure Key Vault](/connectors/keyvault/)</li><li>[Azure File Storage](/connectors/azurefile/)</li><li>[Azure Blob Storage](/connectors/azureblob/)</li><li>[HTTP with Microsoft Entra ID (preauthorized)](/connectors/webcontents/)</li></ul> | Generally available |
| Connectors | <ul><li>[Snowflake](/connectors/snowflakeip/)</li></ul> | Preview |
  
# 고려사항

위임된 서브넷에서 요청을 실행할 때 발생하는 트래픽은 가상 네트워크에 적용된 정책의 영향을 받게 됩니다. 예를 들어서 가상 네트워크에서 퍼블릭 망으로 발생하는 트래픽을 금지한 상태에서 플러그인이 퍼블릭 망에 있는 서비스를 호출하려고 하는 경우 문제가 발생할 수 있습니다. 이 경우 해당 서비스를 Azure 환경 내 IaaS 방식으로 배포하거나 혹은 Azure의 PaaS 리소스를 사용 중이라면 private endpoint를 사용하는 것을 고려할 수 있습니다. 

# FAQ
  
**가상 네트워크 데이터 게이트웨이(Virtual Network Data Gateway)와 가상 네트워크 지원(Virtual Network support)의 차이점?**  
가상 네트워크 데이터 게이트웨이는 관리형 데이터 게이트웨이로써, 온프렘 환경에 데이터 게이트웨이를 배포하지 않고도 Azure와 Power Platform에 액세스 할 수 있도록 해줍니다. 예를 들어서 Power BI와 Power Platform 데이터 흐름(dataflow)에서 ETL 작업에 최적화 돼있습니다.  
가상 네트워크 지원 기능은 Power Platform 환경에서 가상 네트워크 서브넷을 사용하는 것입니다. Power Platform API가 주로 사용하는 기능이며, 한 번에 많은 양의 요청을 처리할 때 사용할 수 있습니다.  
가상 네트워크 지원 기능을 사용할 수 있는 경우는 Power BI와 Power Platform dataflow를 제외한 모든 Power Platform에서 발생하는 아웃바운드 호출 시나리오입니다. Power BI와 Power Platform dataflow는 가상 네트워크 데이터 게이트웨이를 사용합니다.  
  
**가상 네트워크 지원 기능이 failover를 지원하는지?**  
지원합니다. Power Platform에서 선택할 수 있는 Korea 리전과 연동된 리전은 한국 중부(Korea Central) 및 한국 남부(Korea South)이기 때문에 두 개의 리전에 가상 네트워크를 각각 배포하고 서브넷 위임도 각각 설정해야 합니다. 
  
**가상 네트워크에서 발생하는 아웃바운드 트래픽을 모니터링 할 수 있는지?**  
가능합니다. Network Security Group 혹은 방화벽을 통해 아웃바운드 트래픽을 확인할 수 있습니다.  
  
**플러그인이나 커넥터에서 인터넷으로 아웃바운드 호출을 할 수 있는지?**  
가능합니다. 위임된 서브넷에 NAT Gateway 구성이 필요합니다.  
  
**가상 네트워크 지원 기능을 사용하기 위해 Power Platform 테넌트를 Azure 구독에 연결해야 하는지?**  
해당 기능을 사용하기 위해 Azure 구독에 연결이 필요합니다.  
  
# 가상 네트워크 지원 구성하기
  
핸즈온랩은 [링크](https://github.com/youkhi/Power-Platform-virtual-network-support/blob/main/Configuration%20Hands%20on%20Lab.md)에 따로 정리돼있습니다.
  


