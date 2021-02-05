## canying_community

### bug修改

#### 1. 引入的bootstrup和jQuery失效 -> (1)更换cdn (2)下载到本地 引入本地文件
(1). 更换cdn 
* bootstrap.min.css
~~~html
<link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet">
~~~
* bootstrap.js
~~~html
<script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.3.1/js/bootstrap.js"></script>
~~~
* jquery.slim.js
~~~html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.slim.js" integrity="sha256-fNXJFIlca05BIO2Y5zh1xrShK3ME+/lYZ0j+ChxX2DA=" crossorigin="anonymous"></script> 
~~~

#### 2. 建议所有dao包下所有Mapper加上 @Mapper @Repository 这两个注解

#### 3. 分页点击页码无法跳转 -> 修改index.html即可
```java 
<nav class="mt-5" th:if="${page.rows>0}" th:fragment="pagination">
    <ul class="pagination justify-content-center">
	<li class="page-item">
	    <a class="page-link" th:href="@{${page.path}(current=1)}">首页</a>
	</li>
	<li th:class="|page-item ${page.current==1?'disabled':''}|">
	    <a class="page-link" th:href="@{${page.path}(current=${page.current-1})}">上一页</a>
	</li>
	<li th:class="|page-item ${i==page.current?'active':''}|"th:each="i:${#numbers.sequence(page.from,page.to)}">
	    <a class="page-link" th:href="@{${page.path}(current=${i})}" th:text="${i}">1</a>
	</li>
        <li th:class="|page-item ${page.current==page.total?'disabled':''}|">
	    <a class="page-link" th:href="@{${page.path}(current=${page.current+1})}">下一页</a>
        </li>
	<li class="page-item">
	    <a class="page-link" th:href="@{${page.path}(current=${page.total})}">末页</a>
	</li>
    </ul>
</nav>
```
#### 4. 添加拦截器后无法访问静态资源 -> 修改 WebMvcConfig.java 
~~~java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Autowired
    private LoginTicketInterceptor loginTicketInterceptor;

    @Autowired
    private LoginRequiredInterceptor loginRequiredInterceptor;

    @Autowired
    private MessageInterceptor messageInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginTicketInterceptor)
                .excludePathPatterns("/css/*","/js/*","/img/*");

        registry.addInterceptor(loginRequiredInterceptor)
                .excludePathPatterns("/css/*","/js/*","/img/*");

        registry.addInterceptor(messageInterceptor)
                .excludePathPatterns("/css/*","/js/*","/img/*");
    }
}
~~~



### 版本信息
* SpringBoot 2.1.6.RELEASE
* jdk        1.8.0_271
* MySQL      5.7.31
* Redis      3.2.1
* Kafka      2.13
* Elasticsearch 6.4.3
