---
layout: post
title:  "Spring의 parameter 전달 체계"
date:   2018-09-12 21:30:00 +0900
categories: jekyll update
---

# 스프링 프레임워크의 파라미터 전달
출처 - 토비의 스프링 3.1

## @PathVariable
URL에 { }로 들어가는 패스변수를 받음.</p>
```java
@RequestMapping("/hello/{id}")
public String view(@RequestParam("id") int id) {...}
```

## @RequestParam
단일 HTTP 요청 파라미터를 메소드 파라미터에 넣어주는 애노테이션
```java
public String view(@RequestParam("id") int id) {...}
```
```java
public String view(@RequestParam("id") int id, 
                    @RequestParam("name") String name, 
                    @RequestParam("file") MultipartFile file) {...}
```
파라미터 이름을 지정하지 않고 Map<String, String> 타입으로 선언
```java
public String add(@RequestParam Map&lt;String, String&gt; params) {...}
```

## @CookieValue
```java
public String check(@CookieValue("auth") String auth) {...}
```
@RequestParam과 마찬가지로 지정된 쿠키 값이 반드시 존재해야하나, `required`, `defaultValue`	엘리먼트를 사용할 수 있음.

## @RequestHeader
요청 헤더정보를 메서드 파라미터에 넣어주는 애노테이션
```java
public void header(@RequestHeader("Host") String host, @RequestHeader("Keep-Alive") long keepAlive) {...}
```

## Map, Model, ModelMap
파라미터로 선언한 컨트롤러 메서드에 모델 정보를 담는데 사용할 수 있는 오브젝트가 전달됨. 자동 이름 생성 기능 제공

## @ModelAttribute
URL의 쿼리스트링으로 들어오는 GET방식 HTTP 요청 정보를 @ModelAttribute가 붙은 파라미터 타입 오브젝트에 모두 담아서 전달. 페이지 내의 폼 데이터를 받는 경우에도 @ModelAttribute를 사용함.
```java
@RequestMapping("/user/add", method=RequestMethod.POST)
public String add(@ModelAttribute User user) {
    userService.add(user);
    ...
}
```
@RequestParam, @ModelAttribute 두 가지 모두 생략 가능함. 스프링은 어떻게 annotation이 없는 파라미터를 보고 이 둘은 구분해내는가?<br>
-> 스프링은 String, int와 같은 단순 타입은 @RequestParam으로 보고, 그 외의 복잡한 오브젝트는 모두 @ModelAttribute가 생략된 것으로 간주함.

## Errors, BindingResult
```java
@RequestMapping(value="add", method=RequestMethod.POST)
public String add(@ModelAttribute User user, BindingResult bindingResult) {...}
```

## @RequestBody
HTTP 요청의 본문(body) 부분이 그대로 전달됨. XML, JSON 기반 메시지 쓸 땐 매우 유용
```java
public void header(@RequestHeader("Host") String host, @RequestHeader("Keep-Alive") long keepAlive) {...}
```

## @Valid
JSR-303의 빈 검증기를 이용해서 객체를 검증하도록 지시하는 지시자


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
