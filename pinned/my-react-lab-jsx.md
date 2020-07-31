My React Lab - JSX 편
===

### **"JSX는 어떻게 렌더링 되는가"**

1주차 강의 때, React 없이 ```createElement``` 함수를 만들어서, DOM을 조작하고 JSX문법을 쓰는 것을 보고 신기하면서도 (?) 그 때는 아무 것도 몰랐기 때문에 '그러려니..' 하면서 강의를 봤다. 하지만, 시간이 지날 수록 어떻게 JSX가 javascript 형태로 바뀌는지, React와는 어떤 관계인지 너무 궁금해졌다. 

그리고, 최근 며칠동안 고민해보고 어느 정도 깨닫게 된 것(!)이 있어서, 이를 정리하기로 한다. 의문을 해결하는 방법은 다음과 같다.

 1) ```my-react-lab``` 프로젝트에서 다양한 코드들을 실행시켜보고, 제대로 실행되는지 확인한다.
 2) babel 공식 홈페이지에서 제공해주는 'Try it out' 기능을 통해, 다양한 코드들이 어떻게 변환되는지 관찰한다.

### [실험 재료]
* **코드1**
  ```javascript
  /* @jsx createElement */

  function createElement(tagName, props, ...children) {
    const element = document.createElement(tagName);
    Object.entries(props || {}).forEach(([key, value]) => {
      element[key.toLowerCase()] = value;
    });

    children.flat().forEach((child) => {
      if (child instanceof Node) {
        element.appendChild(child);
        return;
      }
      element.appendChild(document.createTextNode(child));
    });

    return element;
  }

  function setElement(count) {
    return (
      <div id="hello" className="greeting">
        <h1>Hello!</h1>
        <p>{ count }</p>
      </div>
    );
  }

  const count = 0;

  function render() {

    const element = setElement(count);

    document.getElementById('app').textContent = '';
    document.getElementById('app').appendChild(element);
  }

  render();
  ```

* **코드2**
  ```javascript
  import React from 'react';
  import ReactDOM from 'react-dom';

  function App() {
    return (
      <h1>hello!</h1>
    );
  }

  ReactDOM.render(<App />, document.getElementById('app'));
  ```
### [실험 내용]
**<my-react-lab에서 실행시켜보기>**

1. 코드1에서 ```/* @jsx createElement */``` 없애보기
2. 코드1에서 ```/* @jsx iAmHungry */``` 로 바꿔보기
3. 코드1에서 ```/* @jsx iAmHungry */```로 바꾸고, ```function iAmHungry```로 함수이름까지 통일시키기
4. 코드1에서 ```createElement``` 함수 내용을 다음과 같이 바꿔보기
  ```javascript
  function createHahaha(tagName, props, ...children) {
    const element = document.createElement('p');
    element.appendChild(document.createTextNode('I changed this function!'));

    return element;
  }
  ```

**<babel try it out 관찰해보기>**

5. 코드1에서 ```/* @jsx createElement */``` 없애보기
6. 코드2에서 어떻게 변환되나 확인해보기


### [실험 결과]
1. 실행 안됨.
2. 실행 안됨.
3. 실행 됨.
4. element로 정의한 JSX 문법대로 나오는 것이 아니라, "I changed this function!" 만 나옴.

5. 원래는 JSX 문법이 'createElement' 함수로 변환 되었는데, ```/* @jsx createElement */``` 없애니까 'React.createElement'로 바꼈다. 즉, react를 필요로 하게 되었다.
6. JSX 부분만 'React.createElement'로 바꼈다.

### [실험 결론]
1. JSX는 React에 의존하지 않는다.
2. JSX 문법을 개발자가 원하는 형태로 바꿔줄 수 있다.
3. JSX 문법을 개발자가 원하는 형태로 바꿔주기 위해서는, 한개의 함수를 정의해줘야 한다.
4. 함수의 이름 역시, 개발자가 원하는대로 지을 수 있다.
5. babel에게 JSX를 바꿔주는 함수가 어떤 함수인지 알려주기 위해서는 ```/* @jsx functionName */``` 과 같은 코드를 작성해줘야 한다.
6. React에서는 자체적으로 JSX를 변환하도록 되어있다. 
7. React에서 JSX를 변환해주는 함수는 ```React.createElement```이다.

-----
아 드디어 정리하고 싶은 것을 오늘 정리했다. 속이 다 시원하다!!!!

