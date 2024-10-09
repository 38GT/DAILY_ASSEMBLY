<h1 style="font-size: 48px;">Set </h1>

1. Redis에선 set 자료구조를 지원한다.
2. 명령어들의 이름은 s + something 형태이다. 

<h3>SADD key member [member ...]</h3>  
    : Set에 하나 이상의 member를 추가합니다. 중복된 멤버는 추가되지 않습니다.
    : 예: SADD myset "apple" "banana" "orange"

<h3>SCARD key</h3> 
    : Set의 멤버 수를 반환합니다. 
    : 예: SCARD myset

<h3>SMEMBERS key</h3> 
    : Set의 모든 멤버를 반환합니다.
    : 예: SMEMBERS myset

SISMEMBER key member 
    : Set에 member가 존재하는지 확인합니다. 존재하면 1, 존재하지 않으면 0을 반환합니다.
    : 예: SISMEMBER myset "apple"

SREM key member [member ...]
    : Set에서 하나 이상의 member를 제거합니다.
    : 예: SREM myset "apple"

SPOP key [count]

    : Set에서 랜덤으로 count개의 멤버를 제거하고 반환합니다. count를 지정하지 않으면 하나의 멤버만 제거됩니다.
    : 예: SPOP myset 1

SRANDMEMBER key [count]
    : Set에서 랜덤으로 count개의 멤버를 반환합니다. count를 지정하지 않으면 하나의 멤버만 반환하며, Set에서 제거되지 않습니다.
    : 예: SRANDMEMBER myset 1

SMOVE source destination member
    : source Set에서 destination Set으로 member를 이동시킵니다.
    : 예: SMOVE myset otherset "apple"

SDIFF key [key ...]
    : 여러 Set의 차집합을 반환합니다. 첫 번째 Set에서 나머지 Set에 있는 멤버를 제외한 값을 반환합니다.
    : 예: SDIFF myset otherset

SDIFFSTORE destination key [key ...]
    : 차집합을 계산하여 결과를 destination Set에 저장합니다.
    : 예: SDIFFSTORE resultset myset otherset

SINTER key [key ...]

여러 Set의 교집합을 반환합니다. 모든 Set에 공통으로 존재하는 멤버를 반환합니다.
예: SINTER myset otherset
SINTERSTORE destination key [key ...]

교집합을 계산하여 결과를 destination Set에 저장합니다.
예: SINTERSTORE resultset myset otherset

SUNION key [key ...]
    : 여러 Set의 합집합을 반환합니다. 모든 Set에 존재하는 멤버를 하나의 Set으로 반환합니다.
    : 예: SUNION myset otherset

SUNIONSTORE destination key [key ...]
합집합을 계산하여 결과를 destination Set에 저장합니다.
예: SUNIONSTORE resultset myset otherset

SSCAN key cursor [MATCH pattern] [COUNT count]

Set의 멤버를 반복적으로 조회할 수 있습니다. 큰 Set에서 효율적으로 데이터를 탐색할 때 유용합니다.
예: SSCAN myset 0 MATCH "a*" COUNT 10