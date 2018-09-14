---
layout: post
title:  "Spring의 parameter 전달 체계"
date:   2018-09-12 21:30:00 +0900
categories: jekyll update
---

# 스프링 프레임워크의 컨트롤러 파라미터
출처 - 토비의 스프링 3.1

## @PathVariable
URL에 { }로 들어가는 패스변수를 받음. 하나의 URI템플릿 안에 여러 개를 선언할 수도 있음.
일치하지 않는 타입의 값이 들어오면 'HTTP 400 - Bad Request'를 발생시킴.
```java
@RequestMapping("/hello/{id}")
public String view(@PathVariable("id") int id) {...}
```

## @RequestParam
단일 HTTP 요청 파라미터를 메소드 파라미터에 넣어주는 애노테이션.
```java
public String view(@RequestParam("id") int id) {...}
```
해당 파라미터가 반드시 있어야함. 만약 없다면 400-Bad Request를 받게 됨. 필수가 아니라 선택적으로 받을 수 있게 하려면
required 엘리먼트를 false로 설정해야함. 요청 파라미터가 없을 때 defaultValue를 지정할 수도 있음
```java
public String view(@RequestParam(value="id", required=false, defaultValue="50") int id) {...}
```
```java
public String view(@RequestParam("id") int id, 
                    @RequestParam("name") String name, 
                    @RequestParam("file") MultipartFile file) {...}
```
요청 파라미터 이름을 지정하지 않고 Map<String, String> 타입으로 선언하면 컨트롤러에서 모든 요청 파라미터를 담은 맵으로 받을 수 있음.
파라미터 이름, key 값은 맵의 key, 값은 value에 담겨 전달됨. 
```java
public String add(@RequestParam Map<String, String> params) {...}
```
자바 코드 컴파일 시 디버깅 정보를 모두 삭제하는 수준의 최적화를 하지 않았다면 이름도 생략 가능함. 단 컨트롤러가 요구하는 파라미터의 이름과 
요청 파라미터의 이름이 같아야 함.
```java
public String view(@RequestParam int id) {...}
```

## @CookieValue
HTTP 요청과 함께 전달된 쿠키 값을 받을 메서드 파라미터에 넣어줌. 쿠키의 이름을 지정해주면 됨.<br/>
@RequestParam과 마찬가지로 지정된 쿠키 값이 반드시 존재해야하나, `required`, `defaultValue`	엘리먼트를 사용할 수 있음.
```java
public String check(@CookieValue("auth") String auth) {...}
```


## @RequestHeader
요청 헤더정보를 메서드 파라미터에 넣어주는 애노테이션<br/>
@RequestParam과 마찬가지로 지정된 쿠키 값이 반드시 존재해야하나, required, defaultValue 엘리먼트를 사용할 수 있음.
```java
public void header(@RequestHeader("Host") String host, 
                    @RequestHeader("Keep-Alive") long keepAlive) {...}
```

## Map, Model, ModelMap
Model, ModelMap은 addAttribute() 메서드를 제공함. 일반적인 Map의 put()처럼 이름을 지정해서 값을 넣어줄 수 있음.
```java
@RequestMapping(...)
public void hello(ModelMap model) {
	User user = new User(1, "Spring");
	model.addAttribute(user); // -> model.addAttribute("user", user)와 동일함.
```
다른 애노테이션이 안 붙어있으면, 파라미터로 선언한 컨트롤러 메서드에 모델 정보를 담는데 사용할 수 있는 오브젝트가 전달됨. 
모델을 담을 맵은 메서드 내에서 직접 생성할 수도 있음. 하지만, 파라미터로 정의하면 핸들러 어댑터에서 미리 만들어서 제공해 줌.
그걸 사용하는 것이 편함. 자동 이름 생성 기능을 제공함.


## @ModelAttribute
요청 파라미터를 메서드 파라미터에서 1:1로 받으면 @RequestParam이고, 도메인 오브젝트나 DTO 프로퍼티에 요청 파라미터를 바인딩해서 한 번에 받으면 @ModelAttribute 라고 함.
하나의 오브젝트에 클라이언트 요청정보를 담아서 한 번에 전달되기 때문에 커맨드 패턴에서 말하는 커맨드 오브젝트라고 부르기도 함.<br><br>

URL의 쿼리스트링으로 들어오는 GET방식 HTTP 요청 정보를 @ModelAttribute가 붙은 파라미터 타입 오브젝트에 모두 담아서 전달함. 페이지 내의 폼 데이터를 받는 경우에도 @ModelAttribute를 사용함.
```java
@RequestMapping("/user/search")
public String search(@ModelAttribute UserSearch userSearch, Model model) {
    List<User> list = userService.search(userSearch);
    model.addAttribute("userList", list);
    ...
}
```
페이지 내의 폼 데이터를 받는 경우에도 @ModelAttribute를 사용할 수 있음. 
이 때는 보통 폼의 내용을 담을 수 있는 도메인 오브젝트나 DTO를 @ModelAttribute 파라미터로 사용함.
```java
@RequestMapping("/user/add", method=RequestMethod.POST)
public String add(@ModelAttribute User user) {
    userService.add(user);
    ...
}
```
@RequestParam, @ModelAttribute 두 가지 모두 생략 가능함. 스프링은 어떻게 annotation이 없는 파라미터를 보고 이 둘은 구분해내는가?<br>
-> 스프링은 String, int와 같은 단순 타입은 @RequestParam으로 보고, 그 외의 복잡한 오브젝트는 모두 @ModelAttribute가 생략된 것으로 간주함.
<br><br>
!! 스프링은 간단한 숫자나 문자로 전달된 요청 파라미터를 복잡한 오브젝트로 변환할 수 있기 때문에 단순 타입이 아니라고 해서 꼭 @ModelAttribute가 생략됬다고 볼 수 없음. 
그래서 @RequestParam, @ModelAttribute을 작성해주는 것을 권장함. 매우 단순한 커맨드 오브젝트나 파라미터라면 생략해도 나쁘진 않음.
<br><br>
@ModelAttribute는 컨트롤러가 리턴하는 모델에 파라미터로 전달한 오브젝트를 자동으로 추가해주는 기능을 가지고 있음.
이 때 모델 이름은 기본적으로 파라미터 타입의 이름임.(Class User => "user") 다른 이름을 쓰고 싶으면 지정해줄 수 있음.

## Errors, BindingResult
@ModelAttribute는 여러 Request Parameter를 넣어서 넘겨주는게 전부가 아님. 검증 작업이 추가적으로 진행됨. @RequestParam은 안됨.
- 변환 불가능한 타입의 Request Paremeter가 들어왔을 때
    * @RequestParam : 스프링의 기본 타입 변환 기능 이용해서 타입을 변환함.
        String이 가장 단순함. URL 쿼리 스트링, Form field는 멀티 타입이 아니면 문자열로 오기 때문임.
        int형 파라미터라면, String형 Request를 int형으로 타입 변환 시도함. 성공하면 int형 parameter에 전달됨. 실패하면 400-Bad Request 응답함.
    * @ModelAttribute : @ModelAttribute 오브젝트에 요청 프로퍼티 값을 넣다 타입 에러 발생하면?
        타입 변환에 실패해도 작업이 곗곡 진행됨. 단지, 타입 변환 중 발생한 예외가 BindException 타입의 오브젝트에 답겨서 컨트롤러로 전달될 뿐임.
        # 그럼 왜 바로 에러로 처리 하지 않음?
          -> 그 이유는 @ModelAttribute는 Request parameter가 model object의 property type과 일치하는지를 포함한 
            다양한 방식의 검증 기능을 수행하기 때문. @ModelAttribute 입장에서 parameter 타입 불일치는 검증 작업의
            한 결과일 뿐, 예상치 못한 예외상황이 아님. 별 검증 과정 없이 값을 무조건 넣으려고 시도하는 
            @RequestParam과 @ModelAttribute는 이런 차이가 있음.

버튼, 링크에 미리 할당된 정보가 아니라 사용자가 직접 입력하는 값은 다양한 오류가 있을 수 있어서 무조건 검증이 필요함.
검증 작업은 타입 확인, 필수 여부, 길이 제한, 포맷, 값의 허용 범위 등 다양한 검증 기준이 있을 수 있음.
검증 통과 못했다고 400에러 띄우면 안댐. 누가 회원 가입하다 하나 빼먹었다고 400에러 띄움? 진짜 황당한거임.
<br><br>
사용자 입력 값에 오류가 있으면 이 처리를 컨트롤러에 맡겨야됨. 
컨트롤러는 적절한 에러 페이지를 반환하거나, 친절하게 에러메시지를 보여주면서 사용자가 다시 폼을 수정할 기회를 줘야함.
그래서 @ModelAttribute를 쓸 땐 Errors 또는 BindingResult 타입 파라미터를 같이 써야함.
<br><br>
Errors나 BindingResult 파라미터를 함께 쓰지 않으면 Spring은 Request parameter 타입이나 값에 문제가 없도록 애플리케이션이 보장 빵빵하게 해준다고 생각함.
단지 파라미터 개수가 여러개라 커맨드 오브젝트 형태로 전달받는거라 생각한다는 거임. 그래서 이 때는 타입이 다르면 BIndingException 예외가 던져짐.
@RequestParam은 400 응답 상태 코드로 변환해주기라도 하지, 얘는 지저분한 에러 메시지를 띄워버림. 사용자 정보를 입력 받는 컨트롤러는 이렇게 하셈.
```java
@RequestMapping(value="add", method=RequestMethod.POST)
public String add(@ModelAttribute User user, BindingResult bindingResult) {...}
```

## @RequestBody
HTTP 요청의 본문(body) 부분이 그대로 전달됨. 일반적인 GET/POST 요청 파라미터면 쓸 일이 없음. 근데 XML, JSON 기반 메시지 쓸 땐 매우 유용함.

## @Value
빈의 값 주입해서 쓰던 애임. 컨트롤러에서도 쓸 수 있음. 끝.


## @Valid
JSR-303의 빈 검증기를 이용해서 객체를 검증하도록 지시하는 지시자임. 객체의 검증 방법을 지정하는데 쓰는 애노테이션임.
보통 @ModelAttribute와 함께 사용함.
```java
@RequestMapping(value="add", method=RequestMethod.POST)
public String add(@Valid @ModelAttribute User user, BindingResult bindingResult) {...}
```

하이버네이트에서 제공하는 Validator를 쓰면 이런 조건을 걸 수 있음.
```
@AssertFalse : false 값만 통과 가능
@AssertTrue : true 값만 통과 가능
@DecimalMax(value=) : 지정된 값 이하의 실수만 통과 가능
@DecimalMin(value=) : 지정된 값 이상의 실수만 통과 가능
@Digits(integer=,fraction=) : 대상 수가 지정된 정수와 소수 자리수보다 적을 경우 통과 가능
@Future : 대상 날짜가 현재보다 미래일 경우만 통과 가능
@Past : 대상 날짜가 현재보다 과거일 경우만 통과 가능
@Max(value) : 지정된 값보다 아래일 경우만 통과 가능
@Min(value) : 지정된 값보다 이상일 경우만 통과 가능
@NotNull : null 값이 아닐 경우만 통과 가능
@Null : null일 겨우만 통과 가능
@Pattern(regex=, flag=) : 해당 정규식을 만족할 경우만 통과 가능
@Size(min=, max=) : 문자열 또는 배열이 지정된 값 사이일 경우 통과 가능
```

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
