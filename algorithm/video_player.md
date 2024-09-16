<h1 style="font-size: 48px;">Video Player</h1>


```java
import java.time.*;
import java.time.format.DateTimeFormatter;

class Solution {
    public String solution(String video_len, String pos, String op_start, String op_end, String[] commands) {
        // HH:mm:ss 포맷으로 맞추기
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("HH:mm:ss");
        
        // LocalTime 객체로 변환
        LocalTime pos_time = parseCustomTime(pos);
        LocalTime op_start_time = parseCustomTime(op_start);
        LocalTime op_end_time = parseCustomTime(op_end);
        LocalTime video_len_time = parseCustomTime(video_len);
        LocalTime start_time = LocalTime.of(0, 0); // 00:00:00

        // commands를 단계적으로 처리하기
        for (String command : commands) {
            // 1. 먼저 오프닝 구간에 있는지 확인하고, 있으면 오프닝 끝으로 이동
            if (check_op(pos_time, op_start_time, op_end_time)) {
                pos_time = op_end_time;
            }

            // 2. command에 따라 prev 또는 next로 재생 위치 이동
            pos_time = operate(pos_time, video_len_time, start_time, command);

            // 3. 명령 실행 후 다시 오프닝 구간에 있는지 확인하고, 있으면 오프닝 끝으로 이동
            if (check_op(pos_time, op_start_time, op_end_time)) {
                pos_time = op_end_time;
            }
        }

        // 다시 "mm:ss" 형식으로 변환해서 반환
        return formatToMMSS(pos_time);
    }

    // Custom parser to handle minutes and seconds correctly
    static LocalTime parseCustomTime(String time) {
        // 시간이 mm:ss로 들어오는 경우를 처리
        String[] parts = time.split(":");
        int minutes = Integer.parseInt(parts[0]);
        int seconds = (parts.length > 1) ? Integer.parseInt(parts[1]) : 0;

        return LocalTime.of(0, minutes, seconds); // 시간을 0으로 설정하고 분, 초만 설정
    }

    static boolean check_op(LocalTime pos_time, LocalTime op_start_time, LocalTime op_end_time) {
        // pos_time이 오프닝 구간에 포함되는지 확인
        return !pos_time.isBefore(op_start_time) && !pos_time.isAfter(op_end_time);
    }

    static LocalTime operate(LocalTime pos_time, LocalTime video_len_time, LocalTime start_time, String command) {
        // 문자열 비교 시 equals() 사용
        if (command.equals("next")) {
            return (pos_time.plusSeconds(10).isAfter(video_len_time)) ? video_len_time : pos_time.plusSeconds(10);
        }

        if (command.equals("prev")) {
            return (pos_time.minusSeconds(10).isBefore(start_time)) ? start_time : pos_time.minusSeconds(10);
        }

        return pos_time;
    }

    // "mm:ss" 형식으로 변환하는 메서드
    static String formatToMMSS(LocalTime time) {
        int minutes = time.getMinute();
        int seconds = time.getSecond();

        // 두 자리 형식으로 변환하여 반환
        return String.format("%02d:%02d", minutes, seconds);
    }
}

```

