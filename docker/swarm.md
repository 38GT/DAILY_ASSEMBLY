<h1 style="font-size: 48px;">Docker Swarm</h1>

1. 마스터 노드와 워커 노드로 구분됨
2. 마스터 노드는 1대로 쓰거나 3개 이상으로 구성해서 사용한다고 함
3. factory, web, front, bot, redis, 마스터 노드로 구성되면 4-8GB 정도의 메모리가 요구될 것으로 보임
4. 마스터 노드는 워커노드의 역할도 할 수 있으므로 하나의 ec2인스턴스 안에서 마스터노드 1개로 여러 컨테이너들을 관리하면 된다.
5. docker-compose.yml 파일로 스택 만듬

