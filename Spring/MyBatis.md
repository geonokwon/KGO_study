# MyBatis
- DB 작업 위해서 mybatis 설치해줘야함
- Maven repository 에서 MyBatis, MyBatis Spring 추가
- MySQL 추가 Spring jdbc 추가
- MemberMapper.xml 생성
	- src/main/resources 에 생성
```xml title:MemberMapper
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
<mapper namespace="com.itwillbs.mapper.MemberMapper">

	<!-- ?에 해당하는 값을 memberDTO #{변수이름}을 넣기 -->
	<insert id="insertMember">
		INSERT INTO members(id, pass, name, date) 
		VALUES (#{id}, #{pass}, #{name}, #{date})
	</insert>
</mapper>
```

- root-context 에서 Spring jdbc , mybatis설정
```xml title:root-context
	<!-- DB연결 객체 생성 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul"></property>
		<property name="username" value="root"></property>
		<property name="password" value="1234"></property>
	</bean>
	
	<!-- MyBatis 설정 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="mapperLocations" value="classpath:mapper/**/*Mapper.xml"></property>
	</bean>	
	<!-- MyBatis 객체생성 -->
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
		<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
	</bean>
```

- MemberDAO 에서 Mapper 사용
```java title:MemerDAO
package com.itwillbs.dao;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;
import com.itwillbs.domain.MemberDTO;

@Repository
public class MemberDAO {
	
	//mybatis 객체 생성
	@Autowired
	private SqlSession sqlSession;
	String memberMapper_direct = "com.itwillbs.mapper.MemberMapper.";
	
	
	
	//회원가입
	public void insertMember(MemberDTO memberDTO)  {
		System.out.println("MemberDAO insertMember()<<");
		// memberMapper에서 sql구문 이름을 찾아서, memberDTO 멤버변수에 들어있는 값을 넣고 insert실행
		sqlSession.insert(memberMapper_direct + "insertMember",memberDTO);
		
	}
}

```