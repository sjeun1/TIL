### JadenCase 문자열 만들기

#### 문제 설명
JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다.<br>
단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)<br>
문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.<br>
<br>

#### 제한 조건
s는 길이 1 이상 200 이하인 문자열입니다.<br>
s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.<br>
숫자는 단어의 첫 문자로만 나옵니다.<br>
숫자로만 이루어진 단어는 없습니다.<br>
공백문자가 연속해서 나올 수 있습니다.<br>
<br>
#### 입출력 예<br>
s	                            answer<br>
"3people unFollowed me"   	"3people Unfollowed Me"<br>
"for the last week"	    "For The Last Week"<br>

```
import java.util.*;

class Solution {
    public String solution(String s) {
        return convertJadenCase(s);
    }
    
    public static String convertJadenCase (String str) {
        str = str.toLowerCase();
        String[] strArr = str.split("");
        String[] array = new String[str.length()];
        boolean flag = false;
        
        for(int i =0; i<strArr.length; i++) {
            if(i == 0) flag = true;
            
            String text = (flag) ? strArr[i].toUpperCase() : strArr[i];
            if(flag) flag = false;
            array[i] = text;
            
            if(" ".equals(strArr[i].toString())) 
                flag = true;
        }
        StringBuilder sb = new StringBuilder();
        for (String s : array)
		sb.append(s);
        
	return sb.toString();
     }
}
```

출처: 프로그래머스 코딩 테스트 연습, https://school.programmers.co.kr/learn/challenges
