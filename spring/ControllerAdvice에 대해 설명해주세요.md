## ControllerAdvice에 대해 설명해주세요.
**`@ControllerAdvice`** 는 Spring Framework에서 예외 처리, 바인딩, 모델 객체의 전역 적용 등 **컨트롤러 전반에 걸친 공통 관심사** 를 처리할 수 있게 해주는 어노테이션입니다.

### 1. 개념 및 특징
@ControllerAdvice는 모든(혹은 지정한) 컨트롤러에 대해 전역적으로 적용되는 기능을 제공하는 어노테이션입니다.
- @ControllerAdvice가 선언된 클래스에 `@ExceptionHandler`, `@InitBinder`, `@ModelAttribute`를 등록하면 예외 처리, 바인딩 등을 한 곳에서 처리할 수 있어, 코드의 중복을 줄이고 유지보수성을 높일 수 있습니다. 
- @ControllerAdvice는 내부에 `@Component`가 포함되어 있어 컴포넌트 스캔 과정에서 빈으로 등록됩니다. 
- @RestControllerAdvice는 내부에 `@ResponseBody`를 포함하여 `@ExceptionHandler`와 함께 사용될 때 예외 응답을 Json 형태로 내려준다는 특징이 있습니다.

### 2. 주요 목적
1. 예외 처리의 일원화
2. 바인딩 설정의 일원화
3. 공통 모델 속성 추가

<br>

## 주요 활용 예시
**1. 전역 예외 처리** : 컨트롤러에서 발생하는 예외를 한 곳에서 처리할 수 있습니다.
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAllExceptions(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                             .body("에러 발생: " + ex.getMessage());
    }
}
```

**2. 특정 예외만 처리** : 특정 예외에 대해서만 별도로 처리할 수 있습니다.
```java
@ControllerAdvice
public class CustomExceptionHandler {

    @ExceptionHandler(NullPointerException.class)
    public String handleNullPointer(NullPointerException ex, Model model) {
        model.addAttribute("error", "NullPointerException 발생!");
        return "errorPage";
    }
}
```

**3. 공통 모델 속성 추가** : 모든 컨트롤러의 모델에 공통 데이터를 추가할 수 있습니다.
```java
@ControllerAdvice
public class GlobalModelAttribute {

    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("serviceName", "Jin's Service");
        model.addAttribute("currentYear", LocalDate.now().getYear());
        model.addAttribute("copyright", "© 2025 Jin");
    }
}
```
  - 활용 예시: 여러 컨트롤러에서 반복적으로 필요한 데이터 (예: 로그인 사용자 정보, 서비스명, 공지사항 등)을 모든 요청에 자동으로 추가
  - 위 코드를 적용하면, 모든 컨트롤러의 뷰에서 serviceName, currentYear, copyright를 사용할 수 있음

```java
  @ModelAttribute
  public void addUserInfo(Model model, @AuthenticationPrincipal UserDetails user) {
      if (user != null) {
          model.addAttribute("username", user.getUsername());
      }
  }
```
  - 로그인한 사용자의 이름, 권한 등 정보를 모든 뷰에서 활용 가능

**4. 바인딩 설정** : 폼 데이터 바인딩 시 공통적으로 적용할 설정을 지정할 수 있습니다.
```java
@ControllerAdvice
public class GlobalBindingInitializer {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        // 전역 바인딩 설정
        binder.registerCustomEditor(Date.class, new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), false));
    }
}
```
  - 활용 예시: 날짜 포맷, 커스텀 에디터 등

### 다양한 활용 시나리오
1. **API 서버** :
  REST API에서 예외 발생 시 일관된 JSON 응답을 반환하도록 처리
2. **다국어 지원** :
  공통 모델 속성에 언어 정보, 메시지 번역 결과를 추가
3. **로깅** :
  예외 발생 시 로그를 남기고, 필요시 알림 전송

<br>

## 정리
- @ControllerAdvice는 **컨트롤러 전역에 적용되는 공통 관심사** 를 처리하는 강력한 도구입니다.
- **예외 처리, 모델 속성 추가, 바인딩 설정** 등 다양한 용도로 활용할 수 있습니다.
- 코드의 **중복을 줄이고, 유지보수성을 높여줍니다**.
