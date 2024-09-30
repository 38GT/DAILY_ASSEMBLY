<h1 style="font-size: 48px;">Architecture: 안정적인 메세지 전송을 위한 설계</h1>



<h2>`Notification Logs 테이블`: 각 알림의 전송 상태를 기록</h2>

```sql
CREATE TABLE notification_logs (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    report_id UUID NOT NULL,
    sent_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('pending', 'success', 'failure') NOT NULL,
    error_message TEXT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (report_id) REFERENCES reports(id)
);
```

장점: 간단하고 직관적인 구조. 데이터베이스의 트랜잭션과 무결성을 활용할 수 있음.  
단점: 대량의 데이터가 축적될 경우 성능 저하 가능.
알림 전송과 기록을 동시에 처리해야 함으로 인해 복잡성 증가 가능.

<h2>`OutBox 패턴`: 메세지 발송을 위한 별도의 테이블을 두고 트랜잭션 내에서 메시지 발송 정보를 기록하는 방식</h2>

Outbox 패턴의 장점:

트랜잭션 일관성: 메시지 발송 정보가 비즈니스 트랜잭션과 함께 저장되어 일관성을 유지.
Decoupling: 메시지 발송 로직과 비즈니스 로직을 분리하여 관리.
신뢰성: 메시지 발송 실패 시 재시도 로직을 쉽게 구현할 수 있음.

```sql
CREATE TABLE outbox (
    id UUID PRIMARY KEY,
    aggregate_id UUID NOT NULL,
    event_type VARCHAR(255) NOT NULL,
    payload JSON NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    processed BOOLEAN DEFAULT FALSE
);
```

```typescript
// 예시: 새로운 알림을 생성하고 Outbox에 기록
await transactionalEntityManager.save(notification);
await transactionalEntityManager.save(outboxEntry);
```

Outbox 프로세서:

별도의 서비스가 주기적으로 Outbox 테이블을 조회하여 미처리된 메시지를 큐(예: Redis Streams, RabbitMQ 등)에 전송하고, 처리 완료 후 processed 필드를 업데이트.

Transactional Outbox 패턴을 구현하는 방식은 대표적으로 Polling Publisher 와 Transaction Log Tailing 이 있습니다. 두 방식의 가장 큰 차이점은 publish 할 메시지를 구성하는 데 있습니다.


<h2>`Event Sourcing`: Event Sourcing은 시스템의 상태 변화를 이벤트 형태로 저장하는 방식으로, 모든 변경 사항을 이벤트로 기록하여 재구성할 수 있습니다. 이는 복잡한 도메인 로직을 다루는 대규모 시스템에서 주로 사용</h2>

장점: 투명성: 모든 상태 변화를 명확히 기록. 유연성: 다양한 방식으로 데이터를 재구성하고 분석 가능.  
단점: 복잡성 증가: 구현과 관리가 복잡. 스토리지 요구량 증가: 모든 이벤트를 저장해야 하므로 스토리지 사용량 증가.