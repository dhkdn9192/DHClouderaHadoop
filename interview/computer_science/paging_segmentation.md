# 페이징과 세그먼테이션

## 가상 메모리
가상 메모리는 RAM을 관리하는 방법의 하나로, 각 프로그램에 실제 메모리 주소가 아닌 가상의 메모리 주소를 주는 방식을 말한다. 
실제 주기억장치보다 큰 메모리 영역을 제공하는 방법으로도 사용된다.

가상적으로 주어진 주소를 "가상 주소" 또는 "논리 주소"(logical address) 라고 하며, 
실제 메모리 상에서 유효한 주소를 "물리 주소"(physical address)라 한다. 

가상 주소 공간은 "메모리 관리 장치"(MMU)에 의해서 물리 주소로 변환된다. 
이 덕분에 프로그래머는 가상 주소 공간상에서 프로그램을 짜게 되어 프로그램이나 데이터가 주메모리상에 어떻게 존재하는지를 의식할 필요가 없어진다.

가상 메모리는 크게 나누어 세그먼트(segment) 방식과 페이징 방식의 2종류가 있다.
각각 세그먼트/페이지 매핑 테이블이 사용된다.


## 페이징 (Paging)
페이징 기법은 컴퓨터가 메인 메모리에서 사용하기 위해 물리 메모리로부터 데이터를 저장하고 검색하는 메모리 관리 기법이다.
가상 메모리를 모두 <b>같은 크기의 블록</b>으로 나누고 이를 페이지(page)라 한다.
물리 메모리는 페이지와 같은 단위인 프레임 단위로 나누어 페이지와 동일하게 매핑되도록 한다.
- <b>프레임(Frame)</b>: 물리 메모리를 일정된 한 크기로 나눈 블록
- <b>페이지(Page)</b>: 가상 메모리를 일정된 한 크기로 나눈 블록

페이징은 <b>내부 단편화(Internal Fragmentation)</b>를 유발할 수 있다.
페이지의 크기를 줄이면 내부 단편화를 줄일 수 있지만 page mapping이 많아지므로 오버헤드가 커진다.


## 세그먼테이션 (Segmentation)
페이징에서처럼 논리 메모리와 물리 메모리를 같은 크기의 블록이 아닌, 서로 다른 크기의 논리적 단위인 세그먼트(Segment)로 분할한다.

세그먼트들의 크기가 서로 다르기 때문에 메모리를 페이징 기법처럼 미리 분할해 둘 수 없고,
메모리에 적재될 때 빈 공간을 찾아 할당하는 사용자 관점의 가상메모리 관리 기법이다.

세그먼트는 <b>외부 단편화(External Fragmentation)</b>를 유발할 수 있다.


## 단편화
- <b>내부 단편화(Internal Fragmentation)</b>: 실제 필요한 크기보다 메모리를 더 할당하여 프로세스에 필요한 메모리 공간이 낭비되는 상태

![internal_fragmentation](img/internal_fragmentation.jpeg)

- <b>외부 단편화(External Fragmentation)</b>: 주메모리의 여유공간이 충분하지만 작은 조각들로 나누어져 있어 할당을 할 수 없는 상태

![external_fragmentation](img/external_fragmentation.jpeg)


## Reference
- https://ko.wikipedia.org/wiki/%EA%B0%80%EC%83%81_%EB%A9%94%EB%AA%A8%EB%A6%AC
- https://sycho-lego.tistory.com/10
- https://jupiny.com/2017/03/28/paging-segmentation/
- https://m.blog.naver.com/rbdi3222/220623825770
