# 리액트에서 HOC란

고차 컴포넌트(HOC, Higher Order Component)는 **컴포넌트 로직을 재사용하기 위한 React의 고급 기술**이다. 고차 컴포넌트(HOC)는 React API의 일부가 아니며, **React의 구성적 특성에서 나오는 패턴**이다.

*고차 컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수*

```react
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

Redux의 [`connect`](https://github.com/reduxjs/react-redux/blob/master/docs/api/connect.md#connect)와 Relay의 [`createFragmentContainer`](https://relay.dev/docs/v10.1.3/fragment-container/#createfragmentcontainer)와 같은 서드 파티 React 라이브러리에서 흔하게 볼 수 있다.



함수형 컴포넌트가 생기고 hook이 도입된 이후, HOC을 사용하는 경우는 많이 줄어들고 있다. 

HOC이 보통 *클래스형 컴포넌트에서 react life cycle을 고려한 재사용 가능한 로직을 만들기 위해 사용*되기 때문. 

함수형 컴포넌트에서는 거의 대부분 hook으로 대체가 가능하다. 

대표적 HOC으로 소개된 connect만 보아도, [useSelector와 useDispatch](https://jiyoon-park.tistory.com/entry/useSelector-useDispatch로-리덕스에-편하게-접근해보자)를 사용하는게 더 코드를 직관적이고 간편하게 만든다.



### 예제1(함수형 컴포넌트)

1. **간단한 HOC을 만들어보자.**

   컴포넌트 렌더 시에 로딩 상태값을 두고, 로딩이 되지 않았다면 로딩 메세지를 보여주고 로딩이 끝났다면 컨텐츠가 담긴 컴포넌트를 만들어보자. 여러개의 컴포넌트에 해당 로직을 적용하고 싶으니 해당 로직을 공통으로 가지고 있는 HOC을 만든다. 

   ```react
   import React from 'react';
   import Loading from '../components/Loading';
   
   const withLoading = (WrappedComponent) => props => {
     if (props.isLoading) return <Loading/>
     return <WrappedComponent {...props}/>
   }
   
   export default withLoading;
   ```

   HOC은 주로 with를 prefix로 지정해 짓기 때문에 withLoading이라는 HOC을 만들었다.

2. **만든 HOC을 사용해보자.**

   HOC은 `컴포넌트를 인자로 받아 새로운 컴포넌트로 변환해 반환하는 함수`이므로, 함수를 사용하듯 HOC을 사용하기 위해서는 변환하고자 하는 컴포넌트를 인자로 넘겨주면 된다.

   ```react
   import React from 'react';
   import withLoading from '../hoc/withLoading'
   
   const ComponentA = props => {
     ...
   }
   
   export default withLoading(ComponentA);
   ```



### 예제2(클래스형 컴포넌트)

[App.js]

```react
import './App.css';
import TestComponent from './Component/TestComponent';

function App() {
  return (
    <div >
        <h1 className="App-header">
          코딩병원 119
          <TestComponent name="itprogramming119"></TestComponent>
        </h1>
        
    </div>
  );
}

export default App;
```



[TestComponent.js]

```react
import React, { Component } from 'react';
import withChildrenTestComponent from './withChildrenTestComponent';

class TestComponent extends Component {
    render() {
        console.log('2. TestComponent render');
        return(
            <div>props.name : {this.props.name}</div>
        )
    }
}

export default withChildrenTestComponent(TestComponent, 'TestComponent');
```



[withChildrenTestComponent.js]

```react
import React from 'react';
export default function withChildrenTestComponent(InComponent, InComponentName) {
    return class OutComponent extends React.Component {
        componentDidMount() {
            console.log(`3. InComponentName : ${InComponentName}`);
        }

        render() {
            console.log('1. InComponent render');
            return(
                <InComponent {...this.props}></InComponent>
            )
        }
    }
}
```



![img](https://blog.kakaocdn.net/dn/bpbHOO/btrpq7Pw9Xw/a4vOQZd105Mph7qq23WiJ1/img.png)

여러 컴포넌트에 동일하게 적용해야 하는 공통 기능을 코드 중복없이 사용가능





참고)

https://ko.reactjs.org/docs/higher-order-components.html

https://jiyoon-park.tistory.com/entry/React-HOC%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90

https://itprogramming119.tistory.com/entry/React-%EA%B3%A0%EC%B0%A8-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%9E%80

https://narup.tistory.com/274

