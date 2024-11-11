- Java Resources 파일안의 src/main/java 폴더안에 파일을 위치시킨다.
	- Member 패키지를 만든다
	- MemberDAO 클래스를 만들어 준다
```Java title:Java
package Member;

public class MemberDAO {
// 멤버변수

// 메서드 (멤버함수) : 기능을 구현(명령 모임)
// 회원가입하는 기능 정의
	public void insertMember() {
		System.out.println("MemberDAO insertMember() <<");
	}
}
```

- jsp 파일에서 MemberDAO 사용하기 위한 방법
	- 패키지(폴더) member 자바파일 MemberDAO
	- insertMember() 메서드 정의
	- MemberDAO 사용하기위해서 기억장소 할당, 기본 초기값 할당
```html title:JSP
MemberDAO dao = new MemberDAO();
dao.insertMember(id, pass, name, date);
```
---
- 자바의 memberDAO 의 insertMember()함수에 기능을 구현한다
```Java title:Java
//throws SQLException, ClassNotFoundException
public void insertMember(MemberDTO dto) {
	System.out.println("MemberDAO insertMember() <<");
	System.out.println(dto.getId());
	System.out.println(dto.getPass());
	System.out.println(dto.getName());
	System.out.println(dto.getDate());
	
	String id = dto.getId();
	String pass = dto.getPass();
	String name = dto.getName();
	Timestamp date = dto.getDate();
	
	try {
		Class.forName("com.mysql.cj.jdbc.Driver");
		String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
		String dbUser = "root";
		String dbPass = "1234";
		Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);
		String sql_Insert = "INSERT INTO members(id, pass, name, date) VALUES (?, ?, ?, ?)";
		PreparedStatement insert = con.prepareStatement(sql_Insert);
		insert.setString(1, id);
		insert.setString(2, pass);
		insert.setString(3, name);
		insert.setTimestamp(4, date);
		insert.executeUpdate();
	}
	catch (Exception e) {
		e.printStackTrace();
	}
	return;
}
```
---
- 하나의 데이터에 여러개 값 넣어서 가져오기
	- id, pass, name, date .... 하나의 공간에 저장 -> 전달
	- 패키지(폴더) member 자바파일 MemberBean, MemberVO, MemberDTO 만들기
```Java title:Java
package Member;

import java.sql.Timestamp;

public class MemberDTO {
	
	//멤버변수	
	//중요한 데이터를 담아서 전달하기 위해서 외부 접근 못하게 설정
	private String id;
	private String pass;
	private String name;
	private Timestamp date;
	
	//생성자(생략 초기값 할당)
	
	//메서드(멤버 함수)
	//getter AND setter (alt + shift + s -> r)
	public String getId() {
		return id;
	}
	
	public void setId(String id) {
		this.id = id;
	}
	
	public String getPass() {
		return pass;
	}
	
	public void setPass(String pass) {
		this.pass = pass;
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {	
		this.name = name;
	}
	
	public Timestamp getDate() {	
		return date;
	}
	
	public void setDate(Timestamp date) {	
		this.date = date;
	}
}
	
```
---
- 로그인 userCheck 메서드 만들기(MemberDAO)
```Java title:Java
public boolean userCheck(String id, String pass) {

	try {
		Class.forName("com.mysql.cj.jdbc.Driver");
		String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
		String dbUser = "root";
		String dbPass = "1234";
		Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);
		
		String sql_Select = "SELECT * FROM members WHERE id = ? AND pass = ?";
		PreparedStatement select = con.prepareStatement(sql_Select);
		select.setString(1, id);
		select.setString(2, pass);
		ResultSet result_select = select.executeQuery();
	if (result_select.next()) { return true; }
	}
	catch (Exception e) { e.printStackTrace(); }
	return false;
}
```

- 로그인 userCheck 를 부를 jsp 작업
```html title:JSP
<%
	String id = request.getParameter("id");
	String pass = request.getParameter("pass");
	MemberDAO dao = new MemberDAO();
	if(dao.userCheck(id, pass)){
		System.out.println(dao.userCheck(id, pass));
		response.sendRedirect("Main.jsp");
	}
	else{
		System.out.println(dao.userCheck(id, pass));
		response.sendRedirect("Login.jsp");
	}
%>
```

- jsp 에서 MemberDAO 의 userCheck 메서드 불러와 boolean 값으로 반환 받아 맞는지 아닌지 판단해 보내줄수 있다.
---
- select 하기 위한 java 적용(MemberDTO 객체를 이용) jsp getMember 를 가져온다
```Java title:Java
public MemberDTO getMember (String id) {
	MemberDTO dto = new MemberDTO();
	try {
		Connection con = information();
		String sql_Select_All = "SELECT * FROM members WHERE id = ?";
		PreparedStatement select_All = con.prepareStatement(sql_Select_All);
		select_All.setString(1, id);
		ResultSet result_select = select_All.executeQuery();
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
	return dto = null;
}
```

- jsp 에서는 MemberDTO dto = MemberDAO.getMember(id) 로 반환값으로 DTO 객체를 반환 받아 DTO 에 저장된 member를 가져온다

- 유저 회원 삭제
```Java title:Java
//회원삭제
public boolean deleteMember(String id, String pass) {
	boolean isCheck = false;
	try {
		Connection con = information();
		
		String sql_select = "SELECT id, pass FROM members WHERE id = ? AND pass = ?";
		PreparedStatement select = con.prepareStatement(sql_select);
		select.setString(1, id);
		select.setString(2, pass);
		ResultSet result_select = select.executeQuery();
		
		if(result_select.next()) {
			String sql_delete = "DELETE FROM members WHERE id = ?";
			PreparedStatement delete = con.prepareStatement(sql_delete);
			delete.setString(1, id);
			delete.executeUpdate();
			isCheck = true;
			System.out.println("삭제성공");
		}
		else {
			System.out.println("비밀번호 일치하지 않음");
			isCheck = false;
		}
	}
	catch (Exception e) {
		e.printStackTrace();
		isCheck =  false;
	}
	return isCheck;
}
```

- 유저 수정 (업데이트)
```Java title:Java(updateMember)
//회원정보 수정
public boolean updateMember(MemberDTO dto) {

	try {
		Connection con = information();
		String sql_Update = "UPDATE members SET name = ?, date = ? WHERE id = ? ";
		PreparedStatement update = con.prepareStatement(sql_Update);
		update.setString(1, dto.getName());
		update.setTimestamp(2, dto.getDate());
		update.setString(3, dto.getId());
		update.executeUpdate();
		return true;
	}
	catch (Exception e) {
		e.printStackTrace();
		return false;
	}
}
```

- 전체회원목록 불러오기
```Java title:Java(updateMember)
//전체회원 리스트
public ArrayList<MemberDTO> getAllMember(){

		ArrayList<MemberDTO> resultDTO = new ArrayList<MemberDTO>();
	try {
		Connection con = information();
		String sql_Select = "SELECT * FROM members";
		PreparedStatement select = con.prepareStatement(sql_Select);
		ResultSet result_select = select.executeQuery();
		while (result_select.next()) {
			MemberDTO dto = new MemberDTO();
			dto.setId(result_select.getString("id"));
			dto.setPass(result_select.getString("pass"));
			dto.setName(result_select.getString("name"));
			dto.setDate(result_select.getTimestamp("date"));
			resultDTO.add(dto);
		}
	}
	catch (Exception e) {
		e.printStackTrace();
	}
	return resultDTO;
}
```

---
#### 게시판 만들기
- 글쓰기 만들기
- 여기서 num 값을 디비에서 제일 큰수로 받아와서 더한다
```Java title:Java
//numCount 값 리턴해주기
	public int numCount() {
		int count = 0;
		try {
			Connection con = getConnection();
			String sql_Select_num = "SELECT MAX(num) as num FROM board";
		    PreparedStatement select = con.prepareStatement(sql_Select_num);
		    ResultSet result_num =  select.executeQuery();
			while(result_num.next()) {
				count = result_num.getInt("num");
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
		return count;
	}
```

```Java title:Java

//글쓰기 작업시 폼에서 넘어온 값을 BoardDTO로 받은다음 업데이트한다
public void insertBoard(BoardDTO dto) {
		try {
			Connection con = getConnection();
			String sql_Insert = "INSERT INTO board(num, name, subject, content, readcount, date) VALUES(?, ?, ?, ?, ?, ?)";
		    PreparedStatement insert = con.prepareStatement(sql_Insert);
		    insert.setInt(1, dto.getNum());
		    insert.setString(2, dto.getName());
		    insert.setString(3, dto.getSubject());
		    insert.setString(4, dto.getContent());
		    insert.setInt(5, dto.getReadCount());
		    insert.setTimestamp(6, dto.getDate());
		    insert.executeUpdate();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
	}
```

- 게시판 글 리스트 보여주기
```Java title:Java
	//게시판 에 전체 글을 보여주기(selectAll 하기)
	public ArrayList<BoardDTO> selectAll(){
		ArrayList<BoardDTO> dto = new ArrayList<BoardDTO>();
		try {
			Connection con = getConnection();
			String sql_Select_All = "SELECT * FROM board ORDER BY num desc";
		    PreparedStatement select_All = con.prepareStatement(sql_Select_All);
		    ResultSet result_All = select_All.executeQuery();
		    while(result_All.next()) {
		    	BoardDTO dtoBoard = new BoardDTO();
		    	dtoBoard.setNum(result_All.getInt("num"));
		    	dtoBoard.setName(result_All.getString("name"));
		    	dtoBoard.setSubject(result_All.getString("subject"));
		    	dtoBoard.setContent(result_All.getString("content"));
		    	dtoBoard.setReadCount(result_All.getInt("readcount"));
		    	dtoBoard.setDate(result_All.getTimestamp("date"));
		    	dto.add(dtoBoard);
	    	}
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		return dto;
	}
```

- 글읽기 
```Java title:Java
//글 읽기 num 을 기준으로 그 번호로 된 글 가져오기
	public BoardDTO selectNum (int num) {
		BoardDTO dto = null;
		try {
			Connection con = getConnection();
			String sql_Select_Num = "SELECT * FROM board WHERE num = ?";
			PreparedStatement select_Num = con.prepareStatement(sql_Select_Num);
			select_Num.setInt(1, num);
			ResultSet result_All = select_Num.executeQuery();
			
			if(result_All.next()){
				dto = new BoardDTO();
				dto.setNum(result_All.getInt("num"));
				dto.setName(result_All.getString("name"));
				dto.setSubject(result_All.getString("subject"));
				dto.setContent(result_All.getString("content"));
				dto.setReadCount(result_All.getInt("readcount"));
				dto.setDate(result_All.getTimestamp("date"));
			}
			
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		
		return dto;
	}
```

- id 와 글쓴이가 동일하다면 수정 (글 수정)
```Java title:Java
//글 수정하기
	public void update(BoardDTO dto) {=
		try {
			Connection con = getConnection();
			String sql_Update = "UPDATE board SET subject = ?, content = ? WHERE num = ?";
		    PreparedStatement update = con.prepareStatement(sql_Update);
		    update.setString(1, dto.getSubject());
		    update.setString(2, dto.getContent());
		    update.setInt(3, dto.getNum());
		    update.executeUpdate();
		}catch (Exception e) {
			e.printStackTrace();
		}
		
	}
```

- id 와 글쓴이가 동일하다면 (글 삭제)
```Java title:Java
//글 삭제하기
	public void delete(int num) {
		try {
			Connection con = getConnection();
			String sql_Delete = "DELETE FROM board WHERE num = ?";
		    PreparedStatement delete = con.prepareStatement(sql_Delete);
		    delete.setInt(1, num);
		    System.out.println("삭제완료");
		    delete.executeUpdate();
			
		}catch (Exception e) {
			e.printStackTrace();
		}
		
	}
```

- 글 읽으러 들어왔을때 조회수 올려주기
```Java title:Java
//readCount 조회수 올려주기 
	public void readCount(int num, int readCount) {
		try {
			readCount += 1;
			Connection con = getConnection();
			String sql_Update = "UPDATE board SET readcount = ? WHERE num = ?";
			PreparedStatement update = con.prepareStatement(sql_Update);
			update.setInt(1, readCount);
			update.setInt(2, num);
			update.executeUpdate();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
	}
```
