# JQu
- 자바스크립트의 오픈소스 라이브러리
##### 장점
- 웹페이지상에서 요소 를 쉽게 찾고 조작할 수 있다.
- 네트워크, 애니메이션 등 다양한 기능을 제공한다.
- 메서드 체이닝 등 짧고 유지관리가 용이한 코드 작성을 지원한다.

##### jQuery 기본
```js title:jquery
//바닐라 자바스크립트
document.querySelector("h1").style.color = "blue";
let h2 = document.querySelector("h2");
h2.style.color = "red";

//jQuery
$(document).ready(function() {
	$("h3").css("color", "purple");

	let h4 = $("h4");
	h4.css("color", "orange");

	$("h5").css({
		'color':'black',
		'fontSize': '2rem',
		'width': 200,
		'height': 100,
		'backgroundColor': 'orange',
		'border': '2px solid #ccc',
		'textAlign': 'center',
		'lineHeight': '3em'
	});
});
```

##### jQuery 이벤트와 이벤트 핸들러
```js title:jQuery
 
//1. 이벤트 명령어 형식
//하나의 선택자에 이벤트 하나를 등록해서 실행하는 방법
$("#btn_1").click(() => {
	alert("btn_1 : 화면 색상이 변경됩니다. !!!");
})

//2.이벤트 등록 메서드 방식
//하나의 선택지에 이벤트 여러개를 등록하여 실행하는 방법
$("#btn_2").on("click", () => {
	alert("btn_2 : 화면 색상이 변경됩니다. !!!");
})

//3.이벤트 발생을 한번만 수행하는 형식
//one('이벤트종류, 실행할 함수')
$("#btn_3").one("click", function(){
	alert("btn_3 : 화면 색상이 변경됩니다. !!!");
})

//4.이벤트 제거 : off() 메서드 사용
//off('이벤트 종류 or 이벤트 명')
$("#btn_1").off("click");

//2-1.이벤트 등록 메서드 방식
$("#d_4").on({
	"mouseover" : function(){
		$("#d_4").css({
		   'backgroundColor' : 'red',
		   'color': '#fff',
		   'width': 200,
		   'height': 200 
		});
	},
	"mouseout" : function(){
		$("#d_4").css({
			'backgroundColor' : 'yellow',
			'color': 'black',
			'width': 100,
			'height': 100 
		});
	}
});
```

```js title:jQuery
<body>
    <button id="btn_1">text 수정</button>
    <button id="btn_2">text 가져오기</button>

    <p id="first">text1</p>
    <p id="second">시작시간</p>
    <hr>
    <ul>
        <li>오후 1시 A클래스에서 시작합니다.</li>
        <li>오후 8시 B클래스에서 시작합니다.</li>
    </ul>

    <script>
        $(function(){
            //텍스트 수정
            $("#btn_1").click(() => {
                $("#first").text("수업시작");
            })
            
            //텍스트 가져오기
            $("#btn_2").click(() => {
                let some = $("ul>li:eq(0)").text() //가져오기;
                $("#second").text(some);
            })
        });
    </script>
</body>
```

