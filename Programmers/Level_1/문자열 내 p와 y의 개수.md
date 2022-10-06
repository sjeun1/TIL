### 문자열 내 p와 y의 개수

#### 문제 설명
대문자와 소문자가 섞여있는 문자열 s가 주어집니다. <br>
s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요.<br>
'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다.<br>
단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.<br>
예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.<br>
<br>

#### 제한 조건
문자열 s의 길이 : 50 이하의 자연수<br>
문자열 s는 알파벳으로만 이루어져 있습니다.<br>
<br>
#### 입출력 예<br>
s	answer<br>
"pPoooyY"	true<br>
"Pyy"	false<br>

#### 입출력 예 설명<br>
##### 입출력 예 #1<br>
'p'의 개수 2개, 'y'의 개수 2개로 같으므로 true를 return 합니다.<br>

##### 입출력 예 #2<br>
'p'의 개수 1개, 'y'의 개수 2개로 다르므로 false를 return 합니다.<br>

```
class Solution {
    boolean solution(String s) {
        boolean answer = true;
        String str = s.toUpperCase();
        int count = 0;
        
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == 'P') 
                count++;
            
            if (str.charAt(i) == 'Y')
                count--;
        }
        if(count != 0) answer = false;
        return answer;
    }
}
```

출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges
