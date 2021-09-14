# Prj_GNS_Haneum
한이음 KT 멘토님과 IPv6 네트워크 환경 실습 진행 IPsec과 MACsec을 이용한 계층별 공격 실습 및 방어책 제시

- 프로젝트 설명 동영상  
[![Video Label](http://img.youtube.com/vi/m8Ya1IZkOr4/0.jpg)](https://youtu.be/m8Ya1IZkOr4?t=0s)     

## :computer: Used
- GNS3
- VMware Workstation
- Wireshark
- Ubuntu 18.04
- Window 7
- Kali Linux (2019.03)
- C

## :+1: Achievement
1) 한이음 프로젝트 완료
2) 한국통신학회 하계학술대회 참여 https://www.dbpia.co.kr/Journal/articleDetail?nodeId=NODE10587564
3) 'IPv6 라우팅 프로토콜 개정판' 전공서적 보조 집필


## :memo: Summary
IPv6 네트워크 환경에서의 해킹기법 및 방어기술을 GNS3로 구현하여 IPv6 보안문제에 대한 해결책을 제시한다. 특히, ICMPv6기술 중 하나인 NDP의 멀티캐스트를 이용한 중간자공격(MiMA) 혹은 Dos를 악용한 서비스거부 공격을 구현하여 보안기법을 구안해낸다. 이후, 계층별로 3계층 보안 프로토콜인 IPsec과 2계층 보안 프로토콜인 MACsec을 구현하고 공격을 방어하고 대비하는 과정을 경험하고 분석한다. 마지막으로 구현된 결과를 최종보고서와 논문형태의 산출물로 도출하여 정리한다.

## :memo: Content
**- 시나리오 1 (MACSEC 단독)**    
![image](https://user-images.githubusercontent.com/40004210/133222406-d9ac797c-c42d-442b-b42a-dfc25f08f2af.png)  
따라서 MACSec이 구현된 토폴로지에서는 Host1과 Host2의 MAC Address가 공격자의 ND Spoofing 전후로 변하지 않았다. 이를 통해 MACSec 프로토콜이 2계층 공격에 대응하기 위한 방어책으로 유효하다는 것을 알 수 있다. 그러나 스위치의 상위계층 장비인 라우터의 ND Table을 참조하면 라우터의 ND 테이블은 바뀐 것을 알 수 있다. 이를 통해 MACsec은 레이어 2에서만 작동하므로 단일 LAN 만 보호할 수 있으며 트래픽이 라우팅 될 때 보호 기능을 제공하지 않다는 것을 알 수 있다. 예를 들어 ARP 스푸핑 또는 IP 리디렉션을 사용하여 트래픽을 다른 인터페이스에서 강제로 나가는 공격은 MACsec만으로는 방지 할 수 없다. 즉 내부 LAN의 공격에 대해서는 유효하지만, 외부 LAN으로부터의 공격에 대해서는 유효하지 않는 한계를 지니고 있다.

**- 시나리오 2 (IPSEC 단독)**  
![image](https://user-images.githubusercontent.com/40004210/133222493-78c08472-d882-4ff0-8527-e33733a9c843.png)  
IPsec 토폴로지에서 터널사이 외부랜에서의 MITM공격은 라우터의 MAC주소를 변환 시키는 데는 성공하였지만 암호화된 패킷을 감지하므로 성공적으로 방어하였다고 볼 수 있다. 하지만 터널모드에서의 IPsec은 외부 LAN에서는 패킷이 암호화되지만 내부 LAN에서는 그대로 ICMPv6 패킷이 감지되기 때문에 보안에 취약점이 있다고 할 수 있다.

**- 최종 시나리오 (MACSEC, IPSEC 병행)**    
![image](https://user-images.githubusercontent.com/40004210/133222267-e06ec2a5-0da7-4b55-afe5-a9f466f410ce.png)  
앞서 Parasite6를 통한 외부 LAN과 내부 LAN에서의 스푸핑 공격을 시도하고 결과를 봤다. IPsec은 외부 LAN(라우터 터널링) 보안의 강력함이 있지만 내부 LAN에서의 보안 취약점이 있었다. 반면에, MACsec은 내부 LAN 보안의 강력함이 있지만 외부 LAN 보안의 취약점이 보였다. 따라서 IPsec과 MACsec의 보안 프로토콜 병행 사용을 통한 내외부 LAN의 보안성을 높이고자 아래 토폴로지를 제안한다. Ubuntu2와 Ubuntu3를 기준으로 어디에 공격자가 들어와서 ICMPv6 공격 패킷을 보내도 두 개의 보안 프로토콜의 사전 방어 기법을 통해 방어할 수 있고 네트워크 안전성을 유지할 수 있다. 

## :clipboard: 결론

최근 네트워크 기술을 이용한 다양한 응용 분야의 증가와 기하급수적인 요구로 인한 공급으로 인한 여러 보안 취약점이 생겨나면서 이에 대응 할 보안이 요구되고 있다. 특히 IPv4 주소 자원의 부족으로 IPv6의 공급의 증가가 예상되면서 IPv6 주소 메카니즘의 보안 강화는 충분히 고려되어야 한다. 

IPv6 네트워크 환경에서 IP 계층과 데이터 링크 계층 보안 프로토콜인 IPsec과 MACsec을 구현하면서 각 보안 프로토콜의 방어 기법과 성능을 확인했다. 이를 통해 IPsec와 MACsec의 병행 사용으로 외부 LAN의 데이터 기밀성과 무결성을 유지하고 내부 LAN의 Dos Attack, MITM Attack을 방어하는 강력한 보안 기법임을 제시하였다.

하지만, 두 개의 보안 프로토콜을 함께 사용하는 것은 유지비용과 효율적인 측면에서 바라보았을 때 적절한지 고민해야 한다. MACsec의 경우 주요 네트워크 장비에서는 키 교환을 위해 RADIUS 서버 구축이 필요하며 이는 고가의 장비를 구매해야한다. 또한 유지보수하는 입장에서 두 개의 보안 프로토콜을 관리한다는 것은 때에 따라서 비효율적일 수도 있다. 따라서 기업과 개인은 최적화 된 보안 프로토콜을 사용하여 미래 IPv6 네트워크 환경에서의 안전하고 신뢰할 수 있는 네트워크 기술을 유지해야한다.

또한, 앞으로 IPsec과 MACsec의 보안 프로토콜이 개선되아야할 점을 고쳐나가야 한다. IPsec의 경우 IPv6의 방대한 주소 공간을 효율적으로 관리하기 위해 주소 자동 설정 방식을 제공하는데 이는 관리자의 수동적인 조작 없이 호스트에서 자동으로 주소를 설정하게 된다. 이 때, 인터페이스 주소를 사용하면서 NIC(Network Interface Card)가 교체되지 않는 한 동일한 주소가 설정되는 문제가 있다. 또, IPv6 주소에 네트워크 주소가 암호화나 수정 없이 그대로 사용되어 위치 정보가 노출될 수 있다. 

또한 IPsec의 ISAKMP 프로토콜의 구현 상의 오류로서 영향 받는 플랫폼의 원격 공격자가 악의적으로 조작된 IKE 패킷을 전송하여 서비서 거부가 발생하거나, 포맷 스트링, 버퍼 오버 플로우 취약점이 나타날 수 있다. MACse의 경우에도 하드웨어 ACL(Access Control Lists)은 MACsec 처리 후 수신 트래픽에 적용되기 때문에 ACL에 MACsec 캡슐화가 표시되지 않거나 MACsec에 의해 프레임이 차단된 경우 ACL 작업이 수행되지 않는다. 이외에도 Port mirroring, VCStacking, MRU(Maximum Receive Unit) 보안 문제가 있다. 향후 연구로는 IPsec과 MACsec의 보안 문제를 파악하고 해결하여 미래 네트워크 기술을 안전하고 유지할 수 있는 보안 대책을 마련한다.
