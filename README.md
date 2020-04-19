# interview-repo

## <b>호이스팅</b>
함수 안에 필요한 변수값을 전부 모아 유효 범위의 최상단에 선언하는 것
Javascript Parser가 함수 실행 전 해당 함수를 한 번 훑는다.

## <b>클로저</b>
내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있다. 이러한 메커니즘을 클로저라한다.


- 일반적으로 드는 클로저 예제
```
function outter() {
    var title = 'coding everybody';

    function inner() {
        alert(title);
    }
    inner();
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
## <b>비동기 처리</b>
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


## <b>Promise</b>
비동기 처리에 사용되는 객체

### <b>Promise 3가지 상태
1) Pending : 대기상태
2) Fulfilled : 완료
3) Rejected : 실패
</b>

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

## <b>Async & Await</b>
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

## <b>AJAX(Asynchronous Javascript And Xml)</b>
브라우저가 가진 XMLHttpRequest 객체를 이용하여 페이지의 일부만을 위한 데이터를 로드하는 기법 (웹페이지 리로드 X)

<b>장점</b><br>
웹페이지의 속도향상<br>
서버의 처리가 완료될 때까지 기다리지 않고 처리가 가능하다.<br>
서버에서 Data만 전송하면 되므로 전체적인 코딩의 양이 줄어든다.<br>
기존 웹에서는 불가능했던 다양한 UI를 가능하게 해준다. ( Flickr의 경우, 사진의 제목이나 태그를 페이지의 리로드 없이 수정할 수 있다.)<br>

<b>단점</b><br>
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

## <b>정적타입언어</b>
종류 : C, C++, JAVA 
컴파일 시 자료형을 정한다. (타입 안전성)

## <b>동적타입언어(인터프리터)</b>
종류 : Javascript, Ruby, Python 
실행 시 자료형을 정한다. (타입 에러 날 가능성이 있음)
