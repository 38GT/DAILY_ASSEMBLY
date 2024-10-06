<h1 style="font-size: 48px;">GitLab Cron </h1>

1. cron 라이브러리가 nest에 내장되어 있다.
2. @Cron 애노테이션을 활용해서 반복할 매서드를 지정할 수 있고 따로 onmoduleinit을 implements할 필요 없다.
3. 코드 예시
```typescript
  @Cron('*/10 * 6-21 * * 1-5', {
    timeZone: 'Asia/Seoul',
})
private async polling(){...}
```
4. 위 Cron의 첫번째 매개변수의 값들은 각 초, 분, 시, 일, 달, 주 를 의미함
5. 해석하자면 10초 마다 매일 6시-21시사이에 월(1)-금(5)에 가동된다.
