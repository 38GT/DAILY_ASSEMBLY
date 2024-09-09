<h1 style="font-size: 48px;">Problem29</h1>


# Javascript code
```javascript
function solution(orders, course) {
    
    const coursesInOrders = Array.from({length: orders.length}, () => new Map());
    
    for(let i = 0; i < orders.length; i++){
        for(let j = 0; j < orders.length; j++){
            if(j === i) continue;
            
            // 두 오더에서 중복되는 메뉴 고르기
            let setMenu = ""
            Array.from(orders[i]).forEach(menu => {
                if(orders[j].includes(menu)) setMenu += menu
            })
            
            const courseStr = setMenu.split("").sort().join("");
            
            // setMenu에서 가능한 모든 서브코스 계산하기 -> allCourseStr
            const allCourseStr = [];
            function findAllCourses(str){
                const result = [];
                function backtrack(start,path){
                    result.push(path.join(""));
                    for(let i = start; i < str.length; i ++){
                        path.push(str[i]);
                        backtrack(i + 1,path);
                        path.pop();
                    }
                }
                backtrack(0,[]);
                return result;
            }
            
            allCourseStr.push(...findAllCourses(courseStr))
            
            // 집계하기: coursesInOrders에 allCourseStr 값들 넣기            
            allCourseStr.forEach(courseStr => {
                if(!coursesInOrders[i].get(courseStr)) coursesInOrders[i].set(courseStr,1);
                if(!coursesInOrders[j].get(courseStr)) coursesInOrders[j].set(courseStr,1);
            })
        }
    }
    
    // 코스별 주문 횟수 계산하기
    const courseWithMultiple = new Map ();
    
    for (let courses of coursesInOrders){
        for(let [course] of courses){
            if(!courseWithMultiple.get(course)){
                courseWithMultiple.set(course, 1);
            }else{
                courseWithMultiple.set(course, courseWithMultiple.get(course) + 1);
            }
        }
    }
    
    let answer = []
    
    for(let courseSize of course){
        const sorted = [...courseWithMultiple].filter(([course,multiple])=> course.length === courseSize)
        .sort(([course1, multiple1],[course2,multiple2])=>multiple2 - multiple1);

        answer.push(...sorted.filter(([course,multiple])=>{
            return multiple === sorted[0][1]
        }).map(([course,multiple])=>course));
    }

    return answer.sort()
}

```

# Java code

```java

import java.util.*;
import java.util.stream.Collectors;

public class Solution {
    private static class Course{
        public final String course;
        public final int occurrences;

        public Course(String course, int occurrences) {
            this.course = course;
            this.occurrences = occurrences;
        }
    }

    private void getCourses(char nextMenu, Set<String> selectedMenus, List<Set<String>> orderList, Map<Integer, List<Course>> courses){
        int occurrences = (int) orderList.stream()
                .filter(order -> order.containsAll(selectedMenus))
                .count();
        if(occurrences < 2) return;
        
        int size = selectedMenus.size();
        
        if(courses.containsKey(size)){
            List<Course> courseList = courses.get(size);
            Course course = new Course(selectedMenus.stream()
                    .sorted()
                    .collect(Collectors.joining("")),
                    occurrences);
            Course original = courseList.get(0);
            if(original.occurrences < occurrences){
                courseList.clear();
                courseList.add(course);
            }else if (original.occurrences == occurrences){
                courseList.add(course);
            }
        }
        
        if(size >= 10) return;
        for(char menuChar = nextMenu; menuChar <= 'Z'; menuChar++){
            String menu = String.valueOf(menuChar);
            selectedMenus.add(menu);
            getCourses((char) (menuChar + 1), selectedMenus, orderList, courses);
            selectedMenus.remove(menu);
        }
    }
    
    public String[] solution(String[] orders, int[] course){
        List<Set<String>> orderList = Arrays.stream(orders)
                .map(String::chars)
                .map(charStream -> charStream
                        .mapToObj(menu -> String.valueOf((char) menu))
                        .collect(Collectors.toSet()))
                .collect(Collectors.toList());
        Map<Integer, List<Course>> courses = new HashMap<>();
        for (int length : course){
            List<Course> list = new ArrayList<>();
            list.add(new Course("",0));
            courses.put(length, list);
        }
        getCourses('A',new HashSet<>(), orderList, courses);
        
        return courses.values().stream()
                .filter(list -> list.get(0).occurrences > 0)
                .flatMap(List::stream)
                .map(c -> c.course)
                .sorted()
                .toArray(String[]::new);
    }
}
```