# 목차
  
- [목차](#목차)
- [테스트 시나리오](#테스트-시나리오)
- [리소스 생성 순서](#리소스-생성-순서)
- [리소스 생성하기](#리소스-생성하기)

# 테스트 시나리오

![Power Platform 가상 네트워크 지원 기능을 나타낸 아키텍쳐쳐](screenshots/test-scenario-hands-on-lab.jpg)
  
# 리소스 생성 순서

0. 해당 핸즈온랩은 Power Platform virtual network support 기능을 위한 virtual network(리소스 명 : hub-krc-vnet)는 이미 생성되었으며, subnet injection기능까지 구현했다고 가정합니다. 만약 아직 구성하지 않았다면 [링크](https://github.com/youkhi/Power-Platform-virtual-network-support/blob/main/Configuration%20Hands%20on%20Lab.md)를 참조하세요.  
1. spoke-krc-vnet
2. SQL Server 리소스 생성
3. SQL Server 리소스에 public access 막기
4. SQL Server 리소스에 private endpoint 구성하기
5. hub-krc-vnet과 spoke-krc-vnet간 vnet peering 구성하기
6. SQL Server private endpoint의 DNS를 hub-krc-vnet에서 resolution 할 수 있도록 설정하기
7. Power Automate 구성하기
8. 테스트
  
# 리소스 생성하기
  

  

  


