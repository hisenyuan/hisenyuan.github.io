---
title: SpringMVC把jsp改为Thymeleaf模版引擎
keywords: [SpringMVC把jsp改为Thymeleaf模版引擎,springmvc,jsp,Thymeleaf]
date: 2017/8/1 11:12
tags: [java,spring]
categories: java
---
Thymeleaf简介
---
前面的例子我们使用的视图技术主要是JSP。JSP的优点是它是Java EE容器的一部分，几乎所有java EE服务器都支持JSP。缺点就是它在视图表现方面的功能很少，假如我们想迭代一个数组之类的，只能使用<% %>来包括Java语句进行。虽然有标准标签库（JSTL）的补足，但是使用仍然不太方便。另外JSP只能在Java EE容器中使用，如果我们希望渲染电子邮件之类的，JSP就无能为力了。

Java生态圈广泛，自然有很多视图框架，除了JSP之外，还有Freemarker、Velocity、Thymeleaf等很多框架。Thymeleaf的优点是它是基于HTML的，即使视图没有渲染成功，也是一个标准的HTML页面。因此它的可读性很不错，也可以作为设计原型来使用。而且它是完全独立于java ee容器的，意味着我们可以在任何需要渲染HTML的地方使用Thymeleaf。

Thymeleaf也提供了spring的支持，我们可以非常方便的在Spring配置文件中声明Thymeleaf Beans，然后用它们渲染视图。

改造 - 由jsp到Thymeleaf
---
1. 引入依赖
```
    <!--thymeleaf模版 spring4.x-->
    <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring4</artifactId>
      <version>3.0.5.RELEASE</version>
    </dependency>
```
2. 配置ViewResolver(在spring的xml文件里)
<!--more-->
```
<!-- 配置ViewResolver 使用：thymeleaf 模版引擎-->
  <bean id="templateResolver"
    class="org.thymeleaf.spring4.templateresolver.SpringResourceTemplateResolver">
    <property name="prefix" value="/WEB-INF/templates/"/>
    <property name="suffix" value=".html"/>
    <property name="templateMode" value="HTML5"/>
    <!-- 缓存-->
    <property name="cacheable" value="false"/>
  </bean>
  <bean id="templateEngine"
    class="org.thymeleaf.spring4.SpringTemplateEngine">
    <property name="templateResolver" ref="templateResolver"/>
  </bean>
  <bean class="org.thymeleaf.spring4.view.ThymeleafViewResolver">
    <property name="templateEngine" ref="templateEngine"/>
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
  </bean>
```
3. 接下来就可以直接使用了，跟之前的jsp没有什么不同
```
  @RequestMapping(value = "/listpageplug/{start}", method = RequestMethod.GET)
  private String listPagePlug(@PathVariable("start") String start, Model model) {
    PageHelper.startPage(Integer.valueOf(start), 20);
    List<Book> readingList = bookService.getListPlug();
    model.addAttribute("books", readingList);
    return "readingList";
  }
```

到这里就改造完了，接下来就是Thymeleaf的各种用法了

这里举一个循环遍历的例子,后台返回了books对象集合
```
        <!--判断是否为空-->
        <tbody th:unless="${#lists.isEmpty(books)}">
          <!--循环-->
          <tr th:each="book : ${books}">
            <td th:text="${book.bookId}"></td>
            <td th:text="${book.name}"></td>
            <td th:text="${book.number}"></td>
            <td>
              <a href="<%=appPath%>/book/detail/${book.bookId}">详情</a> |
              <a href="<%=appPath%>/book/del/${book.bookId}">删除</a>
            </td>
          </tr>
        </tbody>
```
参考
---
1. <a href="http://www.thymeleaf.org/doc/tutorials/2.1/thymeleafspring.html#integrating-thymeleaf-with-spring" target="_blank">thymeleaf官方指南</a>
2. <a href="http://www.tianmaying.com/tutorial/using-thymeleaf" target="_blank">新一代Java模板引擎Thymeleaf</a>
3. <a href="http://www.webinno.cn/blog/article/view/131" target="_blank">Thymeleaf基本知识</a>
4. <a href="http://v8en.com/news/list/47/0" target="_blank">thymeleaf总结文章</a>
5. <a href="http://www.cnblogs.com/lazio10000/p/5603955.html" target="_blank">Thymeleaf 模板的使用</a>
6. <a href="http://www.blogjava.net/bjwulin/archive/2013/02/07/395234.html" target="_blank">thymeleaf 学习笔记</a>