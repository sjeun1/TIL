

![image](https://user-images.githubusercontent.com/62210870/178767705-f8ddb4c4-08c9-4c7b-a9b6-9d82f6526191.png)
<br><br><br><br><br>


## 클린 코드(Claen Code)

#### 목차
- [깨끗한 코드와 나쁜 코드](#깨끗한-코드와-나쁜-코드)
- [의미있는 코드](#의미있는-코드)

<br><br><br><br>


## 깨끗한 코드와 나쁜 코드

#### 나쁜 코드란?
나쁜 코드는 개발 속도를 저하시키고 고칠 때마다 엉뚱한 곳에서 문제가 생긴다.<br>
나쁜 코드가 쌓일 수록 속도저하 등으로 팀 생산성이 떨어진다. 생산성을 증가시키기 위해서는 프로젝트에 인력을 추가로 투입한다. <br>
하지만 투입이 얼마 안 된 새 인력이라면 시스템을 깊게 알지 못하고 압박에 시달린다. 그로 인해 생산성이 더욱 떨어진다.<br>
이와 같은 상황이 되지 않고 기한을 맞추면서 빨리 가는 방법은 언제나 코드를 깨끗하게 유지하는 것이다.<br>

그렇다면 깨끗한 코드란 무엇일까? <br>
책에서 나온 전문가들은 이렇게 설명하고 있다.<br>


#### 깨끗한 코드란?
-&nbsp;논리가 간단하며 의존성을 최대한 줄이며 각 의존성을 명확하게 정의한다.<br>
-&nbsp;중복이 없다. <br>
-&nbsp;단순하고 직접적이다. 가독성이 좋고 설계자의 의도를 숨기지 않는다.<br>
-&nbsp;타인이 읽어도 읽기 쉽고 고치기 쉽다. 의미있는 이름이 붙는다.<br>
-&nbsp;간단한 추상화 고려한다.<br>
-&nbsp;특정목적을 달성하는 방법은 여러가지가 아니라 하나만 제공한다.<br>
-&nbsp;단위 테스트 케이스와 인수 테스트 케이스가 존재한다. 테스트 시 모든 테스트를 통과한다.<br>
-&nbsp;주의깊게 짠다. 다시 고칠 필요가 없도록, 모든 상황을 고려하여서.<br>
<br>
<br>
<br>
<br>
## 의미있는 코드

##### &nbsp;의도를 분명히 한다 
-&nbsp;변수명이 경과시간이라면 int d; 보다는 int elapsedTimeInDays; 로 명확하게 정의하는 것이 낫다. 
<br><br>

##### &nbsp;잘못된 정보를 피한다 
-&nbsp;그룹으로 묶을 때 실제 List 가 아니라면 accountList 라는 식으로 명명하지 않는다.
-&nbsp;accoutGroup, Accounts, bunchOfAccounts 등이 더 옳은 표현이다.
-&nbsp;헷갈릴만한 유사한 이름을 사용하지 않는다.
<br><br>

##### &nbsp;의미있게 구분한다.
-&nbsp;숫자를 붙인 이름은 사용하지 않는다. (ex.  a1, a2 ..)
-&nbsp;불용어를 추가하는 방식은 적절하지 못한다. (ex.  Product, Products, ProductInfo, ProductData ..)
<br><br>

##### &nbsp;발음하기쉬운 이름을 사용한다.
-&nbsp;genymdhms -> generationTimestamp 
<br><br>

##### &nbsp;검색하기 쉬운 이름을 사용한다.
<br><br>

##### &nbsp;인코딩을 피한다
-&nbsp;헝가리식 표기법(변수이름에 타입인코딩할 필요 X), 멤버 변수 접두어(X), 인터페이스/구현클래스 사용 시 인코딩이 필요한 경우라면 구현 클래스에 인코딩.
<br><br>

##### &nbsp;클래스이름 : 명사나 명사구가 적합 (customer, payment)

##### &nbsp;메서드이름 : 동사나 동사구가 적합 (insertCustomer, postPayment) 접급자/변경자/조건자는 get, set, is 등이 붙는다

##### &nbsp;한개념에 한단어를 사용한다

##### &nbsp;해법영역에서 가져온 이름을 사용한다 
-&nbsp;전산용어, 알고리즘 이름 등 기술개념에는 기술이름이 가장 적합(ex. visitor패턴 사용시 AccountVisitor, 멀티프로세싱때 jobQueue 메소드)
<br><br>

##### &nbsp;문제영역에서 가져온 이름을 사용한다
-&nbsp;적절한 개발 용어가 없으면 문제영역에서 가져와서 분야 전문가가 파악 가능.
<br><br>

##### &nbsp;의미 있는 맥락을 추가한다.
-&nbsp;firstName, street, city, state, zipcode 를 보면 주소라는 사실을 알지만 state 변수 하나만 사용한다면 state 가 주소 일부라는 사실을 빠르게 파악하기가 힘들다.
-&nbsp;같은 변수라도 addr 접두어를 추가하여  addrFirstName 라고 쓰면 맥락이 분명해진다.
<br><br>

##### &nbsp;불필요한 맥락을 없앤다.
-&nbsp;의미가 분명할 경우에 한해서는 짧은 이름이 긴 이름보다 좋다. 
&nbsp;accountAdress, customerAddress 는 Address 클래스 인스턴스로는 좋은 이름이지만 클래스 이름으로는 적합하지 않다.
-&nbsp;postalAddress, MAC, URI 등 분명한 이름으로 붙이는게 좋다.
<br><br><br><br>







