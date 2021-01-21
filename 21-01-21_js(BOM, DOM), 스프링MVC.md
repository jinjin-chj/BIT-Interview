## 1.게시판 replyShape 생성시 아래의 쿼리문에서 bStep > ? 은 무슨 의미 인가?

>> update mvc_board set bStep = bStep + 1 where bGroup = ? and bStep > ?

답변 간의 순서를 정해주기 위한 조건. 같은 그룹번호 내에서 (같은 원글을 가지고 있을 경우) 
작성 일자가 빠른 답변이 위로 오게 된다.


## 2. sql 문제
>> 17. 부서별 급여 평균을 출력하시오.  

select deptno, trunc(avg(sal)) from emp group by deptno;  
>> 18. 오늘은 몇요일인가?   

select sysdate, to_char(sysdate, 'day') as day from dual;  
>> 10. EMP Table에서 급여가 1800 이상이면 ‘good’, 아니면 ‘poor’를 출력하시오.   

select ename, sal,   
    case when sal >= 1800 then 'good'  
        when sal<1800 then 'poor'   
        end as RESULT from emp order by sal asc;  
	
	
	
## 3. 가위바위보 이미지 넣어서 짜시오.
Ver.1 <테이블 없이>
~~~javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">
		function User() {
			var result;

			this.getResult = function() {
				return this.result;
			};

			this.setResult = function(result) {
				this.result = result;
				if (this.result == 1) {
					document.write("당신이 낸 것");
					var img = new Image();
					img.src = 'sc.jpg';
					document.body.appendChild(img);
				} else if (this.result == 2) {
					document.write("당신이 낸 것");
					var img = new Image();
					img.src = 'rock.jpg';
					document.body.appendChild(img);
				} else if (this.result == 3) {
					document.write("당신이 낸 것");
					var img = new Image();
					img.src = 'pa.jpg';
					document.body.appendChild(img);
				} else
					// 써놓긴 했는데 그냥 계속 진행됨.
					alert("error! 1, 2, 3 중에 입력하십시오.");
			}
		}

		function Com() {
			var result;

			this.getResult = function() {
				return this.result;
			};

			this.setResult = function() {
				this.result = Math.floor(Math.random() * 3 + 1);
			}

			this.playCom = function() {
				if (this.result == 1) {
					document.write("상대방이 낸 것");
					var img = new Image();
					img.src = 'sc.jpg';
					document.body.appendChild(img);
				} else if (this.result == 2) {
					document.write("상대방이 낸 것");
					var img = new Image();
					img.src = 'rock.jpg';
					document.body.appendChild(img);
				} else if (this.result == 3) {
					document.write("상대방이 낸 것");
					var img = new Image();
					img.src = 'pa.jpg';
					document.body.appendChild(img);
				}
			}
		}

		function Game(user, com) {
			var result = (user - com + 3) % 3;

			if (result == 1) {
				document.write("당신이 이겼습니다!");
			} else if (result == 2) {
				document.write("당신이 졌습니다!");
			} else if (result == 0) {
				document.write("비겼습니다!");
			}
		}

		var user = new User();
		var str = prompt("1(가위), 2(바위), 3(보) 중 한 가지를 입력하십시오.", "ex) 1");
		user.setResult(Number(str));

		var com = new Com();
		com.setResult();
		com.playCom();

		Game(user.getResult(), com.getResult());
	</script>
</body>
</html>
~~~
![rcp결과](https://user-images.githubusercontent.com/75013048/105341653-b594bf80-5c22-11eb-9a0f-4568037532c7.JPG)


Ver 2. <테이블 추가>
~~~javascript
<!DOCTYPE html>
<html>

<head>
<title>Javascript</title>
<style>
table {
	border-collapse: collapse;
	text-align: center;
}

tr td {
	border: 2px solid black;
}
</style>
<script>
	function player() {
		var hand = "";
		var rcp = [ "가위", "바위", "보" ];
		var rcpImg = [ "sc.jpg", "rock.jpg", "pa.jpg" ];

		this.setHand = function(hand) {
			this.hand = hand;
		};

		this.result = function() {
			var comNum = Math.floor(Math.random() * 3);
			var com = rcp[comNum];
			var me = 0;
			var result = "";

			if (this.hand == "가위") {
				me = 0;
				if (com == "가위") {
					result = "비겼습니다!";
				} else if (com == "바위") {
					result = "당신이 졌습니다!";
				} else {
					result = "당신이 이겼습니다.";
				}
			} else if (this.hand == "바위") {
				me = 1;
				if (com == "가위") {
					result = "당신이 이겼습니다!";
				} else if (com == "바위") {
					result = "비겼습니다!";
				} else {
					result = "당신이 졌습니다!";
				}
			} else {
				me = 2;
				if (com == "가위") {
					result = "당신이 졌습니다!";
				} else if (com == "바위") {
					result = "당신이 이겼습니다!";
				} else {
					result = "비겼습니다!";
				}
			}
			document.write("me : " + this.hand + "<br />" + "com : " + com);

			var myImg = document.createElement("img");
			var comImg = document.createElement("img");

			var str = "";
			str += "<table><tr><td>당신</td><td>상대방</td></tr>";
			str += "<tr><td><img id='user'></td><td><img id='computer'></td></tr>";
			str += "<tr><td colspan='2'>" + result + "</td></tr></table>";

			document.body.innerHTML = str;

			var userImgNode = document.getElementById("user");
			userImgNode.setAttribute("src", rcpImg[me]);
			userImgNode.setAttribute("width", 200);
			userImgNode.setAttribute("height", 300);

			var comImgNode = document.getElementById("computer");
			comImgNode.setAttribute("src", rcpImg[comNum]);
			comImgNode.setAttribute("width", 200);
			comImgNode.setAttribute("height", 300);
		};
	}
</script>

</head>

<body>
	<script>
		var me = new player();
		me.setHand(prompt("(가위 / 바위 / 보) 중, 한 가지 값을 입력하세요."));
		me.result();
	</script>

</body>

</html>
~~~

![rcp테이블](https://user-images.githubusercontent.com/75013048/105341660-b6c5ec80-5c22-11eb-855e-27e30f212413.JPG)


## 4. Bom , 과 Dom 이란?

## 5. <조별회의 시간에 토의 해보기>
>> 메인클래스에서 제네릭 쓸 때, 
shape.setWidth(10); 에 왜 에러나냐? 메모리 올리는 거랑 관련해서 이해하기.  
왜 get은 되는데 set은 에러나냐?  

다형성. 부모=자식 이 관계라서 부모는 자식의 set함수에 접근할 수 없다.

