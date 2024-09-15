<h1 style="font-size: 48px;">Binary Search</h1>

1. 이진 탐색을 사용하기 위해선 데이터가 정렬이 되어 있어야 한다.
2. while, 재귀 방식을 이용해서 구현 가능
3. List에선 Collection.binarySearch 메서드 제공, Array에선 Arrays.binarySearch 메서드 제공
4. start, end 포인터 연습 필요
```java
package java_algorithm;
import java.util.*;

public class Binary_search {
    public int listSearch(List<Integer> arr, int target) {
        Collections.sort(arr);
        return Collections.binarySearch(arr, target);
    }

    public int listSearchWithWhile(List<Integer> arr, int target) {

        int start = 0;
        int end = arr.size() - 1;

        while (start <= end) {
            System.out.println("start:" + start + " end:" + end);
            int mid = (start + end) / 2;
            System.out.println("mid:" + mid);
            if (arr.get(mid) == target) {
                return mid;
            }else if (arr.get(mid) < target) {
                start = mid + 1;
                System.out.println(start);
            }else{
                end = mid - 1;
            }
        }
        return -1;
    }


//    public int listSearchWithRecursive(List<Integer> arr, int target) {}

    public Integer arraySearch(int[] arr, int target) {
        Arrays.sort(arr);
        return Arrays.binarySearch(arr, target);
    }
}

```


```javascript
const N = 10;
const arr = new Array(N).fill(0).map((item, index)=> index);
console.log(arr);

let start = 0;
let end = arr.length;
let target = 4

while (start < end) {
    let mid = Math.floor((start + end) / 2);
    if (arr[mid] < target) {
        start = mid + 1;
    } else {
        end = mid;
    }
}

console.log(start, end, target);

```