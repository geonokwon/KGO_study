##### Ioc(Inversion of Control) - 제어의 역전
-> 스프링에서 set메서드를 호출한다!
- 자바 객체 생성 하던것을 -> 스프링으로 객체 생성 하기
- root-context.xml 파일
```xml title:root-context.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
	
	<!-- MemberController 객체생성-->
	<bean id="memberController" class="com.itwillbs.controller.MemberController"></bean>
	
	<!-- MemberService 부모 인터페이스 =  MemberServiceImpl 객체생성 -->
	<bean id="memberService" class="com.itwillbs.service.MemberServiceImpl"></bean>
	
	<!-- MemberDAO 부모 인터페이스 =   MemberDAOImpl 객체생성 -->
	<bean id="memberDAO" class="com.itwillbs.dao.MemberDAOImpl"></bean>
	
</beans>

```
- 인터페이스 형의 자식 인터페이스를 지정하는 방법은 class 에 자식 클래스 명명 후 id에서 부모 인터페이스 클래스를 지정하면 된다!

- xml 파일에서 객체 생성후 set 메서드를 이용해서 객체 보낸다
```xml
<!-- MemberController 객체생성-->
	<bean id="memberController" class="com.itwillbs.controller.MemberController">
		<!-- setMethod 호출  name="set메서드이름" ref="전달할 객체생성값"-->
		<property name="memberService" ref="memberService"></property>
	</bean>
	
	<!-- MemberService 부모 인터페이스 =  MemberServiceImpl 객체생성 -->
	<bean id="memberService" class="com.itwillbs.service.MemberServiceImpl2">
		<property name="memberDAO" ref="memberDAO"></property>
	</bean>
	
	<!-- MemberDAO 부모 인터페이스 =   MemberDAOImpl 객체생성 -->
	<bean id="memberDAO" class="com.itwillbs.dao.MemberDAOImpl2"></bean>
```
- 객체 생성 하는 클래스 명만 수정하면 알아서 참조하고있는 값들이 바뀐다
```java title:Java
private MemberService memberService;
	//2) setMethod
	@Autowired
	public void setMemberService(MemberService memberService) {
		this.memberService = memberService;
	}
-----------------------------------------------
private MemberDAO memberDAO;
	@Autowired
	public void setMemberDAO(MemberDAO memberDAO) {
		this.memberDAO = memberDAO;
	}
```
- 각 java 파일에 set 메서드를 만들어주고  받아서 스프링 xml에서 받아서 처리한다!
