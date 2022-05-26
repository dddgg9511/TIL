Test 코드를 작성하는 도중 다음과 같이 static 함수를 `mocking` 해야 하는 상황이 있었다.

```java
public boolean verifyEmail(String rawCode, String code){
    return passwordEncoder.matches(rawCode, code);
}

public String generateVerifyCode(String email){
    checkDuplicate(email);
    String code = VerifyCodeUtil.generateToken();
    sendEmailVerifyMail(email, code);
    return passwordEncoder.encode(code);
}
```

이메일 인증을 하기 위해 인증 코드를 발급하고 발급된 인증코드를 메일로 받아 입력 후 인증을 하는 로직이다.

하지만 테스트에서는 이메일로 발송된 암호화 처리가 되지 않는 인증 코드를 확인할 수 가 없었다.

`generateVerifyCode()` 내부에서 static 함수를 호출해 랜덤으로 인증코드를 생성한 뒤 암호화된 인증코드를 반환하기 때문이었다.

그래서 `VerifyCodeUtil.generateToken()` 을 `mocking` 하려 했지만

```java
VerifyCodeUtil verifyCodeUtil = mock(VerifyCodeUtil.class);
```

이런 식으로 `mock` 객체를 생성해도 함수 내에서는 객체를 사용하는 게 아니라 `static` 함수를 호출 하기 때문에 의미 없었다. 

그래서 static 함수를 `mocking` 하는 방법을 검색해 다음과 같이 테스트 코드를  작성했다.

```java
@Nested
@DisplayName("이메일 인증은")
class Describe_verify_email{
    @Nested
    @DisplayName("인증번호가 주어지면")
    class Context_with_verify_code{
        String rawCode;
        String code;

        @BeforeEach
        void setUp(){
            rawCode = "AB1234";
            mockStatic(VerifyCodeUtil.class);
            when(VerifyCodeUtil.generateToken()).thenReturn(rawCode);

            code = userService.generateVerifyCode("ddgg9511@naver.com");
        }

        @Test
        @DisplayName("true를 반환한다.")
        void it_return_true(){
            boolean result = userService.verifyEmail(rawCode, code);
            assertThat(result).isTrue();
        }
    }
}
```

하지만 다음과 같은 에러가 나왔다.

```
The used MockMaker SubclassByteBuddyMockMaker does not support the creation of static mocks

Mockito's inline mock maker supports static mocks based on the Instrumentation API.
You can simply enable this mock mode, by placing the 'mockito-inline' artifact where you are currently using 'mockito-core'.
Note that Mockito's inline mock maker is not supported on Android.
org.mockito.exceptions.base.MockitoException: 
The used MockMaker SubclassByteBuddyMockMaker does not support the creation of static mocks
```

메시지를 보니 현재 사용중인 `mockito-core`를 `mockito-inline` 로 변경하면 에러를 해결할 수 있을 것 같다.

```java
testImplementation 'org.mockito:mockito-inline:4.5.1'
```

다음과 같이 의존성을 추가한 후 다시 실행해 보았다.

test는 원하는 결과로 성공했다.

하지만 검색한 블로그를 보니 static method를 mocking하게 되면 다른 테스트까지도 영향을 미치기 때문에

```java
MockedStatic<VerifyCodeUtil> verifyCodeStaticMock = mockStatic(VerifyCodeUtil.class);
verifyCodeStaticMock.close()
```

다음과 같이 사용 후에 해제해야 한다고 한다.

### 최종 코드

```java
@Nested
@DisplayName("이메일 인증은")
class Describe_verify_email{
    @Nested
    @DisplayName("인증번호가 주어지면")
    class Context_with_verify_code{
        String rawCode;
        String code;
        MockedStatic<VerifyCodeUtil> verifyCodeStaticMock;
        
        @BeforeEach
        void setUp(){
            rawCode = "AB1234";
            verifyCodeStaticMock = mockStatic(VerifyCodeUtil.class);
            when(VerifyCodeUtil.generateToken()).thenReturn(rawCode);

            code = userService.generateVerifyCode("ddgg9511@naver.com");
        }
        
        @AfterEach
        void cleanUp(){
            verifyCodeStaticMock.close();
        }
        
        @Test
        @DisplayName("true를 반환한다.")
        void it_return_true(){
            boolean result = userService.verifyEmail(rawCode, code);
            assertThat(result).isTrue();
        }
    }
}
```
