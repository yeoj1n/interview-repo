# interview-repo

## 호이스팅<br>
함수 안에 필요한 변수값을 전부 모아 유효 범위의 최상단에 선언하는 것
Javascript Parser가 함수 실행 전 해당 함수를 한 번 훑는다.

## 클로저<br>
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

``

