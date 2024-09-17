<h1 style="font-size: 48px;">Bill Fold</h1>

https://school.programmers.co.kr/learn/courses/30/lessons/340199#

```java
    class Solution {
    public int solution(int[] wallet, int[] bill) {
        int answer = 0;
        int[] state;
        state = bill;

        while(!check(wallet,state)){
            answer += 1;
            state = fold(state);
        }
        return answer;
    }

    static boolean check (int[] wallet, int[] bill){
        if(bill[0] <= wallet[0] && bill[1] <= wallet[1]){
            return true;
        }else if(rotate(bill)[0] <= wallet[0] && rotate(bill)[1] <= wallet[1]){
            return true;
        }
        return false;
    }

    static int[] rotate (int[] bill){
        int[] rotated_bill = { bill[1], bill[0] };
        return rotated_bill;
    }

    static int[] fold (int[] bill){
        if(bill[0] > bill[1]){
            return new int[] { bill[0]/2, bill[1] };
        }else{
            return new int[] { bill[0], bill[1]/2 };
        }
    }
}
```