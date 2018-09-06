### 使用命令创建java测试Servlet

    需要通过以下命令安装javac编译环境: yum install java-devel

```shell
# 创建web应用存放java类目录
mkdir /usr/libexec/tomcat9/webapps/ROOT/WEB-INF/classes 

chown tomcat. /usr/libexec/tomcat9/webapps/ROOT/WEB-INF/classes 
# 进入web的classes目录
cd /usr/libexec/tomcat9/webapps/ROOT/WEB-INF/classes 

# 创建java Servlet文件
vi daytime.java
 ```

 ```java

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class DefaultServlet extends HttpServlet {
	
	private static final long serialVersionUID = 1L;
    
    public DefaultServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```

``` shell
 javac -classpath /usr/libexec/tomcat9/lib/servlet-api.jar daytime.java 
 vi /usr/libexec/tomcat9/webapps/ROOT/WEB-INF/web.xml
 ```

```xml
# add follows between <web-app> - </web-app>
  <servlet>
     <servlet-name>daytime</servlet-name>
     <servlet-class>daytime</servlet-class>
  </servlet>
  <servlet-mapping>
     <servlet-name>daytime</servlet-name>
     <url-pattern>/daytime</url-pattern>
  </servlet-mapping>
  ```