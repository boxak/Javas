# 잉/력/시/장 프로젝트

잉력시장 URL : http://3.21.101.95:8000/javas/

### Index

- ##### 프로젝트 선정 및 배경

- ##### 개발 계획 수립

- ##### 기능 설명

- ##### 개발 후기

--------------------------------------------------



## 1. 프로젝트 선정 및 배경

![image-20200805141334915](https://user-images.githubusercontent.com/38724041/89514675-1feeaa80-d811-11ea-9bf1-267874db558a.png)

<img src="https://user-images.githubusercontent.com/38724041/89515196-c2a72900-d811-11ea-8ff8-d8b570ef9c01.png" alt="Arrow" style="zoom:5%;" />

![image-20200805141428604](https://user-images.githubusercontent.com/38724041/89514678-211fd780-d811-11ea-9a46-62c1502aada7.png)

<img src="https://user-images.githubusercontent.com/38724041/89515196-c2a72900-d811-11ea-8ff8-d8b570ef9c01.png" alt="Arrow" style="zoom:5%;" />

![image-20200805141513663](https://user-images.githubusercontent.com/38724041/89514679-21b86e00-d811-11ea-9483-b204d24c0154.png)

------------------------------------



## 2. 개발 계획 수립

우리 팀은 다음과 같은 순서로 개발 계획을 수립하였다.

#### 1. 설계 단계 준비

- 컨텐츠 회의
- 웹 화면 프로토타이핑



#### 2. 시스템 설계

- DB 및 ERD 설계
- 클래스 다이어그램, 시퀀스 다이어그램 작성
- View 설계



#### 3. 시스템 개발

- MVC 패턴에 기반해 java, jsp 파일 작성
- Boot Strap을 활용하여, CSS 및 JS 적용



#### 4. 테스트

- 사용자 중심의 기능 테스트
- 개발 도중 오류 및 개선점이 발견되면 상호 피드백과 수정을 하였음.
- 시스템 개발과 테스트를 반복하는 사이클이 생성.

--------------------------



## 3. 설계 단계

#### 3-1. 컨텐츠 회의

- 우리 팀의 아이템이 '아르바이트 사장'과 '아르바이트를 구하는 구직자' 간의 매칭 시스템인 만큼 기존의 유사한 서비스인 '당근마켓'의 플랫폼 구성을 참고하였음. 그 결과, 우리 팀은 서비스를 크게 3가지로 나누어 개발 전략을 짜기로 하였다.
  - 회원가입, 로그인을 담당하는 User 인터페이스
  - 게시판 글 작성, 수정, 삭제를 담당하는 Board 인터페이스
  - 특정 게시글에 댓글 작성,삭제를 담당하는 Review 인터페이스



회원 가입, 로그인, 게시판 기능은 당연히 필수적인 기능이다. Review의 경우, 해당 글을 올린 점주나 아르바이트생이 평이 좋은지 나쁜지를 코멘트 남기고 별점을 매김으로써 다른 사용자들에게 후기를 알려줄 수 있고, 해당 글 작성자에게 피드백을 주는 기능을 할 수 있다. 그래서 추가하기로 한 기능이다. 



#### 3-2. 화면 프로토타이핑

- 프로토타이핑 툴은 카카오에서 제공하는 ovenapp.io 를 사용하였다.

- 공유 링크 : **https://ovenapp.io/view/DIwGXLWwVcOJAOTBphPlk4mARTfSkNLh/**

  페이지를 다음과 같이 구성하였다. 

  - 메인화면

    - 회원가입, 로그인 화면
    - 알바 구직, 구인 게시판
    - 댓글란과 댓글 입력 화면
    - 마이페이지



## 4. 시스템 설계

#### 4-1. DB 설계

DB는 MongoDB를 사용하였다. SQL 계열들을 놔두고 MongoDB를 쓴 이유는 다음의 장점이 있어서였다.

	- 사용이 간편하다.(코드 작성이 편하다) 즉, 게시판 db와 리뷰 db의 경우, 리뷰 db들은 게시판 db들의 각 행들을 참조하는 데, SQL문을 작성할 때, 쿼리가 복잡해졌다.
	- 스키마가 없기 때문에 유연하다. -> 필드 추가,  삭제 등의 추가 작업을 하는데 있어 번거로움이 없다. 개발 중간중간에 VO class의 변수를 추가하거나 필요없는 변수를 제거하는 일이 있었는데, 그럴 때마다 SQL문을 수정하는 작업을 하지 않아도 된다는 것이다.



- UserInfo(유저 정보 컬렉션)

  ```json
  {
      "Document" : { 
      	"_id" : "UserInfo에 도큐먼트를 추가하면 자동으로 생성되는 인덱스 문자열"
      },
      {
      	"id" : "유저 아이디",
      	"password" : "비밀번호",
      	"name" : "유저 이름",
      	"email" : "유저 이메일",
      	"birthday" : "유저의 생년월일",
      	"sex" : "유저 성별",
      	"phone" : "유저의 핸드폰 번호",
      	"address" : "유저의 거주지 주소",
      	"date" : "회원가입 일자(회원가입 하는 순간의 시각 저장)",
      	"isEmployer" : "사업자인지, 알바생인지 구분, 1이면 사업자, 2이면 알바생"
  	}
  }
  ```



- BoardInfo(구인 게시판인 jobadInfo, 구직 게시판인 wantadInfo로 나눠져 있지만 구조는 동일)

  ```json
  {
      "Document" : {
          "_id" : "BoardInfo에 도큐먼트를 추가하면 자동으로 생성되는 인덱스 문자열"
      },
      {
      	"id" : "작성자 아이디",
      	"name" : "작성자 이름",
      	"title" : "게시글 제목",
      	"content" : "게시글 내용",
      	"date" : "작성시각",
      	"hit" : "게시글 조회수",
      	"reviewCnt" : "게시글에 달린 리뷰 수"
  	}
  }
  ```

  

- ReviewInfo(이것도 역시 구인 게시판 리뷰와 구직 게시판 리뷰로 나눠져 있다.)

```json
{
    "Document" : {
        "_id" : "ReviewInfo에 도큐먼트를 추가하면 자동으로 생성되는 인덱스 문자열"
    },
    {
    	"postId" : "리뷰가 달린 게시글의 _id 속성을 가져와서 저장",
    	"id" : "리뷰 작성자 아이디",
    	"targetId" : "리뷰가 달린 게시글 작성자의 id",
    	"comment" : "리뷰 내용",
    	"date" : "리뷰 작성 시각",
    	"rate" : "평점"
	}
}
```



MongoDB로 db를 구축하면서 느낀 점은, Java와 연동하여 코드 작성에 굉장히 편하지만, 다른 컬렉션(SQL의 Table과 대응되는 개념)의 field를 참조하거나 join 기능이 없어서 ReviewInfo가 BoardInfo의 

_id를 참조하게 하려면 따로 도큐먼트에 속성을 추가해서 명시해줘야 한다. 이는 NoSQL의 단점이다.



#### 4-2. 클래스 설계

MVC 패턴에 기반해 클래스를 설계하였다. Spring MVC 프로젝트를 사용하였고, VO,DAO,Conroller로 나누어 작동하도록 하였다. 그리고 로그인 여부와 사용자 성격에 따라 접근권한을 다르게 주기 위해 Interceptor를 만들었다.

- VO
  - UserVO : 유저 정보와 매핑되는 VO
   - BoardVO : 게시글 VO
   - ReviewVO : 리뷰 VO
- DAO
  - UserDAO, UserDAOImpl : DAOImpl 클래스가 DAO인터페이스를 상속하게 하였다.
  - BoardDAO, BoardDAOImpl
  - ReviewDAO, ReviewDAOImpl
  - PagingControl : 게시판 페이징에 관한 클래스
  - DAOTest : DAO 기능 테스트 클래스
- Controller
  - UserController
   - LoginController, LogoutController
   - FindController : 사용자가 본인 Id,Password 를 잊었을 때, 아이디 찾기, 비번 찾기 기능을 사용할 수 있도록 구현한 컨트롤러
   - BoardController
   - ReviewController
   - MainController : 메인 화면에서 다른 페이지 이동할 때 호출되는 컨트롤러
   - MypageController : 마이페이지 기능 구현한 컨트롤러
   - AuthController : 접근권한에 대한 컨트롤러, 권한이 없으면 authfail.jsp 로 이동하도록 돼있음.





## 5. 기능 설명

위에서 설명한 게시판 작성, 리뷰 작성 등의 기능에서 부가적으로 구현한 기능들에 대해 설명하겠다.

#### 5-1. 비밀번호 일치 여부 확인

- 불일치

![image-20200806163955557](https://user-images.githubusercontent.com/38724041/89514681-21b86e00-d811-11ea-9561-6b37a649cda5.png)



- 일치

![image-20200806164025279](https://user-images.githubusercontent.com/38724041/89514684-22510480-d811-11ea-9eb9-6da52ea74f14.png)





#### 5-2. 도로명 주소 API

- 도로명 주소 팝업창

  ![image-20200806164453257](https://user-images.githubusercontent.com/38724041/89514691-22e99b00-d811-11ea-8c26-f138695d3c00.png)

- 입력 후 회원가입 화면

  ![image-20200806164527932](https://user-images.githubusercontent.com/38724041/89514692-22e99b00-d811-11ea-8f4c-2d789289805f.png)



#### 5-3. 프로필 사진 등록

- 회원가입 시 등록

  ![image-20200806164627067](https://user-images.githubusercontent.com/38724041/89514693-23823180-d811-11ea-92a3-8c6873813d7b.png)

- 프로필 사진이 필요한 곳에 "xx님 환영합니다." 문구와 함께 프로필 사진이 올라오도록 구현.

  ![image-20200806164753686](https://user-images.githubusercontent.com/38724041/89514694-241ac800-d811-11ea-9c84-e07e40411dbd.png)



#### 5-4. 아이디 저장

- '아이디 저장'에 체크하고 로그인

  ![image-20200806164946833](https://user-images.githubusercontent.com/38724041/89514696-241ac800-d811-11ea-9f6b-014163fa3766.png)



- Java 단에서 클라이언트의 Cookie 객체에 로그인 아이디를 저장하게 구현하였다.

  ![image-20200806165053605](https://user-images.githubusercontent.com/38724041/89514702-24b35e80-d811-11ea-89d3-dfcf6ed56700.png)

  (다음 로그인 시도 시, yejis 라는 아이디가 떠 있는 모습)





## 6. 개발 후기

#### 6-1. 간결함이 중요하다

- DB 설계시, 여러 속성을 컬럼명으로 넣으려 하다보니 SQL 작성, Java 코드 작성이 늘어나고 관리하기 번거로워지는 문제가 있었다. 처음에는 Oracle DB를 사용했는데, 각 테이블 간 참조, join 쿼리를 설정해놓은 시점에서 컬럼을 추가하거나 삭제하는 데 있어 많은 불편함이 있었다. 만일, 처음부터 지금 사용하는 것처럼 필요한 컬럼들만 만들었다면 DB를 수정하는 번거로움은 없었을 것이다.
- MongoDB를 사용할 때 이점은 Java 코드 자체가 Oracle을 쓸때보다 간결해져서 기능에 문제가 있어서 수정해야 할 때, 수정 작업이 굉장히 용이했다는 것이고 이를 통해 간결한 코드의 힘을 깨달을 수 있었다. 물론, 이 프로젝트보다 더 방대한 사이트를 관리하는 데 있어서는 여러 테이블 간 join 쿼리를 지원하며 고가용성인 MySQL 같은 RDBMS가 더 적합할 것이다.



#### 6-2. 합칠 수 있는 것은 최대한 합치자.

- 처음에는 BoardVO, BoardDAO 클래스가 없었다. jobadVO와 wantadVO 처럼 구직 게시판, 구인 게시판 용 클래스와 View를 다 나누었다. jobad 들은 본인이 담당했고, wantad 는 다른 분이 담당했는데, 그러다보니 코딩 스타일같은 것들이 달라서 프로젝트 관리에 어려움을 겪었다. 그래서 크래스와 뷰들을 BoardDAO, BoardView 등으로 통일하고, jsp에 boardType만 jobad, wantad로 다르게 주면 그에 맞게 기능할 수 있도록 수정했다. 이것 또한 간결함의 장점인데, 코드 관리가 이전에 비해 너무 잘되는 것을 느꼈다.

- 프로젝트 관리에 있어서 각 모듈 간의 응집도와 결합도의 적절한 균형이 중요한데, 처음에는 조금이라도 다른 기능을 하는 것들을 (ex. 구직, 구인 게시판)을 다른 클래스 및 뷰로 만들었다.

  이 경우는 지나치게 분리를 해놓은 경우이다. 이 때는 중복해서 쓰지 않아도 될 코드를 엄청나게 중복하는 문제가 있었다.

  생각해보니 이렇게 할 필요가 없어서 합칠 수 있는 것은 최대한 합쳤다.



#### 6-3. 다른 팀원으로부터 배우는 것이 더 많았던 좋은 추억이었다.

- 이번 프로젝트를 끝내고 나니 느낀 것은 내가 많이 배웠다는 것이다. 특히, 디자인에 능숙하신 해림씨로부터 부트스트랩을 적용하는 법과 화면 디자인에 대한 감각을 보고 배울 수 있었다. 그리고 수이씨는 지금까지 배운 것들을 제대로 적용하실 줄 아셔서 내가 미처 생각도 못했던 것들을 많이 알려주셨다. 가령, 로그인 처리할 때, interceptor를 거치게 하는 처리를 통해, 각 유저 별 접근 권한 처리를 간편하게 할 수 있음을 알게 되었다.
- 프로젝트 기간동안, 전에 해보지 않은 것을 만들어보려 하니 다들 막막한 상태에서 시작하면서, 골치 아픈 문제들을 마주했다. 하지만, 난관을 하나씩 해결하며 다같이 밤샘을 하며 팀워크가 향상되고 자유로운 분위기 속에서 부족한 것이 있을 때마다 주저하지 않고 머리를 맞대고 해결을 해나갔다. 다른 프로젝트 조 앞에서 우리의 결과물을 발표하며 내 생애 처음으로 내 자식같은 느낌이 물씬 풍기는 결과물을 만들어 선보인다는 생각에 정말로 뿌듯했다. '이런 것도 내가 할 수 있구나'하는 자신감이 들었다.
