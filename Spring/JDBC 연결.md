# JDBC 연결
```xml title:root-context.xml
<!-- MemberDAO 부모 인터페이스 =   MemberDAOImpl 객체생성 -->
	<bean id="memberDAO" class="com.itwillbs.dao.MemberDAOImpl">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

<!-- DB연결 객체 생성 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul"></property>
		<property name="username" value="root"></property>
		<property name="password" value="1234"></property>
	</bean>
```

---