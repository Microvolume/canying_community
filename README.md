## canying_community

### bug修改
#### 1. 分页点击页码无法跳转 修改index.html即可
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

### 版本信息
* SpringBoot 2.1.6.RELEASE
* jdk        1.8.0_271
* MySQL      5.7.31
* Redis      3.2.1
* Kafka      2.13
* Elasticsearch 6.4.3
