**React基础**

React Native 的基础是React， 是在 web 端非常流行的开源 UI 框架。要想掌握 React Native，先了解 React 框架本身是非常有帮助的。本文旨在为初学者介绍一些 react 的入门知识。

本文主要会探讨以下几个 React 的核心概念：

- components 组件
- JSX
- props 属性
- state 状态

如果你想更深一步学习，我们建议你阅读React 的官方文档。

**你的第一个组件**

函数式组件

```jsx
import React from 'react';
import { Text } from 'react-native';

const Cat = () => {
  return (
    <Text>Hello, I am your cat!</Text>
  );
}

export default Cat;
```

Class组件

```jsx
import React, { Component } from 'react';
import { Text } from 'react-native';

class Cat extends Component {
  render() {
    return (
      <Text>Hello, I am your cat!</Text>
    );
  }
}

export default Cat;
```

**JSX**

React 和 React Native 都使用JSX 语法，这种语法使得你可以在 JavaScript 中直接输出元素：<Text>Hello, I am your cat!</Text>。React 的文档有一份完整的JSX 指南可供你参考。因为 JSX 本质上也就是 JavaScript，所以你可以在其中直接使用变量。

**交互示例**

```jsx
import React from 'react';
import { Text } from 'react-native';

const Cat = () => {
  const name = "Maru";
  return (
    <Text>Hello, I am {name}!</Text>
  );
}

export default Cat;
```

括号中可以使用任意 JavaScript 表达式，包括调用函数，例如{getFullName("Rum", Tum", "Tugger")}

**交互示例**

```jsx
import React from 'react';
import { Text } from 'react-native';

const getFullName = (firstName, secondName, thirdName) => {
  return firstName + " " + secondName + " " + thirdName;
}

const Cat = () => {
  return (
    <Text>
      Hello, I am {getFullName("Rum", "Tum", "Tugger")}!
    </Text>
  );
}

export default Cat;
```

**自定义组件**

你应该已经了解React Native 的核心组件了。 React 使得你可以通过嵌套这些组件来创造新组件。这些可嵌套可复用的组件正是 React 理念的精髓。

例如你可以把Text和TextInput嵌入到View 中，React Native 会把它们一起渲染出来：

```jsx
import React from 'react';
import { Text, TextInput, View } from 'react-native';

const Cat = () => {
  return (
    <View>
      <Text>Hello, I am...</Text>
      <TextInput
        style={{
          height: 40,
          borderColor: 'gray',
          borderWidth: 1
        }}
        defaultValue="Name me!"
      />
    </View>
  );
}

export default Cat;
```

这样你就可以在别处通过<Cat>来任意引用这个组件了：

```jsx
import React from 'react';
import { Text, TextInput, View } from 'react-native';

const Cat = () => {
  return (
    <View>
      <Text>I am also a cat!</Text>
    </View>
  );
}

const Cafe = () => {
  return (
    <View>
      <Text>Welcome!</Text>
      <Cat />
      <Cat />
      <Cat />
    </View>
  );
}

export default Cafe;
```

这里Cafe是一个父组件，而每个Cat则是子组件。

**属性**

给每只<Cat>一个不同的name：

```jsx
import React from 'react';
import { Text, View } from 'react-native';

const Cat = (props) => {
  return (
    <View>
      <Text>Hello, I am {props.name}!</Text>
    </View>
  );
}

const Cafe = () => {
  return (
    <View>
      <Cat name="Maru" />
      <Cat name="Jellylorum" />
      <Cat name="Spot" />
    </View>
  );
}

export default Cafe;
```

React Native 的绝大多数核心组件都提供了可定制的 props。例如，在使用Image组件时，你可以给它传递一个source属性，用来指定它显示的内容：

```jsx
import React from 'react';
import { Text, View, Image } from 'react-native';

const CatApp = () => {
  return (
    <View>
      <Image
        source={{uri: "https://reactnative.dev/docs/assets/p_cat1.png"}}
        style={{width: 200, height: 200}}
      />
      <Text>Hello, I am your cat!</Text>
    </View>
  );
}

export default CatApp;
```

**状态**

你可以使用React 的useState Hook来为组件添加状态。Hook （钩子）是一种特殊的函数，可以让你“钩住”一些 React 的特性。例如useState可以在函数组件中添加一个“状态钩子”，在函数组件重新渲染执行的时候能够保持住之前的状态。要了解更多，可以阅读React 中有关 Hook 的文档。

函数式组件

```jsx
import React, { useState } from "react";
import { Button, Text, View } from "react-native";

const Cat = (props) => {
  const [isHungry, setIsHungry] = useState(true);

  return (
    <View>
      <Text>
        I am {props.name}, and I am {isHungry ? "hungry" : "full"}!
      </Text>
      <Button
        onPress={() => {
          setIsHungry(false);
        }}
        disabled={!isHungry}
        title={isHungry ? "Pour me some milk, please!" : "Thank you!"}
      />
    </View>
  );
}

const Cafe = () => {
  return (
    <>
      <Cat name="Munkustrap" />
      <Cat name="Spot" />
    </>
  );
}

export default Cafe;
```

你可以使用useState来记录各种类型的数据： strings, numbers, Booleans, arrays, objects。例如你可以这样来记录猫咪被爱抚的次数：const [timesPetted, setTimesPetted] = useState(0)。useState实质上做了两件事情：

- 创建一个“状态变量”，并赋予一个初始值。上面例子中的状态变量是`isHungry`，初始值为`true`。
- 同时创建一个函数用于设置此状态变量的值——`setIsHungry`。

取什么名字并不重要。但脑海中应该形成这样一种模式：[<取值>, <设值>] = useState(<初始值>)

你可能注意到虽然isHungry使用了常量关键字const，但它看起来还是可以修改！简单来说，当你调用setIsHungry这样的设置状态的函数时，其所在的组件会重新渲染。此处这一整个Cat函数都会从头重新执行一遍。重新执行的时候，useState会返回给我们新设置的值。



Class组件

```jsx
import React, { Component } from "react";
import { Button, Text, View } from "react-native";

class Cat extends Component {
  state = { isHungry: true };

  render() {
    return (
      <View>
        <Text>
          I am {this.props.name}, and I am
          {this.state.isHungry ? " hungry" : " full"}!
        </Text>
        <Button
          onPress={() => {
            this.setState({ isHungry: false });
          }}
          disabled={!this.state.isHungry}
          title={
            this.state.isHungry ? "Pour me some milk, please!" : "Thank you!"
          }
        />
      </View>
    );
  }
}

class Cafe extends Component {
  render() {
    return (
      <>
        <Cat name="Munkustrap" />
        <Cat name="Spot" />
      </>
    );
  }
}

export default  Cafe;
```

不要直接给组件 state 赋值（比如this.state.hunger = false）来修改状态。使用 this.setState() 方法才能让 React 知悉状态的变化，从而触发重渲染。直接修改状态变量可能会使界面无法响应！

注意到上面的<>和</>了吗？ 这一对 JSX 标签称为Fragments（片段）。由于 JSX 的语法要求根元素必须为单个元素，如果我们需要在根节点处并列多个元素，在此前不得不额外套一个没有实际用处的View。但有了 Fragment 后就不需要引入额外的容器视图了。