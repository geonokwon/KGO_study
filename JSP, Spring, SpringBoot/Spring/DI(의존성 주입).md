##### DI(Dependency Injection) - 의존관계 주입
-> 스프링에서 객체생성해서 전달(주입)
- root-context.xml
```xml title:root-context.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
	<!-- MemberController 객체생성-->
	<bean id="memberController" class="com.itwillbs.controller.MemberController">
		<!-- setMethod 호출  name="set메서드이름" ref="전달할 객체생성값"-->
		<constructor-arg ref="memberService"></constructor-arg>
	</bean>
	
	<!-- MemberService 부모 인터페이스 =  MemberServiceImpl 객체생성 -->
	<bean id="memberService" class="com.itwillbs.service.MemberServiceImpl2">
		<constructor-arg ref="memberDAO"></constructor-arg>
	</bean>
	
	<!-- MemberDAO 부모 인터페이스 =   MemberDAOImpl 객체생성 -->
	<bean id="memberDAO" class="com.itwillbs.dao.MemberDAOImpl2"></bean>
</beans>

```
- 생성자 생성한다
- 생성자를 생성할때 파라미터값을 넣어서 가져오는 방법

```java title:Java
private MemberService memberService;
@Autowired
	public MemberController(MemberService memberService) {
		this.memberService = memberService;
	}
----------------------------------------------
private MemberDAO memberDAO;
@Autowired
	public MemberServiceImpl (MemberDAO memberDAO) {
		this.memberDAO = memberDAO;
	}
```
