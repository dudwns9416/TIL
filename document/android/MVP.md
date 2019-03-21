# Model View Presenter

1. View로 사용자의 입력이 들어온다.
2. View는 Presenter로 작업을 요청한다.
3. Presenter에서 필요한 데이터를 Model에 요청한다.
4. Model은 Presenter에 필요한 데이터를 응답한다.
5. Presenter는 View에 데이터를 응답한다.
6. View는 Presenter로부터 받은 데이터를 화면에 보여준다.

View와 Model간 의존성은 없애지만 View와 Presenter가 1:1로 강한 의존성을 가지게 된다.

ToDo
------
* 어떤 방법으로 활용되고 있는가?