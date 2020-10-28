## JSP页面乱码处理

### 1.web.xml中添加乱码处理过滤器。

~~~xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
~~~



### 2.IDEA乱码处理，添加虚拟机启动参数：-Dfile.encoding=UTF-8，如下图：

![image-20201029002029352](assets/JSP页面乱码处理/image-20201029002029352.png)





