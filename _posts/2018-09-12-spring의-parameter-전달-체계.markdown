---
layout: post
title:  "Spring의 parameter 전달 체계"
date:   2018-09-12 21:30:00 +0900
categories: jekyll update
---
<link rel="stylesheet" href="../lib/highlight/styles/default.css">
<script src="../lib/highlight/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>

<div class="container">
    <div class="jumbotron">
        <h1>스프링 프레임워크의 파라미터 전달</h1>
    </div>
    <article>
        <h1>@PathVariable</h1>
        <p>URL에 { }로 들어가는 패스변수를 받음.</p>
        <pre>
            <code class="html">
                @RequestMapping("/hello/{id}")
                public String view(@RequestParam("id") int id) {...}
            </code>
        </pre>
    </article>

    <article>
        <h1>@RequestParam</h1>
        <p>단일 HTTP 요청 파라미터를 메소드 파라미터에 넣어주는 애노테이션</p>
        <pre>public String view(@RequestParam("id") int id) {...}</pre>
        <pre>public String view(@RequestParam("id") int id, @RequestParam("name") String name, @RequestParam("file") MultipartFile file) {...}</pre>
    </article>
    
</div>


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
