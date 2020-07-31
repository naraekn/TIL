My React Lab - props 편
===

### **Component 에서 props를 전달하는 방법**

```component```의 ```attribute```로 ```props```를 넘겨줄 때와, ```opening/closing tag``` 사이로 ```props```를 넘겨줄 때가 어떻게 다른지에 대해서, 실험하고(?) 나름대로 정리해본다. ~~(맞게 이해한건지는 모르겠음ㅎㅎ..멘토님께 여쭤보자..)~~

1. component의 attribute key 와 attribute value를 통해 props를 전달한 경우, props라는 하나의 객체로 묶여서 선언된 컴포넌트로 전달된다. 이 때, attribute key 값에 맞게 destructuring을 해주면 좋다.
    ```javascript
    //과제 제출 코드
    function Button({ number, onClick }){
      ...
    }
    ```
    ```javascript
    //과제 제출 코드
    return (
      <p>
        {[1, 2, 3, 4, 5].map((i) => (
          <Button
            key={i}
            number={i}
            onClick={onClick}
          />
        ))}
      </p>
    );
    ```
    
2. opening/closing tag 사이로 받은 props의 이름은 ```children``` 이다.
    ```javascript
    //강의 코드
    function Button({ children }) {
      return (
        <button type="button">
          {children}
        </button>
      );
    }

    function Buttons() {
      return (
        {[1, 2, 3].map((i) => (
          <Button key={i}>
            {i}
          </Button>
        ))}
      );
    }
    ```
3. opening/closing tag 사이로 props를 넘겨줄 때는, 하나의 값만 넘겨 줄 수 있다. 
    ```javascript
    //실험1 - 안됨
    return (
      <div>
        <Button key={i}>{ number:i, onClick: onClick }</Button>
        <Button key={i}>{ i, onClick }</Button>
      </div>
    );
    ```

  4. opening/closing tag 사이로 props를 넘길 때, 다른 컴포넌트를 전달해줄 수도 있다.
      ```javascript
      // codesoom.com 홈페이지 코드
      return (
        <>
          ...
          <MainContent>
            <Container>
              ...
            </Container>
          </MainContent>
        </>
      );
      ```

  5. tag의 attribute key 와 attribute value를 통해 props를 전달한 경우, key값에 ```children```은 안쓰는 것이 좋다. (아마도, ```children``` 자체가 javascript의 예약어는 아닌거 같다. 그렇지만, 위의 2번 예시와 헷갈릴 수 있으니 막는 거 같다.)
      ```javascript
      // 실험2 - linter가 경고날림
      function Example({ children }) {
        return (
          <h3>{children}</h3>
        );
      }
      function Examples({ i }) {
        return (
          <div>
            <Example children={i} />
          </div>
        );
      }
      ```
