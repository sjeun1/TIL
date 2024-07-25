## @transactional

string 에서 transaction 을 사용할 때 xml 파일을 사용한 설정과 어노테이션을 사용한 설정이 있는데, <br>
해당 페이지는 어노테이션 방식을 사용한 것으로 @Transactional을 클래스 단위 혹은 메서드 단위에 선언해준다. <br>
클래스에 선언하면 해당 클래스에 속하는 메서드에 공통적으로 적용되고, 메서드에 선언하면 해당 메서드에만 적용된다. <br>
예를 들어 아래 코드를 보면 createUserInfo 에 적용되어서 해당 메서드에서 내부 createUser, createAccount 를 호출하고 한 메서드에서 오류가 나면 모두 실패가 뜨는 것을 확인 할 수 있다. <br>
<br>
```
import org.springframework.stereotype.Service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserService {
    private final UserRepository userRepository;
    private final accountRepository;

    @Transactional
    public void createUserInfo(String username, String accountType) {
        createUser(username);
        createAccount(username, accountType);
    }

    public void createUser(String username) {
        User user = new User();
        user.setUsername(username);
        userRepository.save(user);
    }

    public void createAccount(String username, String accountType) {
        Account account = new Account();
        account.setUsername(username);
        account.setAccountType(accountType);
        accountRepository.save(account);
    }
}

```

만약에 아래와 같이 내부 메서드에도 트랜잭션이 걸려있다면 트랜잭션 전파(propagation) 설정에 따라 동작 방식이 달라진다. <br>
기본 전파 속성인 Propagation.REQUIRED가 사용되면 createUserInfo 메소드가 호출될 때 트랜잭션이 시작되는건 동일하고
createUserInfo 메소드 내부에서 createUser 메소드를 호출하면, createUser 메소드의 트랜잭션은 이미 시작된 트랜잭션을 그대로 사용합니다.
<br><br><br>

### 트랜잭션 전파(propagation)
트랜잭션 전파 방식은 특정 시나리오에서 트랜잭션의 동작을 제어하기 위해 사용된다. 이를 적절하게 사용하면 트랜잭션의 일관성과 데이터의 무결성을 보장 할 수 있다. <br><br>
- REQUIRED : 기본 전파방식. 현재 트랜잭션이 존재하면 이를 사용하고, 그렇지 않으면 새로운 트랜잭션을 시작한다. <br>
- REQUIRES_NEW : 항상 새로운 트랜잭션을 시작한다.기존 트랜잭션이 있으면 이를 일시 중단하고 새로운 트랜잭션을 시작하며, 새로운 트랜잭션이 완료되면 기존 트랜잭션을 재개한다. <br>
- SUPPORTS : 현재 트랜잭션이 존재하면 이를 사용하고, 그렇지 않으면 트랜잭션 없이 실행한다.
- NOT_SUPPORTED : 항상 트랜잭션 없이 실행한다. 현재 트랜잭션이 존재하면 이를 일시 중단한다.
- MANDATORY : 트랜잭션이 반드시 필요한 메소드에서 사용하는 것으로, 현재 트랜잭션이 존재하고 존재하지 않으면 예외를 발생시킵니다.
- NEVER : 트랜잭션이 없어야 한다. 현재 트랜잭션이 존재하면 예외를 발생시킨다. <br>
- NESTED : 현재 트랜잭션이 존재하면 중첩 트랜잭션을 시작한다. 중첩 트랜잭션은 부모 트랜잭션에 종속되며, 부모 트랜잭션이 커밋되면 중첩 트랜잭션도 커밋된다. 현재 트랜잭션이 없으면 REQUIRED와 동일하게 동작한다. <br>
<br>
<br><br>

전파방식을 적용하여 새로운 예제 코드로 트랜잭션을 보면, 
```
import org.springframework.stereotype.Service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserService {
    private final UserRepository userRepository;
    private final AccountRepository accountRepository;

    @Autowired
    public UserService(UserRepository userRepository, AccountRepository accountRepository) {
        this.userRepository = userRepository;
        this.accountRepository = accountRepository;
    }

    @Transactional
    public void createUserInfo(String username, String accountType) {
        createUser(username);
        createAccount(username, accountType);
    }

    @Transactional(propagation = Propagation.NESTED)
    public void createUser(String username) {
        User user = new User();
        user.setUsername(username);
        userRepository.save(user);
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void createAccount(String username, String accountType) {
        Account account = new Account();
        account.setUsername(username);
        account.setAccountType(accountType);
        accountRepository.save(account);
    }
}
```
<br>
해당 코드의 동작방식은 다음과 같다. <br>
1. createUserInfo 메소드 호출 <br>
createUserInfo 메소드가 호출되면서 트랜잭션이 시작된다. <br>
트랜잭션이 시작된 상태에서 createUser와 createAccount 메소드가 호출된다. <br>
2. createUser 메소드 호출<br>
createUser 메소드는 Propagation.NESTED 속성을 사용된다. <br>
createUserInfo 메소드에서 시작된 트랜잭션이 부모 트랜잭션이 되어 중첩 트랜잭션이 시작된다. <br>
중첩 트랜잭션은 부모 트랜잭션의 커밋이나 롤백에 종속된다. <br>
3. createAccount 메소드 호출<br>
createAccount 메소드는 Propagation.REQUIRES_NEW 속성을 사용된다. <br>
createUserInfo 메소드에서 시작된 기존 트랜잭션을 일시 중단하고, 새로운 트랜잭션을 시작된다. <br>
createAccount 메소드가 종료되면, 기존 트랜잭션이 재개된다. <br>
<br><br>
즉, 이 코드에서 createUserInfo 메소드가 호출될 때 트랜잭션이 시작되고, createUser 메소드는 중첩 트랜잭션을 사용하여 부모 트랜잭션에 종속적으로 동작한다.<br>
반면 createAccount 메소드는 항상 새로운 트랜잭션을 시작하고, 기존 트랜잭션과 독립적으로 동작한다.<br>
만약에 중간에 예외가 발생한다면 만약 createUser 메소드에서 예외가 발생하면, 중첩 트랜잭션이 롤백된다. 부모 트랜잭션이 커밋될 때 중첩 트랜잭션의 롤백도 함께 반영된다.<br>
만약 createAccount 메소드에서 예외가 발생하면, 새로운 트랜잭션이 롤백되며 createAccount 메소드의 작업만 롤백됩니다. 기존 트랜잭션은 영향을 받지 않는다.<br>
<br>


어노테이션을 제거한 방식으로 사용하고자 한다면 TransactionTemplate 을 사용하여도 된다. <br>
