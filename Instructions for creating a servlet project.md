- Trong file `pom.xml` thêm thư viện:
```
<properties>
		<jsp.api.version>2.0</jsp.api.version>
		<jstl.version>1.2</jstl.version>
		<servlet.api.version>3.1.0</servlet.api.version>
		<sitemesh.version>2.4.2</sitemesh.version>
		<mysql.version>8.0.13</mysql.version>
		<weld.servlet.version>1.1.0.Final</weld.servlet.version>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
	</properties>

	<dependencies>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jsp-api</artifactId>
			<version>${jsp.api.version}</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>${servlet.api.version}</version>
		</dependency>
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
		</dependency>
		<dependency>
			<groupId>opensymphony</groupId>
			<artifactId>sitemesh</artifactId>
			<version>${sitemesh.version}</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.version}</version>
		</dependency>
		<dependency>
			<groupId>org.jboss.weld.servlet</groupId>
			<artifactId>weld-servlet</artifactId>
			<version>${weld.servlet.version}</version>
		</dependency>

		<!-- json -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-core</artifactId>
			<version>2.6.3</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.6.3</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-annotations</artifactId>
			<version>2.6.3</version>
		</dependency>

		<dependency>
			<groupId>org.codehaus.jackson</groupId>
			<artifactId>jackson-mapper-asl</artifactId>
			<version>1.9.13</version>
		</dependency>
		<dependency>
			<groupId>org.codehaus.jackson</groupId>
			<artifactId>jackson-core-asl</artifactId>
			<version>1.9.13</version>
		</dependency>
		<!--end json -->

		<dependency>
			<groupId>commons-beanutils</groupId>
			<artifactId>commons-beanutils</artifactId>
			<version>1.9.3</version>
		</dependency>

		<dependency>
			<groupId>commons-lang</groupId>
			<artifactId>commons-lang</artifactId>
			<version>2.6</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.6.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
```

- Trong thư mục `webapp` tạo 1 thư mục `WEB-INF ` và trong thư mục `WEB-INF` tạo 1 file `web.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xmlns="http://java.sun.com/xml/ns/javaee"
		 xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
		 version="3.0">
	<display-name>new jdbc sales manager</display-name>



	<session-config>
		<tracking-mode>COOKIE</tracking-mode>
	</session-config>
	
	
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>

</web-app>
```
- Trong thư mục `webapp` tạo file `index.jsp`:
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:redirect url="/trang-chu"/>
```

- Sử dụng sitemesh ta phải tạo trong `webapp` 1 thư mục `decorators`
- Trong thư mục WEB-INF ta thêm file `decorators.xml` :
```
<?xml version="1.0" encoding="UTF-8"?>

<decorators defaultdir="/decorators">
	<excludes>
		<pattern>/api*</pattern>
	</excludes>

	<decorator name="admin" page="admin.jsp">
		<pattern>/admin*</pattern>
	</decorator>

	<decorator name="web" page="web.jsp">
		<pattern>/*</pattern>
	</decorator>

	<decorator name="login" page="login.jsp">
		<pattern>/dang-nhap</pattern>
	</decorator>
</decorators>
```
- Muốn sử dụng đượng `sitemesh decorators` thì ta phải tạo thư mục `common` thêm file `taglib.jsp`:
```
%@ taglib prefix = "c" uri = "http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://www.opensymphony.com/sitemesh/decorator" prefix="dec"%>
```

- Trong thư mục `WEB-INF` tạo thư mục `beans.xml`:
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://java.sun.com/xml/ns/javaee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/beans_1_0.xsd">
</beans>
```
