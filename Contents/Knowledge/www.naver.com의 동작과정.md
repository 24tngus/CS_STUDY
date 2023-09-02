`OS` : Window 가정
`브라우저` : Chrome 가정

## 1단계 : Local의 Network구성

`Step 1` DHCP (Dynamic Host Configuration Protocol)

- Local의 네트워크에 연결되어는 집 장비들이 네트워크에 참여하기 위한 조건.
- 가장 먼저 DHCP 서버를 찾아야 사설 IP 주소들을 네트워크에 연결된 클라이언트 기기들에게 할당해준다.
-> 즉 각 기기들은 DHCP 서버에 요청을 보내어 IP를 할당받고, DHCP 서버는 IP | WAN통신을 위한 게이트웨이(라우터의 IP) 주소 등 네트워크 구성에 필요한 정보들을 보내준다.

`Step 2` 주소 결정 프로토콜 ARP(Address Resolution Protocol)

- IP주소를 물리적인 MAC 주소에 매핑 시키는 역할.
- 로컬 네트워크의 기기들이 다른 기기와 통신하기 위해 MAC주소가 필요 -> 따라서 ARP 과정발생
-> ARP Request 브로드캐스트를 통해, 특정 IP를 찾고 해당 응답으로 MAC주소를 라우팅 테이블에 저장해 놓는다.

`Step 3` NAT(Network Address Translation)

- LAN들은 WAN에 접속하기 위해 NAT 과정을 거쳐야한다. (반드시 게이트웨이를 통과함)
-> 게이트웨이는 각 사설 IP들을 공인 IP로 바꿔주는 역할을 한다. -> 따라서 게이트웨이를 거쳐서 나가는 IP는 WAN상에서 모두 동일하다.
-> WAN으로부터 응답이 들어올때에는 게이트웨이에 저장되어 있는 사설 IP들의 정보를 기억하고 응답을 돌려준다.

=> Local Network 구성이 끝이났다, 이과정이 끝이나면 Local의 각 장비들은 네트워크에 연결된 상태 이다.

## 2단계 : 브라우저의 연결

`Step 4` 쿼리(Query)

- 사용자가 주소창에 검색을 하면 검색어를 검색하는 것 인지, 도메인 주소를 입력한것인지 구분하고
크롬의 경우 구글에서 만들었기 때문에 검색어의 경우 구글에 검색을, 도메인 주소의경우 다음 단계로 넘어간다.

`Step 5` 리다이렉트(Redirect)

- 사용자는 웹 브라우저나 웹 서버에 요청된 웹 페이지의 주소가 변경되거나, 다른 위치로 이동되었을 때 이를 알리고 사용자를 이동시키는 과정.
-> 사용자가 특정 URL을 입력했을때, 해당 URL이 다른 URL로 변경 되었을때 리다이렉트가 발생하고, 그렇지 않은경우 웹 서버에서 해당 페이지 내용을 반환 할것 이다.

`Step 6` 캐싱(Caching)

- 해당 요청의 캐시 여부를 파악한다.
-> 이미 요청된 값 -> 캐싱된 값 반환
-> 새로운 요청 -> 다음 단계로 넘어간다.
2종류의 캐싱 (브라우저 캐시, 공유 캐시)
브라우저 캐시 -> 애플리케이션 - 로컬스토리지와 같은 브라우저 속의 캐시
공유 캐시 -> 서버 앞단의 프록시 서버 [웹 서버의 엔진엑스 <-> node.js] 와 같은 구조에 존재

## 3단계 : 연결

`Step 7` DNS(Domain Name System)

- FQDN(Fuuly Qualified Domain Name)을 -> 실제 IP로 바꾸는 과정
-> FQDN은 www.naver.com 과같은 호스트와 도메인이 합쳐진 이름
www -> 호스트 이름 Third Level, Sub 도메인
naver -> Second Level 도메인
com -> Top Level 도메인
-> DNS는 역순으로 일어난다.

`Step 8` 캐싱(Caching)

- 이 과정에서도 캐싱이 일어난다.
dns 저장공간
chrome : net-internals/#dns
Os : ipconfig/displaydns

`Step 9` 라우팅 (Routing)

- 게이트웨이로 부터 나간 정보가 목적지까지 최적의 과정을 거쳐 찾아가게 된다.
- 라우팅 과정에서 TCP 연결이 구축된다. 이때 3웨이 핸드셰이크, SSL과같은 과정이 발생한다.
HTTP/2 까지는 TCP, HTTP/3 부터는 QUIC 연결
- 해당 과정이 끝이나게 되면, 응답받은 컨텐츠를 다운하게 되고, 해당 컨텐츠를 렌더링 함으로서 끝이난다