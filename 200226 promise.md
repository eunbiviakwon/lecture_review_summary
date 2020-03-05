## promise

우리는 axios라는 라이브러리를 사용해서 api호출을 하는 프로젝트를 진행할 예정이다.

하지만 axios는 promise 기반으로 만들어져있기 때문에 promise에 대한 이해가 필수이다.

따라서 axios를 배우기 전에 promise를 배우고 넘어가자

promise는 비동기작업을 동기적으로 실행하고 싶을 때 사용한다.

비동기작업을 동기적으로 실행하고 싶다는 것이 무슨 말일까?

```javascript
console.log('1');

setTimeout(() => {
	console.log('2');
}, 1000)

console.log('3')
```

출력 값의 순서가 어떻게 될까?

답은 1 3 2 이다.

이는 estTimeout이 비동기적으로 작동했기 때문이다.

이를 동기적으로 바꾸어 1 2 3이 나오도록 해보자

```javascript
setTimeout(() => {
	console.log('1');
    console.log('2');
    console.log('3');
}, 1000)
```

출력값은 1 2 3 이 나오지만 우리가 원하던 것과는 조금 다르게 동작한다.

우리가 원하던 것은 1이 출력되고 1초 있다가 2가 출력되고 이어서 바로 3이 출력되도록 하는 것이다.

이렇게 바꿀 수는 있다.

```javascript
function test(flag) {
    if(!flag) {
        console.log(1)
        setTimeout(function() {
            console.log(2)
            test(true);
        }, 1000);
        return;
    } else console.log(3)
}

test();
```

하지만 너무 어렵다. 프로미스를 배우면 보다 쉽게 구현할 수 있다.

아래와 같다.

```javascript
function test() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(2);
            resolve();
        }, 1000)
    })
}

console.log(1);
test().then(() => {
    console.log(3);
})
```

promise  사용법을 배우고 다시 돌아와서 위 코드를 읽어보면 이해가 될 것이다. 

이제 promise를 배워보자.

다음과 같은 함수가 있다고 해보자

```javascript
function increase(number, callback) {
    setTimeout(() => {
        const result = number + 10;
        callback(result);
    }, 1000)
}
```

위 함수는 첫번째 인자로 숫자를 받고, 1초 뒤 그 숙자에 10을 더해서 두번째 인자인 콜백에 넣어 호출하는 함수이다.

따라서 사용할 때는 다음과 같이 사용하면 된다.

```javascript
increase(0, result => {
	console.log(result);
});
```

만약 10을 찍고 20을 찍고 싶다면?

```javascript
increase(0, result => {
	console.log(result);
    increase(result, result => {
        console.log(result);
    })
});
```

10, 20을 찍고 30을 찍고 싶다면?

```javascript
increase(0, result => {
	console.log(result);
    increase(result, result => {
        console.log(result);
        increase(result, result => {
            console.log(result);
        })
    })
});
```

코드가 지저분하다. 이런 것을 콜백헬이라고 한다.

비동기작업을 동기적으로 하고싶지만 이렇게 하자니 너무 지저분하고 읽기도 어려웠을 것이다.

이런 콜백헬을 개선하기 위해 promise가 등장했다. 위 increase함수를 promise로 바꿔보자.

```javascript
function increase(number) {
    return new Promise((resoleve, reject) => {
        setTimeout(() => {
            const result = number + 10;
            resolve(result);
        }, 1000)
    });
}
```

Promise 객체 안에는 콜백을 넣고 new를 통해 새로운 인스턴스를 만들어 반환해주는 형식으로 작성했다.

프로미스의 콜백은 resolve와 reject라는 두가지 인자를 받는다.

비동기작업을 할 때 해당 비동기 작없ㄷ이 큐에 갔다가 스택에 돌아와서 처리될때까지는 약속을, 즉 resolve를 해주지 않는다. 

만약 그 비동기작업을 하다가 어떤 오류가 나면 reject로 에러를 반환해주고 성공적으로 비동기작업이 끝나면 resolve에 담아서 반환해준다.

따라서 콜백헬을 다음과 같이 개선할 수 있따.

```javascript
increase(0)
	.then(number => {
    	console.log(number);
    	return increase(number);
})
	.then(number => {
    	console.log(number);
   		return increase(number);
})
	.then(number => {
    	console.log(number);
    	return increase(number);
})
	.then(number => {
    	console.log(number);
    	return increase(number);
})
	.then(number => {
    	console.log(number);
    	return increase(number);
})
```

.then으로 구획이 나누어져 있기 때문에 콜백헬에 비해 훨씬 가독성이 좋다.

이제 다시 위로 올라가서 처음에 작성한 코드를 살펴보자