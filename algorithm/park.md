<h1 style="font-size: 48px;">Park</h1>

https://school.programmers.co.kr/learn/courses/30/lessons/340198

1. Row 마다 빈 공간들의 [시작점, 길이]를 모아놓은 배열을 만들어서 문제를 해결해보려고 한다.
2. Java에서 생성자를 만들 때 따로 파라미터를 받지 않으면 new키워드로 인스턴스를 만들때 모든 정보를 입력할 필요가 없고 추후에 추가해줘도 된다.
3. prefix int[][]의 사이즈는 주어진 2차원 배열 사이즈의 + 1 씩 한 값으로. (첫 줄 계산을 위해)


```java
import java.util.Arrays;

class Solution {
    public static int solution(int[] mats, String[][] park) {
        //1. 누적합 만들기
        
        int n = park.length;
        int m = park[0].length;
        
        int[][] prefix = new int[n+1][m+1];
        for(int i = 1; i <= n; i ++){
            for(int j = 1; j <= m; j ++){
            prefix[i][j] = prefix[i-1][j] + prefix[i][j-1] - prefix[i-1][j-1] + (park[i-1][j-1].equals("-1") ? 1 : 0);
            }
        }
        
        //2. mats 내림차순 정렬 + inverse 포문 = 오름차순 정렬 -> 코드 절약
        Arrays.sort(mats);
        for(int k = mats.length -1; k >= 0; k --){
            int size = mats[k];
            for(int i = 0; i <  n - size + 1; i ++){
                for(int j = 0; j  < m - size + 1; j ++){
                    if(check(i,j,size,prefix)) return size;
                }
            }
        }
        return -1;
        //3. mats 큰 것 부터 자리가 나는지 체크하고 나면 해당 매트 사이즈 리턴 그리고 모두 없을시 마지막에 -1 리턴
        //4. 체크로직: 현재 위치에서 오른쪽 아래로 정사각형 영역에서 -1 개수를 세서 size * 2 과 같은지 비교
        //5. pesudo logic
        /*
            
            mats.sort();
            for(int size of mats){
                for(int[][] [i,j] of park){
                    if(check(size,[i,j],prefix)) return size;
                }

            }
            -> 시간 복잡도 정렬 ~ 20 + mats.length=20 * park.length=50 
            return -1;
        */
    }
    
    static boolean check(int i, int j, int size, int[][] prefix){
        int sum = prefix[i + size][j + size] - prefix[i][j + size] - prefix[i + size][j] + prefix[i][j];
        return (size * size == sum); 
    }
}

```