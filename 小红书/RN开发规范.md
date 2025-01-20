## 基础规范

---

统一全部采用 ES6 语法

统一采用 tsx 作为 React 组件的拓展名

使用 jsx 语法

统一全部使用 TypeScript

严格编写类型

严格编写必要的注释

严禁 COPY PASTE，每个人需要为自己写的代码负责（需要能够给后续的维护者讲清楚这段代码为什么这么写，否则就由编写代码的本人维护）！！！！

## 命名 & 实现规范

---

### 组件

**文件夹****名**使用 '-' 进行分割

**组件名**使用大驼峰，禁止直接命名为 index

使用 named export，禁止使用 default import

对于每个组件，尽量将其放置文件夹中，保证其路径的语义，而不是平铺在最外层

组件名应保持其语义，描述清楚其功能，场景等

```JavaScript fold title:javaScript
// good
|-- user-info
	|-- user-name
		|-- UserName.tsx
		|-- style.css
		|-- index.ts
	|-- user-avatar
		|-- UserAvatar.tsx
		|-- index.ts
	UserInfo.tsx
	index.ts
// bad
|-- user-info
	|-- name
		|-- index.tsx
	|-- avatar
		|-- index.tsx
	|-- container // 这是什么 container ？
	UserPanel.tsx
	index.tsx
```

**named export**

更好的可维护性、可扩展性和可读性

```JavaScript fold title:javaScript
// user-name/UserName.tsxexport
const UserName = () => ({ <p>UserName</p> });

// user-name/index.ts
export { UserName } from './UserName'; 

// usage
import { UserName } from '@components/user-info/user-name';
```

**如果你的组件不是通用组件，请不要将其包裹在类似 common 的文件夹内造成误解**

```JavaScript fold title:javaScript
// bad这里只是 user-info 的子组件，并不通用
|-- user-info
	|-- common
		|-- user-name
		|-- user-avatar
```

**如果是通用组件，请根据其抽象程度，放在不同的目录下**

```JavaScript fold title:javaScript
|-- packages
	|-- shared        
		|-- components // 各个 package 可共用的组件
	|-- package1 
		|-- shared 
			|-- components // 一个 package 内，不同页面可共用的组件       
	    |-- page1
			|-- components // 一个页面内，不同组件共用的组件
```

**组件名应保持其语义，描述清楚其功能，场景**

```JavaScript fold title:javaScript
// bad
|-- components
	|-- row // 是通用的 row 还是具体业务特定的 row ？    
	|-- tab
// good
|-- components    
	|-- price-table-row    
	|-- base-tab
```

### 业务工具方法（helper）

**使用****小****驼峰命名**

```JavaScript fold title:javaScript
|-- helpers
	|-- bridgeHelper.ts
	|-- messageHelper.ts
	|-- userHelper.ts
```

**helper 中可以耦合业务逻辑**

```JavaScript fold title:javaScript
export function getUserInfo() {
	const { id, name } = store.getUserInfo();
	return {
		userId: id,
		userName: name,
	}
}
```

**尽量不要在 helper 中互相引用，保持单向引用，防止循环依赖**

```JavaScript fold title:javaScript
// bad
// userHelper
import { getLocation } from 'helpers/locationHelper';
// locationHelper
import { getUserId } from 'helpers/userHelper';
export function getLocation() {
	if (!getUserId()) {
		return;
	}    
	return location;
}
```

### 普通工具方法（utils）

**使用****小****驼峰命名**

```JavaScript fold title:javaScript
|-- utils
	|-- dateUtils
	|-- stringUtils
	|-- layoutUtils
```

**utils 应该是纯函数，不应该糅合业务逻辑与副作用**

```JavaScript fold title:javaScript
// bad
export function parseString(string) {
	const object = JSON.parse(string);
	// 产生了副作用
	store.dispatch({
		type: 'update'
		value: object,
	});    
	return { ...object, id: LS.get('key') }; // 耦合了业务逻辑
}
```

**请为关键的 utils 编写单测**

使用重复度高，若修改容易牵一发而动全身

相比于 helper 等耦合了业务逻辑的方法，utils 更容易写单测

**请在 utils 内部处理好错误并抛到外部**

```JavaScript fold title:javaScript
// good
export const square = (num) => {
	if (typeof num !== 'number' || isNaN(num)) {
		throw new Error('Invalid number');
	} 
	return num * num;
}
```

### 类型

所有页面通用的类型放在统一的文件夹中维护，在 index 中 export 出去

```JavaScript fold title:javaScript
|-- types
	|-- index.ts
	|-- user.ts
	|-- node.ts

// index.ts
export * from './user'
export * from './node'
```

对于一个页面内通用的类型，可放在该页面目录下的 type 文件中

```JavaScript fold title:javaScript
|-- card-list
	|-- component
		|-- card-title
			|-- CardTitle.tsx
	|-- CardList.tsx
	|-- type.ts
// type.ts
export const enum CARD_TYPE {
	SIMPLE = 'SIMPLE'
	VERTICAL = 'VERTICAL'
}
// CardTitle & CardList
import { CARD_TYPE } from '../type'
```

针对组件的 props，可用组件名 + I 前缀为类型名，组件的类型放在组件里维护

```JavaScript fold title:javaScript
export interface ICardItem {
	title: string;
	subTitle: string;
}
export const CardItem = memo((props: ICardItem) =>  {});
```

#### 枚举

定义枚举时，若使用场景较少，长度较短且不需要做对象操作，推荐定义 const enum，对于这种枚举，**会被编译成字符串，减少包体积占用**

```JavaScript fold title:javaScript
export const enum LIST_TYPE {
	LIST = 'list',
	GRID = 'grid',
}
```

![](https://xhs-doc.xhscdn.com/1040025030tfhsigrj009pjn318?imageView2/2/w/1600)

#### Any

==宁愿不写类型，也不要写一个 Any 上去，在 CR 阶段人工严格卡点==

### 样式

对于样式内容较多的情况下，单独新建一个 styles.ts，内容较少的情况下可以直接写在组件内

禁用 @xhs/rex-style，如果必须使用请处理好代码飘红问题

✅ 推荐规范（这样写的好处是有代码提示，且简洁）

```JavaScript fold title:javaScript
export const styles = StyleSheet.create({
	container: {
		flex: 1,
	}
})
```

❌ 不推荐

```JavaScript fold title:javaScript
// 严格禁止这种写法
const container = {
flex: 1,
}
export default StyleSheet.create({
	container,
});
// 最好不要这么写
const container: ViewStyle = {
	flex: 1,
}
export default StyleSheet.create({  container,});
```

## 格式规范

---

### 标签

在自闭符号和标签之前留一个空格

没有子组件的父组件使用自闭标签

如果组件有多行属性，闭合标签应该写在新的一行上

Demo

```JavaScript fold title:javaScript
// Good ✅
<Foo /> 
<Foo   
  bar1="bar1"
  bar2="bar2"
/> 
// Bad ❌
<Foo
/> 
<Foo/>
<Foo   
  bar1="bar1"  
  bar2="bar2"/>
```

### 代码顺序

应做好方法和变量的分类放置，避免穿插放置

应做好 hook 的分类放置，避免穿插放置

Demo

```JavaScript fold title:javaScript
// Good ✅
const App = () => {
	const [count, setCount] = useState(0)
	const isLoading = useRef(false)
	
	const handleClick = useCallback(() => {
		console.log('点击')
	}, [])
	
	const handleClickCart = useCallback(() => {
		console.log('点击')
	}, [])
	useEffect(() => { 
		console.log('执行')
	}, []) 
	return (
		<div> 
		<h1>RN 研发规范</h1>
		</div>
	)
} 
// Bad ❌
const App = () => {
	const [count, setCount] = useState(0)
	useEffect(() => {
		console.log('执行')
	}, [])       
	const handleClick = () => {
		console.log('点击')
		setCount(10) 
	}     
	const isLoading = useRef(false)
	
	const handleClickCart = useCallback(() => { 
		console.log('点击')
	}, [])
	return (
		<div> 
			<h1>RN 研发规范</h1>
		</div> 
	)
}
```

## 编码技巧

---

### 编写 Relinx 类型

完整 case 可以参考 ./packages/lancer-slim/src/containers/pay-success

```JavaScript fold title:javaScript
import { createModel } from '@shared-package/helpers/relinx';
```

**首先定义 Model 的类型，包括 names，state，reducers 以及 effects**

```javaScript fold title:javaScript
export type AppModelType = {
	// 务必要加上 names，并且保证这个 names 和你在 Relinx 中注册的 key 一致！
	name: 'app';
	state: {
		// 在这里定义 state 的类型
	};  
	reducers: { 
		// 在这里定义 reducers 的 payload 类型
		// 例如，我有一个 action 叫 add，他的 payload 是 { count: number }    
		add: {      
		count: number;
		};
		// 当然，reducer 也可以没有 payload，直接传入 undefined 
		toggle: undefined;
	}; 
	effects: {   
	// 与 reducers 同理，定义 effects 的 payload 类型
	fetch: {
		id: string;
	};
. };
};
```

**将所有页面的 ModelType 类型放在一个公共类型中，通常叫做 ModelTypes：**

**为什么要这么做？**

因为在同一个 Relinx Store 实例下的所有页面，他们之间的类型是可以互相访问的。例如在 A 页面中，我们可以调用 B 页面的 action，或者访问 B 页面的 state。

为了实现这一层，我们需要把所有页面的 Type 放在一起，这样才可以让 dispatch 以及 getState 拿到正确的 Types。

**在实践中，我们通常是以一个 Halation 页面为一个 Store 实例，所有注册了 Relinx 的页面的 Types 都应该放在一起。**

**不过，也不要把所有 Types 全部放在一个全局的 Types 定义里面（现在存量代码就是这么做的），因为不同 Store 实例之间是不共享的。**

这里有一个最佳实践可以参考 ./packages/lancer-slim/src/containers/pay-success

定义一个 ModelTypes，并且将你刚才创建的类型通过 extends 的方法添加进去

```JavaScript fold title:javaScript
// 引入另一个工具类型，declareType
import { declareType } from '@shared-package/helpers/relinx';
// ModelTypes 就是公共类型，它可以 extends 多个页面的 ModelType
export interface ModelTypes
	extends declareType<AppModelType, ModelTypes> {}
// extends 多个 types 的例子：
export interface ModelTypes
		extends declareType<AppModelType, ModelTypes>,
			declareType<RecommendModelType,ModelTypes>,
			declareType<VendorsModelType, ModelTypes>,
			declareType<GroupChatsModelType, ModelTypes> {}
```

**Why makes so COMPLICATED???**

Well，其实也没有那么复杂，如果你想明白具体的实现，其实上面的写法等同于下面

```JavaScript fold title:javaScript
// 不使用 declareType 的写法，你确定要自己写吗？
export interface ModelTypes {
	app: {
		state: AppModelType['state'];
	reducers: {
		add: (state: AppModelType['state'],
		payload: AppModelType['reducers']['add']) =>  Partial<AppModelType['state']>
};
// ...
}
}
```

**使用 createModel 工具函数创建 Model**

这一步你就很熟悉了，和之前的一致，**其中 AppModelType 是第一步创建的，ModelTypes 是第二步创建的**

```JavaScript fold title:javaScript
export default () => createModel<AppModelType, ModelTypes>({
	state: {
	},
	reducers: {
	},
	effects: {
	},
	// ...
}); 
```

你会发现，所有的 state，dispatch 都有类型了！是的， even 在你创建 Model 的时候！

**在 View 层使用**

```JavaScript fold title:javaScript
// ModelTypes 是第二步创建的公共类型，'app' 是你在第一步创建的时候定义的那个 name
const [state, dispatch] = useRelinx<ModelTypes, 'app'>('app');
```

**务必，在定义的时候 name 和 Relinx 注册的 key 保持一致!!**

```JavaScript fold title:javaScript
// halation-state.ts
export default [
	{    
	name: 'app',
	 // !!! 这个 key 一定要一致啊啊啊啊啊啊啊
    key: 'app',
	type: 'block',
    parent: null,
    },
    // ...
]
```

现在无论在哪，你的 Model 中的数据都会带上类型了！

### Hook 使用

**使用 usePersist 代替会造成 rerender 的 useCallback**

```JavaScript fold title:javaScript
const onClick = useCallback(() => {    
	setCount(count + 1);}, [count]);
// count 改变， onClick 改变，Child Rerender
<Child onClick={onClick} /> 
// onClick 引用不变，且能拿到最新值，
const onClick = usePersistFn(() => {
	setCount(count + 1);
});
// Child不变<Child onClick={onClick} />
```

**不要使用无用的 useCallback/useMemo，配合子组件 memo 使用，不然就是负优化**

```JavaScript fold title:javaScript
// bad
const Child = ({ title }) => {} 

const Parent = () => {    
	const [count, setCount] = 0    
	const title = useMemo(() => 'hello', [])
	// title没变，count变化，Child 仍然 Rerender，且浪费了 memo 性能
	<Child title={title} />
}  

// good
const Child = React.memo(({ title }) => {})
```

### 图片资源