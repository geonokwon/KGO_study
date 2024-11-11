### Connection Pool : 연결정보를 저장하는 기억장소
- 데이터베이스와 연결된 Connection 연결 정보를 미리 웹서버 실행할때 미리 만들어 놓고(생성) 풀(Pool) 속에 저장해 두고 필요할때 마다 이 풀에 접근하여 Connection 연결 정보를 사용 작업이 끝나면 반환
- 장점 : 성능 향상(속도 높아짐), 디비 연결 정보 수정 최소화

- 톰캣(Tomcat) 에 제공되는 DBCP(DataBaseConnection Pool) 프로그램 사용
1) webapp - MATE-INF 폴더 - context.xml(연결정보 -> 자원의 이름)
2) DAO에서 자원의 이름을 불러서 사용
3) 작업을 완료하면 그 관련 기억장소 해제 작업
---
- context.xml 파일 생성
```xml title:Context.xml
<Context>
	<Resource
		name = "jdbc/MysqlDB"
		auth = "Container"
		type = "javax.sql.DataSource"
		driverClassName = "com.mysql.cj.jdbc.Driver"
		url = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul"
		username = "root"
		password = "1234"
	/>
</Context>
```

- DAO에서 불러와서 사용해보기
```java title:Java
public Connection getConnection() throws Exception {
	//import javax.naming.Context;
	//import javax.naming.InitialContext;
	Context init = new InitialContext();
	
	//context.xml 자원의장소/자원의이름
	DataSource ds = (DataSource)init.lookup("java:comp/env/jdbc/MysqlDB");
	Connection con = ds.getConnection();
	
	return con;
}
	```
- con.close() 를 해서 연결을 끊어줘야지 다른곳에서 불러도 부하가 안걸린다.
```java title:Java
//회원정보 확인
	public MemberDTO getMember (String id) {
		MemberDTO dto = new MemberDTO();
		Connection con = null;
		PreparedStatement select_All = null;
		ResultSet result_select = null;
		
		try {
			con = getConnection();
			String sql_Select_All = "SELECT * FROM members WHERE id = ?";
		    select_All = con.prepareStatement(sql_Select_All);
		    select_All.setString(1, id);
		    result_select = select_All.executeQuery();
			
		    if(result_select.next()) {
		    	dto.setId(result_select.getString("id"));
		    	dto.setPass(result_select.getString("pass"));
		    	dto.setName(result_select.getString("name"));
		    	dto.setDate(result_select.getTimestamp("date"));
		    	return dto;
		    }
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			if(result_select != null) {
				try { result_select.close(); }
				catch (SQLException e) { e.printStackTrace(); }
			}
			if(select_All != null) {
				try { select_All.close(); }
				catch (SQLException e1) { e1.printStackTrace(); }
			}
			if(con != null) {
				try { con.close(); }
				catch (SQLException e2) { e2.printStackTrace(); }
			}
		}
		return dto = null;
	}
```
- finally 사용해서 마지막에 null 이 아닌 값들 close() 해준다!

- 전체 다 똑같은 구문들이 실행되고 있으므로 메서드로 Close 모아주고 맴버 변수로 선언
```java title:Java
public class MemberDAO {
	// 멤버변수 
	Connection con = null;
	PreparedStatement insert = null;
	PreparedStatement select = null;
	PreparedStatement delete = null;
	PreparedStatement update = null;
	ResultSet result_select = null;
		//Close 호출 메서드
	public void dbClose() {
		if(result_select != null) {
			try { result_select.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(select != null) {
			try { select.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(insert != null) {
			try { insert.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(delete != null) {
			try { delete.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(update != null) {
			try { update.close(); }
			catch (SQLException e1) { e1.printStackTrace(); }
		}
		if(con != null) {
			try { con.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
	}
```