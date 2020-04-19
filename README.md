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