<h1 style="font-size: 48px;">Rank Search</h1>

1. 이진 탐색 포인터 두기 연습 필요 
2. 마지막에 정렬 다시 되돌리기 작업 안해줬음.
3. 이진 탐색 mid = (start + end)/2, target 비교
4. 투포인터 알고리즘 vs 이진 탐색 비교해서 이해해보기
```javascript
function solution(info, query) {
    
    const candidates = new Array(query.length).fill(0);
    //0. info, query 비교하기 쉽게 자료형 정의하기
    // [lang, pos, jors, food, score] -> 이런 배열로 정의하기
    //1. info, query cote score 기준 오름차순 정렬
    const data_info = info.map(string => string.split(" "))
        .sort(([lang1, pos1,jors1, food1, score1],[lang2, pos2,jors2, food2, score2])=> score1 - score2);
    const data_query = query.map((string => string.split(" ").filter(string => string !== 'and')))
        .sort(([lang1, pos1,jors1, food1, score1],[lang2, pos2,jors2, food2, score2])=> score1 - score2);
    
    
    //2. info의 score를 가지고 query score 기준으로 2진 탐색 시작 -> 발견한 후 그 이후로 조건 체크 시작
    
    for(let [lang, pos,jors, food, score] of data_info){
        let end = data_query.length - 1;
        let pointer = Math.floor(data_query.length/2);
        
        while(score !== data_query[end][4]){
            if(score > data_query[pointer][4]){
                pointer += Math.floor((data_query.length - pointer)/2);
            }else if (score < data_query[pointer][4]){
                end = pointer - 1
                pointer = Math.floor(pointer/2);
            }else{
                end = pointer;
            }
        }
        
        // console.log("data_query[pointer]: ", data_query[pointer][4], "score: ", score)
        for (let i = pointer; i < data_query.length ; i ++){
            //3. 맞는 경우 각 query 를 만족하는 배열 값에 +1 해주기 (따라서 query별로 만족하는 인원 수 배열을 정의해야함. -> candidates)
            let [lang_con, pos_con, jors_con, food_con] = data_query[i];
            if(lang_con === '-') lang_con = lang
            if(pos_con === '-') pos_con = pos
            if(jors_con === '-') jors_con = jors
            if(food_con === '-') food_con = food
            if(lang === lang_con &&  pos === pos_con && jors === jors_con && food === food_con) candidates[i] = candidates[i] + 1 
        }
    }
    
    return candidates;
}

```