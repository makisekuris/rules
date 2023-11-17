# 简单规范和代码实践

- [简单规范和代码实践](#简单规范和代码实践)
  - [通常使用组合式写法（setup + typescript），同时可备选选项式写法](#通常使用组合式写法setup--typescript同时可备选选项式写法)
  - [跟组件名相关的（组件名、文件名及在模板中使用），采用大驼峰 PascalCase 形式](#跟组件名相关的组件名文件名及在模板中使用采用大驼峰-pascalcase-形式)
  - [与父组件紧密耦合的子组件，命名要以父组件名为前缀](#与父组件紧密耦合的子组件命名要以父组件名为前缀)
  - [组件描述应尽量避免使用缩写、拼音等，并将一般化描述放前，特殊化描述放后](#组件描述应尽量避免使用缩写拼音等并将一般化描述放前特殊化描述放后)
  - [props 声明时使用 camelCase](#props-声明时使用-camelcase)
  - [v-if 和 v-for 禁止同时出现在一个组件上](#v-if-和-v-for-禁止同时出现在一个组件上)
  - [除组件文件以外，其他文件名（.ts .js .css .md ）命名为小驼峰命名](#除组件文件以外其他文件名ts-js-css-md-命名为小驼峰命名)
  - [文件存放路径分配](#文件存放路径分配)
  - [布尔值变量应语义化命名](#布尔值变量应语义化命名)
  - [函数命名应函数名统一用动词开头](#函数命名应函数名统一用动词开头)
  - [做好函数与方法的分离，使用函数时应注意副作用方法](#做好函数与方法的分离使用函数时应注意副作用方法)
  - [注意在函数 return 时的引用类型，需要时应做深拷贝后再进行返回](#注意在函数-return-时的引用类型需要时应做深拷贝后再进行返回)
  - [API 请求函数建议根据请求 Method 统一前缀，避免与外层函数混淆。（注：get 请求建议用 fetch 做前缀，避免其他常用冲突）](#api-请求函数建议根据请求-method-统一前缀避免与外层函数混淆注get-请求建议用-fetch-做前缀避免其他常用冲突)
  - [组件内声明的事件回调函数，尽量不要用 onXXX。避免与 props 传入的回调函数混淆](#组件内声明的事件回调函数尽量不要用-onxxx避免与-props-传入的回调函数混淆)
  - [判断条件太长的时候建议进行封装后通过独立的函数或方法进行调用](#判断条件太长的时候建议进行封装后通过独立的函数或方法进行调用)
  - [异步逻辑使用顺序 async/await \> Promise \> callback](#异步逻辑使用顺序-asyncawait--promise--callback)
  - [相互独立的异步时间应当考虑使用 promiseall](#相互独立的异步时间应当考虑使用-promiseall)
  - [有魔法数的场合，多次使用应设定为变量。单次使用至少要标明原因](#有魔法数的场合多次使用应设定为变量单次使用至少要标明原因)
  - [接口类](#接口类)

## 通常使用组合式写法（setup + typescript），同时可备选选项式写法

诸如

```vue
<script setup lang="ts">
//。。。。。。。。。。
</script>
```

另，此处建议配合 vscode 插件`Volar`进行使用

> Volar 是个 VS Code 的插件，个人认为其最大的作用就是解决了 template 的 TS 提示问题。注意，使用它时，要先移除 Vetur，以避免造成冲突。

## 跟组件名相关的（组件名、文件名及在模板中使用），采用大驼峰 PascalCase 形式

> 在 template 中通常将组建名以大驼峰命名引用便于和框架组件进行分离，可直观区分对应的标签信息

```vue
<template>
  <!-- 大驼峰命名 -->
  <Table :user-list="userList" />
</template>

<script setup lang="ts">
import { defineProps, withDefaults } from "vue";

//组件文件、引入名使用大驼峰命名
import Table from "./Table.vue";

interface Props {
  userList: UserInfo[];
}
</script>
```

## 与父组件紧密耦合的子组件，命名要以父组件名为前缀

> 具有明显前后关系的 compontment 尽可能在命名时体现出关系

```vue
<template>
  <Block>
    <BlockTitle> </BlockTitle>
    <SubBlockTitleSlider> </SubBlockTitleSlider>
    <BlockFooterAnimateButton></BlockFooterAnimateButton>
  </Block>
</template>

<script setup lang="ts">
import { defineProps, withDefaults } from "vue";

//组件文件、引入名使用大驼峰命名
import Table from "./Table.vue";

interface Props {
  userList: UserInfo[];
}
</script>
```

## 组件描述应尽量避免使用缩写、拼音等，并将一般化描述放前，特殊化描述放后

> 例：SearchButtonClear、BlockSettingsCheckboxLaunchOnStartup
> 可以接受的缩写（常用且约定俗成的很难引起异义的）： Button -> btn ; string -> str; delete -> del;
> 不好的例子： manager -> man; clear -> clr; current -> crt; delete -> dlt; create -> crt;

## props 声明时使用 camelCase

```vue
<script setup lang="ts">
import { defineProps, withDefaults } from "vue";

defineProps({
  title: string,
  urlClickTo: string,
});
</script>
```

## v-if 和 v-for 禁止同时出现在一个组件上

> 涉及渲染性能，v-if 和 v-for 在同一组件（标签）上会引起不必要的重绘成本

```vue
<template>
  <!-- 避免这种情况发生 -->
  <div v-if="list.length > 0" v-for="item in list">
    {{ item }}
  </div>

  <!-- 修改参考 -->
  <template v-if="list.length > 0">
    <div v-for="item in list">
      {{ item }}
    </div>
  </template>
</template>
```

## 除组件文件以外，其他文件名（.ts .js .css .md ）命名为小驼峰命名

```ts
// GOOD
import MyComponent from "./MyComponent";

import { getSearchText } from "./api/net/search";

// BAD
import MyComponent from "./my-component";
```

## 文件存放路径分配

```bash
.
├── public # 不需要编译的文件
│   ├── favicon.ico
│   ├── manifest.json
│   └── robots.txt
├── src
│   ├── assets # 静态文件
│   │   ├── iconfonts
│   │   └── sprite.png
│   ├── components # 组件
│   │   ├── MyComponent1
│   │   │   ├── index.tsx
│   │   │   └── style.css
│   │   └── MyComponent2.tsx
│   ├── locales # 国际化
│   │   ├── en-US.json
│   │   └── zh-CN.json
│   ├── pages # 页面
│   │   └── Home
│   │       ├── components
│   │       ├── index.tsx
│   │       ├── services.ts # API 请求
│   │       ├── style.css
│   │       └── utils.ts
│   ├── api # 网络请求方法
│   │
│   ├── composables # vue版hook逻辑代码 （需要保留组件和逻辑状态的代码及方法）
│   │
│   ├── utils # 工具函数库 （无状态的工具函数）
│   │
│   ├── store # pinia全局变量定义位置
│   │
│   ├── router # pinia全局变量定义位置
│   │
│   ├── index.tsx
│   ├── style.css
│   └── utils.ts # 公共方法
```

## 布尔值变量应语义化命名

```ts
// 建议：isXXX、shouldXXX、canXXX、hasXXX、XXXable
// GOOD
const { isModalShow, shouldShowModal, canRun, hasReaded, closeable } = { true, false, true, false, false }


// BAD
const { showModal, modalClose } = { true, false }


```

## 函数命名应函数名统一用动词开头

```ts
// GOOD
function showModal() {}
function changeStatus() {}

// BAD
function modalShow() {}
function statusChange() {}
```

## 做好函数与方法的分离，使用函数时应注意副作用方法

```ts
// 作为函数使用时通常在内部要避免改变外界状态，在js/ts语言下实际上函数和方法分的并不是那么开主要依靠个人理解

// GOOD
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];

// BAD
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  //调用了外部变量，需要注意
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

## 注意在函数 return 时的引用类型，需要时应做深拷贝后再进行返回

```ts
// GOOD
const addItemToCart = (cart, item) => {
  // 作为函数使用时通常在内部要避免改变外界状态，在js/ts语言下实际上函数和方法分的并不是那么开主要依靠个人理解
  // return [...cart, { item, date: Date.now() }]; 也可以这么用
  const copyCart = _.cloneDeep(cart);
  copyCart.push({ item, date: Date.now() });
  return copyCart;
};

// BAD
const addItemToCart = (cart, item) => {
  /**
   * cart在此时的array为引用类型，如果执行下面的行为会直接影响到当前所有使用 cart 这个array的元素
   * 在一定情况下会影响需要实现的情况（当然如果只是作为 方法 用来改变改array的内部数据的话，可以无视）
   */

  cart.push({ item, date: Date.now() });
};
```

## API 请求函数建议根据请求 Method 统一前缀，避免与外层函数混淆。（注：get 请求建议用 fetch 做前缀，避免其他常用冲突）

```ts
// GOOD
function submitForm() {
  postSubmitForm(values); // API 请求
}

function getUserInfo() {
  fetchUser(); // API 请求
}

// BAD
function submit() {
  submitForm(values); // API 请求
}
```

## 组件内声明的事件回调函数，尽量不要用 onXXX。避免与 props 传入的回调函数混淆

```ts
// GOOD
function handleSubmit() {
  props.onSubmit();
}
<button onClick={handleSubmit}>Submit</button>;

// BAD
function onSubmit() {
  props.onSubmit();
}
<button onClick={onSubmit}>Submit</button>;
```

## 判断条件太长的时候建议进行封装后通过独立的函数或方法进行调用

主要影响的是观感问题，通常开发中 bool 值嵌套判断超过 20 行或者嵌套三层以上就已经开始影响阅读观感了，所以此处建议条件允许或者能力允许下，尽可能把复杂判断逻辑单拎出来封装一下，并简单写一点注释

```ts
// GOOD
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}

// BAD
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

## 异步逻辑使用顺序 async/await > Promise > callback

```ts
// GOOD
import { get } from "request-promise";
import { writeFile } from "fs-extra";

// async/await
// 异步首选
try {
  const body = await get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin");
  await writeFile("article.html", body);
  console.log("File written");
} catch (error) {
  console.error(error);
}

// promise
// 写复杂了分分钟上百行回调地狱
get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then((body) => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch((err) => {
    console.error(err);
  });

// BAD
// 都什么年代了还在用传统回调？
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, (writeErr) => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

## 相互独立的异步时间应当考虑使用 promiseall

```ts
 //GOOD 请求之间没有依赖关系时应注重同步发出异步
 const init = async () => {
    try {
      //并行请求首屏加载收集报错
      const resList = await Promise.all([
        //需要根据用户登录状态判断使用哪个接口
        errorCatcher(async () =>
          userState.isUserLogin && !loginState.value
            ? (wordItem.value = await getDailyWordPermission())
            : (wordItem.value = await getDailyWord()),
        ),

        //返回数据缺少时间
        errorCatcher(async () => (latestSearchingList.value = await searchingList())),

        //返回数据缺少热度
        errorCatcher(async () => (popularTermList.value = (await popularList(undefined)) ?? [])),

        errorCatcher(async () => {
          const i = await tmVideoList(1, iswe || ismo ? 4 : 12);

          const cacheList = i.records.map((item) => VideoListRecordTranformer(item));
          flowList.value = cacheList;
        }),
      ]);

    } catch (error) {
      console.error(error);
    }

  //BAD 三个请求没有必要先后等待
  const init = async ()=>{
     await searchingList();
     await popularList(undefined);
     await tmVideoList(1, iswe || ismo ? 4 : 12);
  }
```

## 有魔法数的场合，多次使用应设定为变量。单次使用至少要标明原因

> 业务中经常会出现需要定义特定魔法数的情况，单词使用时应注明为什么。多次调用时最好声明为常量进行调用，以免出现改了一处忘了改其他地方的问题

```ts
// BAD
const searchParamCache = ref<{ match: string; years: string }>({
  match: "2",
  years: "",
});

// GOOD

//常量命名使用大写+下划线分割形式
const INIT_MATCH_NUM = "2";

const searchParamCache = ref<{ match: string; years: string }>({
  match: INIT_MATCH_NUM,
  years: "",
});
```

## 接口类

> ID 类，推荐用 string 类型，不推荐 number，超过 17 位的整数 js 会有精度丢失；
> 时间类，推荐用毫秒时间戳（1609459200000），不推荐 string（'2021-01-01 08:00:00'），避免国际化时区造成的麻烦；
> API URL 都以 /api/xxx 开头，其他地址留给前端页面，也方便设置代理；
