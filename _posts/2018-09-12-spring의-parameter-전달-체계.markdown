---
layout: post
title:  "Spring의 parameter 전달 체계"
date:   2018-09-12 21:30:00 +0900
categories: jekyll update
---
<link rel="stylesheet" type="text/css" href="../lib/highlight/styles/default.css">
<script src="../lib/highlight/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>

<div class="container">
    <div class="jumbotron">
        <h1>스프링 프레임워크의 파라미터 전달</h1>
    </div>
    <article>
        <h1>@PathVariable</h1>
        <p>URL에 { }로 들어가는 패스변수를 받음.</p>
        <pre><code>
                    @RequestMapping("/hello/{id}")
                    public String view(@RequestParam("id") int id) {...}
            </code></pre>
        <br>
        <h1>@RequestParam</h1>
        <p>단일 HTTP 요청 파라미터를 메소드 파라미터에 넣어주는 애노테이션</p>
        <pre><code>public String view(@RequestParam("id") int id) {...}
            </code></pre>
        <pre><code>public String view(@RequestParam("id") int id, 
                    @RequestParam("name") String name, 
                    @RequestParam("file") MultipartFile file) {...}
            </code></pre>
        <p>파라미터 이름을 지정하지 않고 Map&lt;String, String&gt; 타입으로 선언</p>
        <pre><code>public String add(@RequestParam Map&lt;String, String&gt; params) {...}</code></pre>
        <h1>@CookieValue</h1>
        <pre><code>public String check(@CookieValue("auth") String auth) {...}</code></pre>
        <p>@RequestParam과 마찬가지로 지정된 쿠키 값이 반드시 존재해야하나, <code>required</code>, <code>defaultValue</code> 엘리먼트를 사용할 수 있음.</p>
        <h1>@RequestHeader</h1>
        <p>요청 헤더정보를 메서드 파라미터에 넣어주는 애노테이션</p>
        <pre><code>public void header(@RequestHeader("Host") String host, @RequestHeader("Keep-Alive") long keepAlive) {...}</code></pre>
        <h1>Map, Model, ModelMap</h1>
        <p>파라미터로 선언한 컨트롤러 메서드에 모델 정보를 담는데 사용할 수 있는 오브젝트가 전달됨. 자동 이름 생성 기능 제공</p>
        <h1>@ModelAttribute</h1>
        <p>URL의 쿼리스트링으로 들어오는 GET방식 HTTP 요청 정보를 @ModelAttribute가 붙은 파라미터 타입 오브젝트에 모두 담아서 전달</p>
        <p>페이지 내의 폼 데이터를 받는 경우에도 @ModelAttribute를 사용함.</p>
        <pre><code>@RequestMapping("/user/add", method=RequestMethod.POST)
                public String add(@ModelAttribute User user) {
                    userService.add(user);
                    ...
                }</code></pre>
        <p>@RequestParam, @ModelAttribute 두 가지 모두 생략 가능함. 스프링은 어떻게 annotation이 없는 파라미터를 보고 이 둘은 구분해내는가?<br>
                -> 스프링은 String, int와 같은 단순 타입은 @RequestParam으로 보고, 그 외의 복잡한 오브젝트는 모두 @ModelAttribute가 생략된 것으로 간주함.</p>
        <h1>Errors, BindingResult</h1>
        <pre><code>@RequestMapping(value="add", method=RequestMethod.POST)
                public String add(@ModelAttribute User user, BindingResult bindingResult) {...}</code></pre>
        <h1>@RequestBody</h1>
        <p>HTTP 요청의 본문(body) 부분이 그대로 전달됨. XML, JSON 기반 메시지 쓸 땐 매우 유용</p>
        <pre><code>public void header(@RequestHeader("Host") String host, @RequestHeader("Keep-Alive") long keepAlive) {...}</code></pre>
        <h1>@Valid</h1>
        <p>JSR-303의 빈 검증기를 이용해서 객체를 검증하도록 지시하는 지시자</p>
    </article>
</div>


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
