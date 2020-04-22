## <b>styled-component</b>

```
yarn add styled-component
or
npm install styled-component
```

### **GlobalStyle**
전체 component 의 background-color에 다크모드를 적용하고 특정 component 는 제외하는 경우 유용하게 사용할 수 있다.<br>
(className을 적용해야되는 css module은 적용불가하여 해당 방법을 찾아냄)

```
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
	body {
		background-color: white;
        color: black;
	}
`;

function Component() {
    return(
        // 코드 작성
        <GlobalStyle/>
    )   
}
```