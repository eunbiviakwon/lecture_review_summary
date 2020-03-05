## async-await

async-awiat를 이용하면 보다 가독성이 높은 코드를 작성할 수 있다.

async-await는 promise와 별개의 것이 아니라 promise 기반 위에서 작동하는 것이다. 

async-await는 다음과 같은 형식으로 이루어진다.

```react
async function asyncFunc(){
	await 첫번째 promise
    await 두번째 promise
    await 세번째 promise
    await 네번째 promise
    ...
}
```

await 키워드를 사용하기 위해서는 async 키워드가 function 앞에 붙어야한다.

또한 await 뒤에는 항상 promise 인스턴스가 와야 한다. 

그렇게 하면 해당 promise가 resolve 값을 반환하기 전까지는 await가 그 다음 진행을 막아준다.

```react
// 비동기작업

setTimeout(() => {
    console.log(1)
}, 1000)
console.log(2)

//결과값: 2 1
```

```react
// promise를 이용해서 비동기를 동기적으로 바꾸면?

function promiseFunc(){
    return new Promise((resolve) => {
        setTimeout(() => {
            console.log(1);
            resolve();
        }, 1000)
    })
}

promiseFunc().then(() => {
    console.log(2);
})

// 결과값: 1 2
```

```react
// async-await를 이용하면?

async function asyncFunc(){
    await promiseFunc();
    console.log(2);
}

결과값: 1 2
```

가독성이 좋은 것을 확인할 수 있다.

async-await를 쓸때는 try-catch로 감싸면 try에서 비동기작업을하다 에러가 날 경우 catch로 잡아줄 수 있다.

```react
async function asyncFunction(){
	try {
        // 네트워크 작업, 소켓요청 등 여러가지 비동기작업을 하다가 에러가 날 경우 catch의 e로 에러인자가 넘어감
    } cathch(e) {
        console.error(e)
    }
}
```

promise를 작성했으면 그걸 async-await로 바꾸는 연습을 해보자

```react
// 지난 시간 jsonplaceholder에 promise로 요청한 것을 async-await로 바꾸면?

import React, { Component } from 'react';
import axios from 'axios';

class App extends Component {
    state = {
        date: null
    };
	
	handleClick = async() => {
        const response = await axios.get(
        	'http://jsonplaceholder.typicodecom/posts'
        );
        this.setState({
            data: response.data
        });
    };

	render() {
        const { data } = this.state;
        return (
        	<div>
            	<button onClick={this.handleClick}>
                	api불러오기
                </button>
                <ul>
                	{data &&
                    	data.map(item => {
                        	return <li key={item.id}>{item.title}</li>;
                    })}
                </ul>
            </div>
        );
    }
}

export default App;
```



