## @Controller 와 @RestController 의 차이점을 설명해주세요.
전통적인 Spring MVC의 컨트롤러인 @Controller와 Restuful 웹서비스의 컨트롤러인 @RestController의 주요한 차이점은 **HTTP Response Body가 생성되는 방식**입니다.

## @Controller
전통적인 Spring MVC의 컨트롤러인 @Controller는 주로 **View를 반환하기 위해 사용**합니다. 아래와 같은 과정을 통해 Spring MVC Container는 Client의 요청으로부터 View를 반환합니다.
<img width="1160" height="577" alt="image" src="https://github.com/user-attachments/assets/3fe92066-9b3e-47b7-9da2-6cc8b9d02362" />


Controller가 반환환 뷰의 이름으로부터 View를 렌더링하기 위해서는 **ViewResolver**가 사용되며, ViewResolver 설정에 맞게 View를 찾아 렌더링합니다.


## @RestController
주로 **RESTful 웹 서비스 API를 정의할 때 사용**됩니다. 

@RestController는 **@Controller에 @ResponseBody가 추가**된 것으로, 당연하게도 RestController의 주용도는 Json 형태로 객체 데이터를 반환하는 것입니다. (자동으로 JSON 또는 XML 형식으로 변환되어 HTTP 응답 본문에 포함되는 것)
<img width="1160" height="544" alt="image" src="https://github.com/user-attachments/assets/0215a1b7-aa63-4e05-9d93-09906a7f1a01" />

### 유의점
이러한 경우의 문제는 **클라이언트가 예상하는 HttpStatus를 설정해줄 수 없다**는 것입니다. 

예를 들어 어떤 객체의 생성 요청이라면 201 CREATED를 기대할 것이지만 객체를 그대로 반환하면 HttpStatus를 설정해줄 수 없습니다. 그래서 객체를 상황에 맞는 ResponseEntity로 감싸서 반환해주어야 합니다.
```java
@RestController
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping(value = "/users")
    public User findUser(@RequestParam String userName){
        return userService.findUser(userName); // User 객체를 그대로 반환, 클라이언트가 예상하는 HttpStatus를 설정해줄 수 없음
    }

    @GetMapping(value = "/users")
    public ResponseEntity<User> findUserWithResponseEntity(@RequestParam String userName){
        return ResponseEntity.ok(userService.findUser(userName)); // ResponseEntity로 감싸서 HttpStatus를 설정하여 반환
    }
}
```

<br>

## 요약
- @Controller와 @RestController의 주요한 차이점은 **HTTP Response Body가 생성되는 방식**
- **@Controller**의 역할은 Model 객체를 만들어 데이터를 담고 View를 반환하는 것
- **@RestController**는 단순히 객체만을 반환하고 객체 데이터는 JSON 또는 XML 형식으로 HTTP 응답에 담아 전송
- 즉, @RestController는 @Controller와 @ResponseBody의 동작을 하나로 결합한 컨트롤러


---
### 출처
- [[Spring] @Controller와 @RestController 차이](https://mangkyu.tistory.com/49)
- [[Spring] @Controller와 @RestController의 차이점 알아보기](https://dev-coco.tistory.com/84)
