---
title: react和typescript项目开发规范
date: 2019-01-28 14:32:37
tags: 'react'
categories: 'react'
---

# 前言

这篇文章只是针对我目前所在团队编码规范的一点建议，不具备普适性。

## 首先是必须遵循的几点

-   优先设置[prettier](https://prettier.io/)风格配置
-   一切与[prettier](https://prettier.io/)不可自定义 format 的配置不兼容的规则都放弃
-   可选的几套 rules （[tslint](https://github.com/palantir/tslint)，[tslint-react](https://github.com/palantir/tslint-react)，[tslint-microsoft-contrib](https://github.com/Microsoft/tslint-microsoft-contrib)，[tslint-config-airbnb](https://github.com/progre/tslint-config-airbnb)，[tslint-config-standard](https://github.com/blakeembrey/tslint-config-standard)，[tslint-eslint-rules](https://github.com/buzinas/tslint-eslint-rules)）
-   暂定使用[tslint](https://github.com/palantir/tslint)，[tslint-react](https://github.com/palantir/tslint-react)为基准，根据团队习惯对规则进行覆盖修改
    <!-- more -->

## 为什么要使用[prettier](https://prettier.io/)

#### 下面的段落引用自作者[原文](https://jlongster.com/A-Prettier-Formatter)

首先它是一个 JavaScript 格式化工具，Prettier 会丢弃掉几乎全部的原始的代码风格，从而保证 JavaScript 代码风格的一致性。跟 ESLint 的不同在于它没有大量的 options 和 rules 需要管理。不过同时有一点也很重要，最终的格式都是确定的。在团队中工作时，很需要减少摩擦，特别是在大型团队里。尽管无法完全避免摩擦，我们更多能做的是通过工具变得更容易在一起协作。你可能觉得配置一下 ESLint 不会消耗太多时间，或者说团队里不会花很多时间争论语法。根据我的经验，实际上不是那么美好。即便你配置了大量的 ESLint 规则，它实际上还是无法捕捉到全部的样式的差异。团队仍然会努力去强制推进一套统一的样式，而这显得很让人分心。语法的细节其实没那么重要。就让 Prettier 这样的工具来做格式和排版就好了，程序员应该关注在那些真正的问题上。用了 Prettier 之后你写代码反而更加自由了，怎么写都可以，因为随后一格式化马上就纠正回来了!语法的细节其实没那么重要。就让 Prettier 这样的工具来做格式和排版就好了，程序员应该关注在那些真正的问题上。

## prettier 的配置

| rule                       | default | desc                     | recommend |
| -------------------------- | ------- | ------------------------ | --------- |
| Print Width                | 80      | 代码一行的最大宽度       | 120       |
| Tab Width                  | 2       | 缩进的空格数             | 4         |
| Tabs                       | false   | 缩进使用 tab             | false     |
| Semicolons                 | true    | 是否加分号               | false     |
| Quotes                     | false   | 使用单引号               | true      |
| JSX Quotes                 | false   | JSX 中使用单引号         | false     |
| Trailing Commas            | "none"  | 是否加尾逗号             | "none"    |
| Arrow Function Parentheses | "avoid" | 箭头函数参数带不带小括号 | "avoid"   |
| End of Line                | "auto"  | 行结尾换行               | “lf”      |

确定好 prettier 的风格后是一些编码的实践建议，涉及到相关的 lint 规则可设置 lint 规则做限制，lint 规则没有的请自觉遵守。

## 模块导入

优先导入第三方模块(node_modules 中的)， 第三方模块导入代码段和业务模块导入代码段之间空一行，import 代码段与之后的业务代码之间空一行，示例如下：

```jsx
import React, { PureComponent } from 'react'
import { Checkbox, Button, Card, Spin } from 'antd'

import VoiceCollect from './VoiceCollect'
import { AudioPlayerProvider } from '../../components/AudioPlayer'
import { FlowTypes } from '../../components/configmodal/ConfigModal'
import * as API from '../../api'
import styles from './index.scss'
import { tips } from '../../common/tools'
import { autoCatch } from '../../decorators'

const CheckboxGroup = Checkbox.Group

type Algorithm = API.alpha.setting.flow.FlowSimpleComponent[]
```

## 如何组织组件文件

![_20181217135904](https://user-images.githubusercontent.com/20181476/50069204-f0d29200-0203-11e9-8fa3-2daa2b1194da.png)

## 导入路径过长则使用 webpack 结合 tsconfig 配置导入别名

```jsx
// bad
import xxx from '../../../../../../components'
// good
import xxx from '@components'
```

## 开启 cssModules

```jsx
import styles from './index.scss'
```

![qq 20181217114453](https://user-images.githubusercontent.com/20181476/50065386-3d60a200-01f1-11e9-9a42-085c25f3ef95.png)
![qq 20181217114622](https://user-images.githubusercontent.com/20181476/50065423-77ca3f00-01f1-11e9-9c4d-5d64efebfa21.png)
![_20181217114636](https://user-images.githubusercontent.com/20181476/50065426-7ac52f80-01f1-11e9-9cc5-725430eefc62.png)
有代码补全，跳到定义，省去 class 命名的烦恼，建议使用

## 组件类型编写

state 和 props 的类型定义写在 class 外

```tsx
// good
type Prop = {}

type State = Readonly<{
    sampleA: string
    sampleB: string
    algorithms: string[]
    submiting: boolean
    result: Array<{ Name: string; IsSame: boolean }>
    audioBlob: Blob
    formIniting: boolean
}>

class SameCheck extends PureComponent<Prop, State> {
    readonly state: State = {
        sampleA: '',
        sampleB: '',
        algorithms: [],
        submiting: false,
        result: [],
        audioBlob: null,
        formIniting: false
    }
}
// bad
class SameCheck extends PureComponent<{}, {}> {}
```

## 优先使用 const 声明

## 没有使用到的模块或变量要删除掉

## 组件成员排布顺序按照 state, 普通成员，生命周期函数，自定义函数，render 函数的顺序编写

```tsx
class SameCheck extends PureComponent<Prop, State> {
    readonly state: State = {
        sampleA: '',
        sampleB: '',
        algorithms: [],
        submiting: false,
        result: [],
        audioBlob: null,
        formIniting: false
    }

    algorithms: Algorithm = []

    componentDidMount() {
        this.getAlgorithmsInfo()
    }

    handleVoiceAChange = (voiceKey: string) => {
        this.setState({ sampleA: voiceKey })
    }
    render() {}
}
```

## 使用箭头函数，不要出现 bind

```tsx
// good
handleClick = () => { }
// bad
handleClick() { }
<div onClick={this.handleClick.bind(this)}></div>
```

## 不在组件内写 constructor 函数，使用更加简洁的写法

```tsx
// bad
class SameCheck extends PureComponent<Prop, State> {
    constructor(props) {
        super(props)
        this.state = {}
    }
}
// good
class SameCheck extends PureComponent<Prop, State> {
    readonly state: State = {}
}
```

## 要开始习惯编写函数组件，为之后的[react-hooks](https://reactjs.org/docs/hooks-intro.html)做准备

## 函数组件基本写法

```tsx
export const SomeCompo: React.SFC<Prop> = props => {
    return <div>xxx</div>
}
```

## style 不涉及变量则都写到 scss 文件中，避免组件代码臃肿

```tsx
// bad
<div style={{ xxx: xxx }} />
```

## 有 JSX 代码的 ts 文件后缀名用.tsx，没有的用.ts

## 不允许直接修改 state，state 应该是只读的

```tsx
// bad
this.state.arr.push(xxx)
this.forceUpdate()
// good
this.setState({ arr: this.state.arr.concat([xxx]) })
```

## 单行注释的//符号与注释内容间空一格

```tsx
//bad
// good
```

## 使用 React.Fragment

```tsx
// bad
<React.Fragment>
    <div></div>
    <h3></h3>
</React.Fragment>
// good
<>
    <div></div>
    <h3></h3>
</>
```

## 不允许使用双等号，请使用恒等

## 单组件代码不能超过 400 行，考虑是否拆分组件

## 单函数代码不超过 50 行

## createRef 使用方式

```tsx
input = createRef<HTMLInputElement>()
render(){
    return <input ref={this.input} />
}
```

## renderProps 模式下的类型编写

```tsx
import React, { PureComponent } from 'react'
import { Modal } from 'antd'

type RenderProps = {
    showModal: () => void
}

type Prop = {
    children(props: RenderProps): JSX.Element
    onOk?: () => void
}

type State = {
    visible: boolean
}

class DemoModal extends PureComponent<Prop, State> {
    readonly state = {
        visible: false
    }

    hide = () => {
        this.setState({ visible: false })
    }

    show = () => {
        this.setState({ visible: true })
    }

    handleOK = () => {
        const { onOk } = this.props
        if (onOk && typeof onOk === 'function') {
            onOk()
        }
        this.hide()
    }

    render() {
        return (
            <>
                <Modal onCancel={this.hide} onOk={this.handleOK} visible={this.state.visible}>
                    适合只有一种方式触发弹层
                </Modal>
                {this.props.children({ showModal: this.show })}
            </>
        )
    }
}

export default DemoModal
```

## connect 过 redux 的组件 types 编写

```tsx
const mapStateToProps = ({ auth: { loading, isLogin } }: RootState) => ({
    loading,
    isLogin
})

const mapDispatchToProps = (dispatch: Dispatch<RootAction>) => ({
    actions: bindActionCreators(authActions, dispatch)
})

interface Prop extends Partial<ReturnType<typeof mapStateToProps>>, Partial<ReturnType<typeof mapDispatchToProps>> {}

type State = Readonly<{}>

class Login extends PureComponent<Prop, State> {}
```

## 尽量使用 renderProps 模式抽离可复用逻辑，高阶组件的 types 编写比较麻烦

### react 中逻辑复用方式推荐度：

minxin（已废除） < extends（不推荐）< [hoc](https://reactjs.org/docs/higher-order-components.html) < [renderProps](https://reactjs.org/docs/render-props.html) < [react hooks](https://reactjs.org/docs/hooks-intro.html)（未正式发版）
