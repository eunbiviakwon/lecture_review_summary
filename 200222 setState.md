## setState

immer를 배우기 전에 setState에 대해 조금 더 알아봐야 한다.

setState는 두가지 방식으로 사용될 수 있다.

첫번째는 우리가 지금까지 해오던 객체를 집어넣는 방식(state change)이다.

```react
this.setState({
	counter: this.state.counter + 1
})
```

두번째는 setState 안에 함수를 집어넣는 방식(updater function)이다.

```react
this.setState((state) => {
	return {
        number: this.state.number + 1
    }
})
```

첫번째와 두번째 방식에는 간단한 차이가 존재한다.

첫번째 방식은 this.setState를 아무리 많이해도 한번에 묶여서 작동하게 된다.

공식 문서에는 다음과 같이 말한다.

"This form of`setState()`is also asynchronous, and multiple calls during the same cycle may be **batched** together.

Subsequent calls will override values from previous calls in the same cycle, so the quantity will only be incremented once."

즉 number을 1씩 증가시키는 setState를 열번하더라도 batch 형태로 작동하기 때문에 number 값은 1만 증간된다.

그럴때는 위에서 말한 두번째 방식을 쓰거나 this.setState의 두번째 인자로 콜백을 쓰면된다.