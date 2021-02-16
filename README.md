# interview-repo

## **호이스팅**
함수 안에 필요한 변수값을 전부 모아 유효 범위의 최상단에 선언하는 것
Javascript Parser가 함수 실행 전 해당 함수를 한 번 훑는다.

## **클로저**
내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있다. 이러한 메커니즘을 클로저라한다.


- 일반적으로 드는 클로저 예제
```
function outer() {
    var title = 'coding everybody';

    function inner() {
        alert(title);
    }
    return inner();
}
outer();
```

- React 클로저
```
function getAdd() {
    let foo = 1;
    return function() {
        foo += 1
        return foo;
    }
}

const add = getAdd()
console.log(add()) // 2
cosnole.log(add()) // 3

```
## **비동기 처리**
특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 것 (JS의 특징)

### 비동기 방식의 문제점
1) ajax
```
function getData() {
	var tableData;
	$.get('https://domain.com/products/1', function(response) {
		tableData = response;
	});
	return tableData;
}

console.log(getData()); // undefined
```

- 해결방안 : callback 함수 사용
```
function getData(callbackFunc) {
    $.get('https://domain.com/products/1', function(response) {
        callbackFunc(response);
    });
}
getData(function(tableData)) {
    console.log(tableData);
}
```

2. setTimeout()

```
console.log("hello1");

setTimeoit(() => {
    console.log("hello2")
}, 3000);
console.log("hello3");

/*
실행 결과
    hello1
    hello3
    hello2
*/
```


## **Promise**
비동기 처리에 사용되는 객체

### **Promise 3가지 상태
1) Pending : 대기상태
2) Fulfilled : 완료
3) Rejected : 실패
**

- callback 방식
```
function getData(callbackFunc) {
    $.get('url 주소/products/1', function(response) {
        callbackFunc(response)
    });
}
getData(function(tableData)) {
    console.log(tableData)
}
```
- Promise
```
function getData(callback) {
    // new Promise() ---> pending 상태
    // resolve ---> fulfilled 상태
    // reject 

    return new Promise(function(resolve, reject) {
        $.get('url 주소/products/1', function(response) {
            if(response) {
                resolve(response);//data 받았을 때 resolve 호출
            }
            reject(new Error("FAIL"));
        });
    });
}

getData().then(function(data) {
  console.log(data); // response 값 출력
}).catch(function(err) {
  console.error(err); // Error 출력
});

// 또는 
getData().then(function(data) {
  console.log(data); // response 값 출력
}, function(err) {
    console.log("err")
})
```

## **Async & Await**
콜백, 프로미스의 단점을 보완한 비동기 처리 패턴 중 하나

- 사용방법
```
async function 함수명() {
  await 비동기처리메소드;
}
```


예제 (여러개 비동기 처리)
```
function fetchUser() {
  var url = 'https://jsonplaceholder.typicode.com/users/1'
  return fetch(url).then(function(response) {
    return response.json();
  });
}

function fetchTodo() {
  var url = 'https://jsonplaceholder.typicode.com/todos/1';
  return fetch(url).then(function(response) {
    return response.json();
  });
}

async function logTodoTitle() {
    try{
        var user = await fetchUser();
        if (user.id === 1) {
            var todo = await fetchTodo();
            console.log(todo.title); 
        }
    } catch(error) {
        console.log("error");
    }
  
}
```
참고 : https://joshua1988.github.io/web-development/javascript/js-async-await/

## **AJAX(Asynchronous Javascript And Xml)**
브라우저가 가진 XMLHttpRequest 객체를 이용하여 페이지의 일부만을 위한 데이터를 로드하는 기법 (웹페이지 리로드 X)

**장점**<br>
웹페이지의 속도향상<br>
서버의 처리가 완료될 때까지 기다리지 않고 처리가 가능하다.<br>
서버에서 Data만 전송하면 되므로 전체적인 코딩의 양이 줄어든다.<br>
기존 웹에서는 불가능했던 다양한 UI를 가능하게 해준다. ( Flickr의 경우, 사진의 제목이나 태그를 페이지의 리로드 없이 수정할 수 있다.)<br>

**단점**<br>
히스토리 관리가 되지 않는다.<br>
페이지 이동없는 통신으로 인한 보안상의 문제가 있다.<br>
연속으로 데이터를 요청하면 서버 부하가 증가할 수 있다.<br>
XMLHttpRequest를 통해 통신하는 경우, 사용자에게 아무런 진행<br>
정보가 주어지지 않는다. (요청이 완료되지 않았는데 사용자가 페이지를 떠나거나 오작동할 우려가 발생하게 된다.)<br>
AJAX를 쓸 수 없는 브라우저에 대한 문제 이슈가 있다.<br>
HTTP 클라이언트의 기능이 한정되어 있다.<br>
지원하는 Charset이 한정되어 있다.<br>
Script로 작성되므로 디버깅이 용이하지 않다.<br>
동일-출처 정책으로 인하여 다른 도메인과는 통신이 불가능하다. <br>(Cross-Domain문제)

## **Axios**
Promise 기반으로 async/await 문법을 사용하여 XHR 요청을 할 수 있다.

**fetch 와 비교하여 좋은 점** <br>
구형브라우저 지원 <br>
요청 중단 가능 <br>
응답 시간 초과 설정 가능 <br>
CSRF 보호 기능 내장 <br>
JSON 데이터 자동변환 <br>
Node.js에서의 사용 <br>

## **정적타입언어**
종류 : C, C++, JAVA 
컴파일 시 자료형을 정한다. (타입 안전성)

## **동적타입언어(인터프리터)**
종류 : Javascript, Ruby, Python 
실행 시 자료형을 정한다. (타입 에러 날 가능성이 있음)

### SPA
장점<br>
사용자 친화적 (빠른 반응성, 화면전환 에니메이션 등 ) : client rendering, router<br>
상대적으로 적은 전체 트래픽 양 (Ajax, 캐쉬)<br>
필요한 부분만 새로 그리고 필요한 데이터만 새로 받는다.<br>
상대적으로 유지보수가 쉽고 개발속도가 빠르다. (모듈화, 컴포넌트화)<br>
프론트앤드와 백앤드의 분리로 인한 개발업무 분업화 및 협업이 쉽다.<br>
단점<br>
초기구동속도 => Lazy Loading으로 해결, 브라우저 캐쉬<br>
검색엔진 최적화(SEO) => SSR로 해결<br>
보안 => 핵심로직 최소화<br>
레거시 브라우저 지원(IE8 이하 지원)<br>
하얀 화면(개발자 버그) : Error Boundaries<br>

불변성을 지키려는 이유
- CPU의 낭비를 막기위해 불변함을 유지해야하고 이 불변함 유지를 하다보면 코드가 복잡해진다.
-> **Immutable.js 사용의 이유!!**

### **CORS(Cross Origin Resource Sharing)**
도메인 또는 포트가 다른 서버의 자원을 요청하는 매커니즘

**Same Origin Policy(동일 출처 정책)**<br>
내가 API 서버를 구축했는데 다른 웹 서비스에서 이 API를 마음대로 사용한다면?!?! <br>
-> Javascript 는 동일 출처 정책(Same Origin Policy) 라는 정책을 두어 다른 도메인의 서버에 요청하는 것을 보안 문제로 간주하고 이를 차단한다.

## **Prototype**
Javascript : 프로토타입 기반 객체 지향 언어

자바스크립트의 객체는 Prototype 이라는 내부 프로퍼티가 존재한다.

```
/*
    자바스크립트는 클래스라는 개념이 없음
    기존의 객체를 복사해서 새로운 객체를 생성하는 프로토타입 기반의 언어
*/  
var objectLiteral = {};
var objectConstructor = new Object(); 
```

**잘못된 객체 생성 예제**
```
function Person(){
	this.hand = 2;
	this.body = 1;
	this.nose = 1;
}

var kim = new Person();
var lee = new Person();
```
**올바른 객체 생성 예제**
```
function Person(){}

Person.prototype.hand = 2;
Person.prototype.body = 1;
Person.prototype.nose = 1;

let kim = new Person();
let lee = new Person();
```


### **OOP(Object Oriented Programming)**
1) 추상화 : 목적과 관련이 없는 부분을 제외하고 필요한 부분만을 포착한다.

2) 캡슐화 : 외부에 노출할 필요가 없는 정보들을 은닉한다.
3) 상속 : 부모 클래스가 자식 클래스에게 속성을 물려주는 것
4) 다형성 : 형태가 같은데 다른 기능을 하는 것
ex) 오버라이딩
## **쿠키 vs 세션 vs 로컬 스토리지 vs 세션 스토리지**
**쿠키** : 클라이언트 정보<br>
- 4096bytes 이하의 저장공간
- 서버측과 클라이언트측 양쪽에서 쿠키 데이터를 사용하는 
api가 존재
- 만료일 존재<br>

**웹 스토리지** (key-value 저장소)
쿠키의 문제점을 보안한다 (HTTP 요청에 전달 X )
1) 로컬 스토리지 : 클라이언트 정보를 영구적으로 보관
ex) 자동 로그인 기능
- 5MB의 공간을 허용
- 로컬 환경에서만 컨트롤
2) 세션 스토리지 : 세션 종료(브라우저 종료) 시 
ex) 일회성 로그인 정보
클라이언트 정보 삭제

## **웹 컴포넌트**
구성요소
- 템플릿(Templates)
- 데코레이터(Decorators)
- 커스텀 엘리먼트(Custom Element)
- 섀도우 DOM(Shadow DOM)

참고자료: https://d2.naver.com/helloworld/188655

## **javascript slice splice split **
### 1) slice : 기존 배열이 변하지 않는다.
### 2) splice : 기존 배열이 변한다.
### 3) split : delimeter를 기준으로 잘라 배열을 만든다.

```
var arr = [1,2,3,4,5]
var str = "hello javascript"

var a = arr.slice(0,2)// [1,2]
var b = arr.splice(0,2)// [1,2]
var c = str.split(" ")// ['hello', 'javascript']
```
## **웹 표준**
웹에서 표준적으로 사용되는 기술이나 규칙
### 웹표준을 지켰을 때의 장점
1) 소스의 통일화로 수정, 운영관리가 용이하다.<br/>
2) 다양한 브라우저, 휴대폰, pda, 장애인 지원용 프로그램에서 대응 가능<br/>
** 시각장애인의 경우 스크린 리더라는 소프트웨어를 사용하는데 스스로 웹페이지를 분석하지 못해 이미지만 두는 경우 해당 img를 인식하지 못한다. img tag의 alt 를 달아주면 해당 설명을 읽을 수 있다.
**
3) SEO(검색엔진최적화)가능<br/>
4) 효율적 소스를 통해 File Size 축소 및 서버 저장 공간을 절약가능하다.
5) CSS와 HTML 문서 분리를 통해 페이지 로딩속도 향상과 같은 효율적 마크업이 가능하다.
6) 다양한 브라우저에서의 호환이 가능하다.

## **SEO(검색엔진최적화) - 웹페이지 로딩속도를 개선하는 방법**
1)css, js 외부파일로 활용
2)과도한 이미지 자제
3)Img, js 압축
4)gzip을 사용하여 파일, 데이터를 압축

** **Flash of Unstyled Content(링크 사라짐)**
브라우저로 웹문서에 접근했을때, 미처 CSS의 스타일이 모두 적용되지 못한 상태에서 화면이 표시되어 발생하는 화면 깜박임, 스타일의 적용 전과 적용 후가 그대로 화면에 노출된 상태로 변경되는 현상

## **IIFE(Immediately Invoked Function Expression):즉시 실행되는 함수 표현식)**

```
(function temp() {
    
})
```
## **WEB Server와 WAS**
Web Server : 정적인 컨텐츠 제공
WAS : 동적인 컨텐츠 제공

## **MVC구조**
Model : 내부 비지니스 로직 처리<br>
View : 화면에 보여주기 위한 역할<br>
Controller : 화면의 로직 처리<br>
