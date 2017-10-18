# ReactJSNote
這裡記錄所有從無到有的 ReactJS 學習筆記。ReactJS 是一個由 Facebook 開發出來的前端顯示程序，主要以元件導向開發思維．
## ReactJS 元件撰寫方式分為以下兩種:
1. 使用 ES6 的 Class（可以進行比較複雜的操作和元件生命週期的控制，相對於 stateless components 耗費資源）

	```javascript
	//  注意元件開頭第一個字母都要大寫
	class MyComponent extends React.Component {
		// render 是 Class based 元件唯一必須的方法（method）
		render() {
			return (
				<div>Hello, World!</div>
			);
		}
	}

	// 將 <MyComponent /> 元件插入 id 為 app 的 DOM 元素中
	ReactDOM.render(<MyComponent/>, document.getElementById('app'));
	```

2. 使用 Functional Component 寫法

	```javascript
	// 使用 arrow function 來設計 Functional Component 讓 UI 設計更單純（f(D) => UI），減少副作用（side effect）
	const MyComponent = () => (
		<div>Hello, World!</div>
	);
	
	// 將 <MyComponent /> 元件插入 id 為 app 的 DOM 元素中
	ReactDOM.render(<MyComponent/>, document.getElementById('app'));
	```
單純地 render UI 的 stateless components，__沒有內部狀態、沒有實作物件和 ref，沒有生命週期函數__。若非需要控制生命週期的話建議多使用 stateless components 也就是 Functional Component 寫法能獲得比較好的效能。

## ReactJS JSX 載入方式
- 內嵌

```html
<script type="text/babel">
  ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
  );
</script>
```

- 從外部引入

`<script type="text/jsx" src="main.jsx"></script>` 

## Redux 狀態控制器
Redux 有一個 store 用以儲存所有元件的狀態，經由元件發動的 action 觸發 Reducer 呼叫對映的方法來改變元件狀態，反回最新的狀態。

 Redux 資料流的模型大致上可以簡化成： `View -> Action -> (Middleware) -> Reducer`。
 
 Reducer: 用來儲存所有 action 所對映的動作，範例如下:
 
 ```javascript
import { createStore } from 'redux';

/** 
  下面是一個簡單的 reducers ，主要功能是針對傳進來的 action type 判斷並回傳新的 state
  reducer 規格：(state, action) => newState 
  一般而言 state 可以是 primitive、array 或 object 甚至是 ImmutableJS Data。但要留意的是不能修改到原來的 state ，
  回傳的是新的 state。由於使用在 Redux 中使用 ImmutableJS 有許多好處，所以我們的範例 App 也會使用 ImmutableJS 
*/
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}

// 創建 Redux store 去存放 App 的所有 state
// store 的可用 API { subscribe, dispatch, getState } 
let store = createStore(counter);

// 可以使用 subscribe() 來訂閱 state 是否更新。但實務通常會使用 react-redux 來串連 React 和 Redux
store.subscribe(() =>
  console.log(store.getState());
);

// 若想改變 state ，一律發 action
store.dispatch({ type: 'INCREMENT' });
// 1
store.dispatch({ type: 'INCREMENT' });
// 2
store.dispatch({ type: 'DECREMENT' });
// 1
```


