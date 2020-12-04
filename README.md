# 뷰 튜토리얼 학습.

## 시작하기

1. 뷰 객체를 선언하고 데이터 바인딩을 할 수 있다.
2. 디렉티브 v-bind를 통해 특수 속성을 사용할 수 있다.
3. vue 객체를 담은 변수를 통해 데이터 값을 변경할 수 있으며, 변경된 데이터는 html에 적용 된다. (+뷰객체로 바인딩된 데이터를 추가할 수 있다.<<에러뜸 추적이 안된다는듯)

### 조건문과 반복문

1. v-if/v-for 디렉티브를 통한 조건/반복문
2. 디렉티브를 활용하면 DOM 구조에도 데이터 바인딩이 가능함을 보여줌.

### 사용자 입력 핸들링

1. v-on 디렉티브를 통해 상호작용 이벤트 리스너 등록 가능 "기능"을 위해 뷰 인스턴스에 methods = {} 속성 지원
2. v-model 디렉티브로 사용자 입력으로 자스 데이터가 변경되는 양방향 바인딩 지원

### 컴포넌트를 사용한 작성방법

1. Vue객체를 통해 컴포넌트를 등록할 수 있다.
2. 등록한 컴포넌트에 props 속성을 통해 하위 컴포넌트에게 데이터를 주입할 수 있다.

## Vue 인스턴스

### Vue 인스턴스 만들기

1. 모든 Vue 앱은 Vue 함수로 새 Vue 인스턴스를 만드는 것부터 시작
2. 따지고보면 MVVM 패턴과 관련이 없지만 Vue의 패턴은 부분적으로 MVVM패턴에 영감을 받았으며, 관례적으로 Vue인스턴스를 참조하기 위해 종종 변수 vm(ViewModel)을 사용함.
3. Vue 인스턴스 생성 시 options 객체를 전달해야함. 필요에 따라 선택적으로 옵션 사항 작성!
4. Vue 앱은 new Vue 를 통해 만들어진 루트 Vue 인스턴스로 구성되며 선택적으로 중첩이 가능하고 재사용 가능한 컴포넌트 트리로 구성됨.

### 데이터와 메소드

1. Vue 인스턴스가 생성될 때 data 객체에 있는 모든 속성이 Vue의 반응형 시스템에 추가됩니다. 각 속성값이 변경될 때 뷰가 "반응"하여 새로운 값과 일치하도록 업데이트 됩니다.

```js
var data = {a: 1};
var vm = new Vue({
    data: data
});
// 인스턴스에 있는 속성은 원본 데이터에도 영향을 미칩니다.
vm.a == data.a  // => true
vm.a = 2
console.log(data.a) // => 2

// ... 반대로 마찬가지입니다.
data.a = 3
vm.a //=>3
```

2. 데이터가 변경되면 화면은 다시 렌더링 됩니다. Vue객체 생성 당시에 data 에 선언한 속성들만 반응형 다음과 같이 새 속성을 추가하면

```js
vm.b = 'hi';
```

b가 변경되어도 화면이 갱신되지 않습니다. 어떤 속성이 나중에 필요하다는 것을 알고 있으며, 빈 값이거나 존재하지 않은 상태로 시작한다면 아래와 같이 초기값을 지정할 필요가 있습니다.

```js
data : {
    newTodoText : '',
        visitCount
:
    0,
        hideCompletedTodos
:
    false,
        todos
:
    [].error
:
    null
}
```

여기에서 유일한 예외는 Object.freeze()를 사용하는 경우입니다. 이는 기존 속성이 변경되는 것을 막아 반응성 시스템이 추적할 수 없다는 것을 의미합니다.

2. Vue인스턴스는 데이터 속성 이외에도 유용한 인스턴스 속성 및 메소드를 제공합니다. 다른 사용자 정의 속성과 구분하기 위해 $접두어를 붙여 구현해두었습니다.

```js
var data = {a: 1};
var vm = new Vue({
    el: '#example',
    data: data
});

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true
vm.$watch('a', function (newVal, oldVal) {
    // 'vm.a'가 변경되면 호출 됩니다.
})
```

### 인스턴스 라이프사이클 훅

1. 각 Vue 인스턴스는 생성될 때 일련의 초기화 단계를 거칩니다. 데이터 관찰 설정이 필요한 경우, 템플릿을 컴파일하는 경우, 인스턴스를 DOM에 마운트하는 경우, 그리고 데이터가 변경되어 DOM을 업데이트하는
   경우 그 과정에서 사용자 정의 로직을 실행할 수 있는 라이프사이클 훅 도 호출됩니다. 예를 들어 created 훅은 인스턴스가 생성된 후에 호출됩니다.

```js
new Vue({
    data: {
        a: 1
    },
    created: function () {
        // 'this' 는 vm 인스턴스를 가리킵니다.
        console.log('a is:' + this.a)
    }
})
// => "a is : 1"
```

인스턴스 라이프사이클의 여러 단계에서 호출될 다른 훅도 있습니다. 그 예로 mounted, updated 및 destroyed가 있습니다. 모든 라이프사이클 훅은 this 컨텍스트가 호출하는 Vue 인스턴스를
가리키며 호출됩니다. Vue 세계에서 "컨트롤러"의 컨셉이 어디에 있는지 궁금할 수 있습니다. 답은 컨트롤러가 없습니다. 컴포넌트의 사용자 지정 로직은 이러한 라이프사이클 훅으로 분할됩니다.
( options 속성이나 콜백에 화살표 함수 금지! 화살표 함수는 this를 갖지않음! )

## 템플릿 문법

### 보간법

#### 문자열

1. 데이터 바인딩의 가장 기본 형태는 "Mustache" 구문(이중 중괄호)을 사용한 텍스트 보간입니다.

```html
<span>메시지: {{ msg }}</span>
```

Mustache 태그는 해당 데이터 객체의 msg 속성 값으로 대체됩니다. 또한 데이터 객체의 msg 속성이 변경될 때 마다 갱신됩니다.

2. v-once 디렉티브를 사용하여 데이터 변경 시 업데이트 되지 않는 일회성 보간을 수행할 수 있지만, 같은 노드의 바인딩에도 영향을 미친다는 점을 유의해야 합니다.

```html
<span v-once>다시는 변경하지 않습니다.: {{ msg }}</span>
```

#### 원시 HTML

1. 이중 중괄호(mustache)는 HTML이 아닌 일반 텍스트로 데이터를 해석합니다. 실제 HTML을 출력하려면 v-html 디렉티브를 사용해야 합니다.

```html
<p>Using mustaches: {{ rawHtml }}</p> <!--태그가 출력됨-->
<p>Using v-html directive: <span v-html="rawHtml"></span></p> <!-- 렌더링거친 데이터가 출력됨-->
```

* v-html디렉티브 사용된 span의 내용은 rawHtml로 대체됩니다. 이 때 데이터 바인딩은 무시되는데 이유는 Vue가 문자열 기반 템플릿 엔진이 아니기 때문에 v-html 디렉티브를 이용한다면
  mustache 보간 즉 템플릿 문법을 사용할 수 없습니다. 이와 달리 컴포넌트는 UI 재사용 및 구성을 위한 기본단위로 사용하는 것을 추천합니다.

+ 추가로 v-html은 XSS취약점이기 때문에 신뢰할 수 있는(문제없는) 콘텐츠에만 사용하고 절대 사용하면 안된다.

#### 속성

1. Mustaches는 HTML 속성에서 사용할 수 없습니다. 대신 v-bind 디렉티브를 사용하세요.

```html

<div id="{{dynamicId}}"></div> <!-- 불가능함 -->
<div v-bind:id="dynamicId"></div> <!-- 가능함 -->
```

2. boolean 속성을 사용할 때 단순히 true인 경우 v-bind는 조금 다르게 작동한다.

```html

<button v-bind:disabled="isButtonDisabled">Button</button>
```

isButtonDisabled가 null, undefined 또는 false 값을 가지면 disabled 속성은 렌더링된 button엘리멘트에 포함되지 않는다.

### JavaScript 표현식 사용

1. 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원합니다.

```html
{{ number + 1 }}
{{ ok ? 'YES': 'NO' }}
{{ message.split(''}.reverse().join('') }}

<div v-bind:id="'list-'+ id"></div>
```

* 바인딩 하나에 단일 표현식만 포함될 수 있으므로 아래처럼 작성하면 안된다.

```html
<!-- 아래는 구문입니다. 표현식이 아닙니다.-->
{{ var a = 1 }}
<!-- 조건문은 작동하지 않습니다. 삼항 연산자를 사용해야 합니다.-->
{{ if (ok) { return message} }
```

* 템플릿 표현식에서 사용자 정의 전역에 엑세스 불가능함. 빌트인 라이브러리는 접근 가능.

### 디렉티브

- 디렉티브는 v- 접두사가 있는 특수 속성입니다. 디렉티브 속성 값은 단일 JavaScript 표현식이 됩니다. (나중에 설명할 v-for은 예외입니다.) 디렉티브의 역할은 표현식의 값이 변경될 때 사이드이펙트를
  반응적으로 DOM에 적용하는 것 입니다.

```html
<p v-if="seen">이제 나를 볼 수 있어요</p>
```

여기서 v-if 디렉티브는 seen표현의 참/거짓에 기반하여 <p> 엘리멘트를 제거 또는 생성 합니다.

#### 전달인자

1. 일부 디렉티브는 콜론으로 표시되는 "전달인자"를 사용할 수 있습니다. 예를 들어, v-bind 디렉티브는 반응적으로 HTML 속성을 갱신하는데 사용됩니다.

```html
<a v-bind:href="url">...</a>
```

여기서 href는 전달인자로, 엘리먼트의 href속성을 표현식 url의 값에 바인드하는 v-bind 디렉티브에게 알려줍니다. 또 다른 예로 DOM 이벤트를 수신하는 v-on 디렉티브 입니다.

```html
<a v-on:click="doSomething">...</a>
```

전달인자는 이벤트를 받을 이름입니다. 우리는 이벤트 핸들링에 대해 더 자세하게 살펴 볼 것입니다.

#### 동적 전달인자.

2.6.0 버전에서 추가됨. 2.6.0 버전부터 JavaScript 표현식을 대괄호로 묶어 디렉티브의 야규멘트로 사용하는것도 가능해졌습니다.

```html
<!--
동적 전달인자는 "동적 전달인자의 형식 제약"의 부분에서 후술되는바와 같이, 조금의 제약이 있는 점에 주의해주세요.
-->
<a v-bind:[attributeName]="url">...</a>
```

여기서 attributeName은 JavaScript형식으로 동적 변환되어, 그 변환결과가 전달인자의 최종적인 벨류로 사용됩니다. 예를들어 당신의 Vue 인스턴스에 href 라는 값을 가진 attributeName
데이터 속성을 가진 경우 이 바인딩은 v-bind:href 와 동등합니다. 이와 유사하게, 동적인 이벤트명에 핸들러를 바인딩할 때 동적 전달인자를 활용할 수 있습니다.

```html
<a v-on:[eventName]="doSomething">...</a>
```

이 예시에서 eventName 의 값이 "focus"라고 한다면 v-on:[EventName]은 v-on:foucs와 동일합니다.

__동적 전달인자 값의 제약__

동적 전달인자는, null을 제외하고는 string으로 변환될 것으로 예상합니다. 특수 값인 null은 명시적으로 바인딩을 제거하는데 사용 됩니다. 그 외의 경우, string이 아닌 값은 경고를 출력합니다.

__동적 전달인자 형식 제약__

동적 전달인자의 형식에는 문자상의 제약이 있습니다. 스페이스와 따옴표같은 몇몇 문자는 HTML의 속성명으로서 적합하지 않은 문자이기 때문입니다. 다음 예씨는 잘못된 경우입니다.

```html
<a v-bind:['foo' + bar]="value">...</a>
```

이를 피하는 방법은, 스페이스나 따옴표를 포함하지 않는 형식을 사용하거나, 복잡한 표현식을 계솬된 속성(Computed)으로 대체하는 것입니다. in-DOM템플릿을 사용할 때에는 (템플릿이 HTML파일에 직접 쓰여진
경우) 브라우저가 모든 속셩명을 소문자로 만드는 관계로 대문자 사용을 피하는 것이 좋습니다.

```html
<!--
in-DOM 템플릿에서는 이 부분이 v-bind:[someattr]로 변환됩니다.
인스턴스에 "someattr"속성이 없는 경우, 이 코드는 동작하지 않습니다.
-->
<a v-bind:[someAttr]="value"">...</a>
```

#### 수식어

수식어는 점으로 표시되는 특수 접미사로, 디렉티브를 특별한 방법으로 바인딩 해야 함을 나타냅니다. 예를 들어, .prevent 수식어는 트리거된 이벤트에서 event.preventDefault()를 호출하도록
v-on디렉티브에게 알려줍니다.

 ```html

<form v-on:submit.prevent="onSubmit">...</form>
```

나중에 v-on과 v-model을 더 자세히 살펴볼 때 수식어를 더 많이 사용할 것 입니다.

### 약어

v- 접두사는 템플릿의 Vue 특정 속성을 식별하기 위한 시각적인 신호 역할을 합니다. 이 기능은 Vue.js를 사용하여 기존의 마으컵에 동적인 동작을 적용할 떄 유용하지만 일부 자주 사용되는 디렉티브에 대해 너무
장황하다고 느껴질 수 있습니다. 동시에 Vue.js가 모든 템플릿을 관리하는 SPA v- 접두어의 필요성이 떨어집니다. 따라서 가장 자주 사용되는 두개의 디렉티브인 v-bind와 v-on에 대해 특별한 약어를
제공합니다.

#### v-bind 약어

```html
<!-- 전체 문법 -->
<a v-bind:href="url">...</a>
<a :href="url">...</a>
<a :[key]="url">...</a>
```

#### v-on 약어

```html
<a v-on:click="doSomething">...</a>
<a @click="doSomething">...</a>
<a @[event]="doSomething">...</a>
```

이들은 일반적인 HTML과 조금 다르게 보일 수 있습니다. 하지만 :와 @ 속성 이름에 유효한 문자이며 Vue.js를 지원하는 모든 브라우저는 올바르게 구문 분석을 할 수 있습니다. 또한 최종 렌더링 된 마크업에는
나타나지 않습니다. 약어는 완전히 선택사항이지만 나중에 익숙해지면 편할 것 입니다.

## computed와 watch

템플릿 내에 표현식을 넣으면 편리합니다. 하지만 간단한 연산일 때만 이용하는 것이 좋습니다. 너무 많은 연산을 템플릿 안에서 하면 코드가 비대해지고 유지보수가 어렵습니다.

```html

<div id="example">
    {{ message.split(''}.reverse().join('') }
</div>
```

이 템플릿은 더 이상 간단하고 명료하지 않습니다. message를 역순으로 표시한다는 것을 알려면 찬찬히 살펴봐야 합니다. 템플릿에 역순할 일이 더 많아진다면 힘듬 복잡한 로직이라면 반드시 computed 속성을
사용해야 하는 이유입니다.

#### 기본 예제

```html

<div id="example">
    <p>원본 메세지 : "{{ message }}"</p>
    <p>역순으로 표시한 메시지 : "{{ reverseMessage }}"</p>
</div>
```

```js
var vm = new Vue({
    el: "#example",
    data: {
        message: "안녕하세요"
    },
    computed: {
        // 계산된 getter
        reverseMessage: function () {
            // 'this'는 vm 인스턴스를 가리킵니다.
            return this.message.split('').reverse().join('')
        }
    }
});
```

#### computed 속성의 캐싱 vs 메소드

표현식에서 메소드를 호출하여 같은 결과를 얻을 수 도있습니다.

```html
<p>뒤집힌 메시지: "{{ reversedMessage() }}"</p>
```

```js
// 컴포넌트 내부
methods: {
    reversedMessage: function () {
        return this.message.split('').reverse().join('')
    }
}
```

computed 속성 대신 메소드와 같은 함수를 정의할 수도 있습니다. 최종 결과에 대해 두 가지 접근 방식은 서로 동일합니다. 차이점은 computed 속성은 종속 대사을 따라 저장(캐싱)된다는 것 입니다.
computed 속성은 해당 속성이 종속된 대상이 변경될 때만 함수를 실행합니다. 즉 message가 변경되지 않는 한 computed 속성인 reverseMessage를 여러 번 요청해도 계산을 다시 하지 않고
계산되어 있던 결과를 즉시 반환합니다. 또한 Date.now()처럼 아무 곳에도 의존하지 않는 computed 속성의 경우 절대로 업데이트되지 않는다는 뜻입니다.

```js
computed: {
    now: function () {
        return Date.now()
    }
}
```

이에 비해 메소드를 호출하면 렌더링을 다시 할 때마다 항상 함수를 실행합니다.

캐싱이 왜 필요할까요? 계산에 시간이 많이 걸리는 computed 속성인 A를 가지고 있다고 해봅시다. 이 속성을 계산하려면 거대한 배열을 반복해서 다루고 많은 계산을 해야합니다. 그런데 A 에 의존하는 다른
computed 속성 값도 있을 수 있습니다. 캐싱을 하지 않으면 A의 getter함수를 꼭 필요한 것보다 더 많이 실행하게 됩니다! 캐싱을 원하지 않는 경우 메소드를 사용하십시오.

#### computed 속성 vs watch 속성

Vue는 Vue인스턴스의 데이터 변경을 관찰하고 이에 반응하는 보다 일반적인 watch 속성을 제공합니다. 다른 데이터 기반으로 변경할 필요가 있는 데이터가 있는 경우, 특히 AngularJS를 사용하던 경우
watch를 남용하는 경우가 있습니다. 하지만 명령적인 watch 콜백보다 computed 속성을 사용하는 것이 더 좋습니다. (역자 주: watch 속성은 감시할 데이터를 지정하고 그 데이터가 바뀌면 이런 함수를
실행하라는 방식으로 소프트웨어 공학에서 이야기하는 '명령형 프로그래밍'방식. computed속성은 계산해야 하는 목표 데이터를 정의하는 방식으로 소프트웨어 공학에서 이야기하는 '선언형 프로그래밍'방식 )

```html

<div id="demo">{{ fullName }}</div>
```

```js
var vm = new Vue({
    el: "#demo",
    data: {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
    },
    watch: {
        firstName: function (val) {
            this.fullName = val + ' ' + this.lastName
        },
        lastName: function (val) {
            this.fullName = this.firstName + ' ' + val
        }
    }
})
```

위의 코드는 명령형이고 또 코드를 반복합니다. computed속성을 사용하는 방식과 비교해 보세요.

```js

var vm = new Vue({
    el: '#demo',
    data: {
        firstName: 'Foo',
        lastName: 'Bar'
    },
    computed: {
        fullName: function () {
            return this.firstName + ' ' + this.lastName
        }
    }
})
```

더 낫지 않나요? ( 역자 주 : 일반적으로 선언형 프로그래밍이 명령형 프로그래밍보다 코드 반복이 적은 등 우수하다고 평가하는 경향이 있음.)

#### computed 속성의 setter 함수

computed 속성은 기본적으로 getter 함수만 가지고 있지만, 필요한 경우 setter 함수를 만들어 쓸 수 있습니다.

```js
var vm = new Vue({
    el: '#example',
    data: {
        firstName: 'Foo',
        lastName: 'Bar',
    }
    computed: {
        fullName: {
            get: function () {
                return this.firstName + ' ' + this.lastName
            },
            set: function (newValue) {
                var names = newValue.split(' ')
                this.firstName = names[0]
                this.lastName = names[names.length - 1]
            }
        }
    }
})
```

이제 vm.fullName = 'John Doe'를 실행하면 설정자가 호출되고 vm.firstName 과 vm.lastName이 그에 따라 업데이트 됩니다.

### Watch 속성

대부분의 경우 computed 속성이 더 적합하지만 사용자가 만든 감시자가 필요한 경우가 있습니다. 그래서 Vue는 watch 옵션을 통해 데이터 변경에 반응하는 보다 일반적인 방법을 제공합니다. 이는 데이터 변경에
대한 응답으로 비동기식 또는 시간이 많이 소요되는 조작을 수행하려는 경우에 가장 유용합니다. 예제입니다.

 ```html

<div id="watch-example">
    <p>
        yes/no 질문을 물어보세요:
        <input type="text" v-model="question">
    </p>
    <p>{{answer}}</p>
</div>
```

```html
<!-- 이미 Ajax 라이브러리의 풍부한 생태계와 범용 유틸리티 메소드 컬렉션이 있기 때문에, -->
<!-- Vue 코어는 다시 만들지 않아 작게 유지됩니다. -->
<!-- 이것은 이미 익숙한 것을 선택할 수 있는 자유를 줍니다. -->
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
    var watchExampleVM = new Vue({
        el: '#watch-example',
        data: {
            question: '',
            answer: '질문을 하기 전까지는 대답할 수 없습니다.'
        },
        watch: {
            // 질문이 변경될 때 마다 이 기능이 실행됩니다.
            question: function (newQuestion) {
                this.answer = "입력을 기다리는 중..."
                this.debouncedGetAnswer()
            }
        },
        created: function () {
            // _.debounce는 loadash가 제공하는 기능으로
            // 특히 시간이 많이 소요되는 작업을 실행할 수 이쓴ㄴ 빈도를 제한합니다.
            // 이 경우, 우리는 yesno.wtf/api 에 엑세스 하는 빈도를 제한하고,
            // 사용자가 ajax 요청을 하기 전에 타이핑을 완전히 마칠 때까지 기다리길 바랍니다.
            // _.debounce 함수 (또는 이와 유사한 _.throttle)에 대한
            // 자세한 내용을 보려면 https://loadash.com/docs#debounce 를 방문하세요
            this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
        }
        methods: {
            getAnswer: function () {
                if (this.question.indexOf('?') === -1) {
                    this.answer = "질문에는 일반적으로 물음표가 포함 됩니다. ;-"
                    return
                }
                this.answer = '생각중...'
                var vm = this;
                axios.get('https://yesnow.wtf/api')
                        .then(function (response) {
                            vm.answer = _.capitalize(response.data.answer)
                        })
                        .catch(function (error) {
                            vm.answer = "에러! API 요청에 오류가 있습니다." + error
                        })
            }
        }
    })
</script>
```

이 경우 watch 옵션을 사용하면 비동기 연산(API 액세스 )를 수행하고, 우리가 그 연산을 얼마나 자주 수행하는지 제한하고, 최종 응답을 얻을 때까지 중간 상태를 설정할 수 있습니다. 계산된 속성은 이러한
기능을 수행할 수 없습니다. watch 옵션 외에도 명령형 vm.$watch API를 사용할 수 있습니다.

## 클래스와 스타일 바인딩

데이터 바인딩은 엘리먼트의 클래스 목록과 인라인 스타일을 조작하기 위해 일반적으로 사용됩니다. 이 두 속성은 v-bind 를 사용하여 처리할 수 있습니다. 우리는 표현식으로 최종 문자열을 계산하면 됩니다. 그러나
문자열 연결에 간섭하는 것은 짜증나는 일이며 오류가 발생하기 쉽습니다. 이러한 이유로, Vue는 class와 style에 v-bind를 사용할 때 특별히 향상된 기능을 제공합니다. 표현식은 문자열 이외에 객체 또는
배열을 이용할 수 있습니다.

## HTML 클래스 바인딩하기

#### 객체 구분

클래스를 동적으로 토글하기 위해 v-bind:calss에 객체를 전달할 수 있습니다.

 ```html

<div v-bind:class="{active : isActive}"></div>
```    

위 구문은 active 클래스의 존재 여부가 데이터 속성 isActive의 참 속성에 의해 결정되는 것을 의미합니다. 객체에 필드가 더 있으면 여러 클래스를 토글 할 수 있습니다. 또한 v-bind:class
디렉티브는 일반 class 속성과 공존할 수 있습니다. 그래서 다음과 같은 템플릿이 가능합니다.

```html

<div
        class="static"
        v-bind:class="{ active : isActive, 'text-danger' : hasError}"
></div>
```

그리고 데이터는 :

```js
data : {
    isActive: true,
        hasError
:
    false,
}
```

아래와 같이 렌더링 됩니다. :

```html

<div class="static active"></div>
```

isActive 또는 hasError 가 변경되면 클래스 목록도 그에 따라 업데이트됩니다. 예를 들어, hasError가 true가 되면 클래스 목록은 "static active text-danger" 가 됩니다.
바인딩 된 객체는 인라인 일 필요는 없습니다.

```html

<div v-bind:class="classObject"></div>
```

```js
data : {
    classObject:{
        active : true,
            'text-danger'
    :
        false,
    }
}
```

같은 결과로 렌더링 됩니다. 또한 객체를 반환하는 계산된 속성에도 바인딩 할 수 있습니다. 이것은 일반적이며 강력한 패턴입니다.

```html

<div v-bind:class="classObject"></div>
```

```js
data : {
    isActive : true,
        error
:
    null,
}
,
computed : {
    classObject : function () {
        return {
            active: this.isActive && !this.error,
            'text-danger': this.error && this.error.type == 'fatal'
        }
    }
}
```

#### 배열 구문

우리는 배열을 v-bind:class 에 전달하여 클래스 목록을 지정할 수 있습니다.

```html

<div v-bind:class="[activeClass, errorClass]"></div>
```

```js
data: {
    activeClass: 'active',
        errorClass
:
    'text-danger'
}
```

```html

<div class="active text-danger"></div>
```

목록에 있는 클래스를 조건부 토글하려면 삼항 연산자를 이용할 수 있습니다.

```html

<div v-bind:class="[isActive ? activeClass:'', errorClass"></div>
```

이것은 항상 errorClass를 적용하고 isActive가 true일 때만 activeClass를 적용합니다. 그러나 여러 조건부 클래스가 잇는 경우 장황해질 수 있습니다. 그래서 배열 구문 내에 객체 구문을
사용할 수 있습니다.

```html

<div v-bind:class="[{active:isActive}, errorClass]"></div>
```

#### 컴포넌트와 함께 사용하는 방법

이 섹션은 Vue 컴포넌트에 대해 안다고 가정합니다. 부담없이 건너 뛰고 나중에 다시 봐도 됩니다. 사용자 정의 컴포넌트로 class 속성을 사용하면, 클래스 컴포넌트의 루트 엘리먼트에 추가 됩니다. 이 엘리먼트는
기존 클래스는 덮어쓰지 않습니다. 예를 들어, 이 컴포넌트를 선언하는 경우에:

```js
Vue.component('my-component', {
    template: '<p class="foo bar">Hi</p>'
})
```

사용할 클래스 일부를 추가하십시오.

```html

<my-component class="baz boo"></my-component>
```

렌더링 된 HTML입니다.

```html
<p class="foo bar baz boo">HI</p>
```

클래스 바인딩도 동일합니다.

```html

<my-component v-bind:class="{active: isActive}"></my-component>
```

isActive가 참일 때 렌더링 된 HTML은 다음과 같습니다.

```html
<p class="foo bar active">Hi</p>
```

### 인라인 스타일 바인딩

#### 객체 구문

v-bind:style 객체 구문은 매우 직설적입니다. 거의 css 처럼 보이겠지만 JavaScript 객체 입니다. 속성 이름에 camelCase와 kebab-case(따옴표를 함께 사용해야 합니다.)를 사용할 수
있습니다.

```html

<div v-bind:style="{color:activeColor, fontSzie:fontSize + 'px' }"></div>
```

```js
data : {
    activeColor: 'red',
        fontSize
:
    30,
}
```

스타일 객체에 직접 바인딩 하여 템플릿이 더 간결하도록 만드는 것이 좋습니다.

```html

<div v-bind:style="styleObject"></div>
```

```js
data : {
    styleObject : {
        color: 'red',
            fontSize
    :
        '13px'
    }
}
```

다시, 객체 구문은 종종 객체를 반환하는 계산된 속성과 함께 사용합니다.

#### 배열 구문

v-bind:style에 대한 배열 구문은 같은 스타일의 엘리먼트에 여러 개의 스타일 객체를 사용할 수 있게 합니다.

```html

<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

#### 자동 접두사

v-bind:style에 브라우저 베더 접두어가 필요한 css 속성 (예 : transform)을 사용하면 Vue는 자동으로 해당 접두어를 감지하여 스타일을 적용합니다.

#### 다중 값 제공

2.3 버전 부터 스타일 속성에 접두사가 있는 여러 값을 배열로 전달할 수 있습니다.

```html

<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

브라우저가 지원하는 배열의 마지막 값만 렌더링합니다. 이 예제에서는 flexbox의 접두어가 붙지않은 버전을 지원하는 브라우저에 대해 display: flex를 렌더링합니다.

## 조건부 렌더링

### v-if

v-if 디렉티브 조건에 따라 블록을 렌더링하기 위해 사용됩니다. 블록은 디렉티브의 표현식이 true 값을 반환할 때만 렌더링됩니다.

```html
<h1 v-if="awesome">Vue is awesome!</h1>
```

v-else 와 함께 "else 블록"을 추가하는 것도 가능합니다.

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no</h1>
```

#### <template> 에 v-if을 갖는 조건부 그룹 만들기

v-if 디렉티브는 떄문에 하나의 엘리먼트를 추가해야합니다. 하지만 하나 이상의 엘리먼트를 트랜지션하려면 어떻게 해야할까요? 이 경우 우리는 보이지 않는 래퍼 역할을 하는 <template>엘리먼트에 v-if를
사용할 수 있습니다. 최종 렌더링 결과에는 <template>엘리먼트가 포함되지 않습니다.

```html

<template v-if="ok">
    <h1>Title</h1>
    <p>Pragraph 1</p>
    <p>Pragraph 2</p>
</template>
```

#### v-else

v-else 디렉티브를 이용하여 v-if에 대한 "else 블록을 나타낼 수 있습니다."

```html

<div v-if="Math.random() > 0.5">
    이제 나를 볼 수 있어요
</div>
<div v-else>
    이제는 안보입니다.
</div>
```

v-else 엘리먼트는 v-if 엘리먼트 또는 v-else-if 엘리먼트 바로 뒤에 있어야 합니다. 그렇지 않으면 인식할 수 없습니다.

#### v-else-if

2.1.0 부터 새롭게 추가됨. v-else-if는 이름에서 알 수 있듯, v-if에 대한 "else if" 역할을 합니다. 여러개를 사용 할 수 있습니다.

```html

<div v-if="type === 'A'">
    A
</div>
<div v-else-if="type === 'B'">
    B
</div>
<div v-else-if="type === 'C'">
    C
</div>
<div v-else>
    Not A/B/C
</div>
```

v-else와 마찬가지로, v-else-if 엘리먼트는 v-if 또는 v-else-if 엘리먼트 바로 뒤에 와야 합니다.

#### key를 이용한 재사용 가능한 엘리먼트 제어

Vue는 가능한 한 효율적으로 엘리먼트를 렌더링하려고 시도하며 종종 처음부터 렌더링을 하지않고 다시 사용합니다. Vue를 매우 빠르게 만드는데 도움이 되는 것 이외에 몇가지 유용한 이점이 있습니다. 예를 들어
사용자가 여러 로그인 유형을 트랜지션할 수 있도록 허용하는 경우입니다.

```html

<template v-if="loginType === 'username'">
    <label>사용자 이름</label>
    <input placeholder="사용자 이름을 입력하세요">
</template>
<template v-else>
    <label>이메일</label>
    <input placeholder="이메일 주소를 입력하세요">
</template>
```

위 코드에서 loginType을 바꾸어도 사용자가 이미 입력한 내용은 지워지지 않습니다. 두 템플릿 모두 같은 요소를 사용하므로 <input>은 대체되지 않고 단지 placeholder만 변경됩니다. 폼 인풋에
텍스트를 입력하고 토글 버튼을 눌러 직접 확인해봐요.

이것은 항상 바람직하지는 않습니다. 때문에 "이 두 앨리먼트는 완전히 별개이므로 다시 사용하지 마십시오."라고 알리는 방법을 제공합니다. 유일한 값으로 key 속성을 추가하십시오.

```html

<template v-if="loginType === 'username'">
    <label>사용자 이름</label>
    <input placeholder="사용자 이름을 입력하세요" key="username-input">
</template>
<template v-else>
    <label>이메일</label>
    <input placeholder="이메일 주소를 입력하세요" key="email-input">
</template>
```

이제 트랜지션 할 떄마다 입력이 처음부터 렌더링 된다. label 엘리먼트는 key 속성이 없기 때문에 여전히 효율적으로 재사용 된다.

### v-show

엘리먼트를 조건부로 표시하기 위한 또 다른 옵션은 v-show 디렉티브

```html
<h1 v-show="ok">안녕하세요!</h1>
```

차이점은 v-show 가 있는 엘리먼트는 항상 렌더링 되고 DOM에 남아있다는 점입니다. v-show는 단순히 display css속성을 토글합니다.

## [리스트 렌더링](./list_rendering.html)

v-for 블록안에는 부모 범위 속성에 대한 모든 권한이 있습니다. v-for는 또한 현재 항목의 인덱스에 대한 두 번째 전달인자 옵션을 제공합니다.
(example2)

in 대신에 of 구분자로 사용할 수 있습니다. 그래서 Javascript의 이터레이터에 대한 자바스크립트 구문과 유사합니다.

### v-for와 객체

v-for 를 사용하여 객체의 속성을 반복할 수도 있습니다. (v-for-object)
키에 대한 두번째 전달 인자를 제공할 수도 있습니다. 인덱스도 제공합니다. 객체 반복 순서는 일관적이지 않습니다.

### Maintaining State

Vue가 v-for 에서 랜더링된 엘리먼트 목록을 갱신할 때 기본적으로 "in-place patch" 전략을 사용합니다. 데이터 항목의 순서가 변경된 경우 항목의 순서와 일치하도록 DOM 요소를 이동하는 대신
Vue는 각 요소를 적절한 위치에 패치하고 해당 인덱스에서 렌더링할 내용을 반영하는지 확인합니다. 이것은 Vue1.x의 track-by=$index의 동작과 유사합니다.

이 기본 모드는 효율적이지만 목록의 출력 결과가 하위 컴포넌트 상태 또는 임시 DOM 상태( 예: 폼 input )에 의존하지 않는 경우에 적합합니다.

Vue 에서 개별 DOM 노드들을 추적하고 기존 엘리먼트를 재사용, 재정렬하기 위해서 v-for의 각 항목들에 고유한 key 속성을 제공해야 합니다. key에 대한 이상적인 값은 각 항목을 식별할 수 있는 고유한
ID 입니다. 이 특별한 속성은 1.x버전의 track-by와 거의 비슷하지만 속성처럼 작동하기 때문에 v-bind를 사용하여 동적 값에 바인딩 해야 합니다.

Vue에서 개별 DOM 노드들을 추적하고 기존 엘리먼트를 재사용, 재정렬하기 위해서 v-for 의 각 항목들에 고유한 key 속성을 제공해야 합니다. key에 대한 이상적인 값은 각 항목을 식별할 수 있는 고유한
ID입니다. 이 특별한 속성은 1.x 버전의 track-by와 거의 비슷하지만 속성처럼 작동하기 때문에 v-bind를 사용하여 동적 값에 바인딩 해야합니다. (여기서는 약어를 사용합니다.)

```html

<div v-for="item in items" v-bind:key="item.id">
    <!-- content -->
</div>
```

반복되는 DOM 내용이 단순한 경우나 의도적인 성능 향상을 위해 기본 동작에 의존하지 않는 경우를 제외하면, 가능하면 언제나 v-for 에 key를 추가하는 것이 좋습니다. key는 노드 식별하는 일반적인
메커니즘임.

### 배열 변경 감지

#### 변이 메소드

Vue는 감시중인 배열의 변이 메소드를 래핑하여 뷰 갱신을 트리거합니다. 래핑된 메소드는 다음과 같습니다. push(), pop(), shift(), unshift(), splice(), sort(),
reverse()

콘솔을 열고 이전 예제의 items 배열로 변이 메소드를 호출하여 동적으로 추가 할 수 있습니다.

#### 배열 대체

이름에서 알 수 있듯 변이 메소드는 호출된 원본 배열을 변형합니다. 이와 비교하여 변형을 하지 않는 방법도 있습니다. 바로 filter(), concat() 와 slice()입니다. 이 방법을 사용하면 원본 배열을
변형하지 않고 항상 새 배열을 반환합니다. 변형이 없는 방법으로 작업할 때 이전 배열을 새 배열로 바꿀 수 있습니다.

```js
example1.items = example1.items.filter(function (item) {
    return item.message.match(/Foo/)
})
```

이렇게 하면 Vue가 기존 DOM을 버리고 전체 목록을 다시 렌더링 한다고 생각할 수 있습니다. 다행히도, 그렇지는 않습니다. Vue는 DOM요소 재사용을 극대화하기 위해 몇가지 똑똑한 구현을 하므로 배열을 겹치는
객체가 포함된 다른 배열로 대체하여 효율적입니다.

#### 주의 사항

JavaScript 제한으로 인해 Vue는 배열에 대해 다음과 같은 변경 사항을 감지할 수 없습니다.

1. 인덱스로 배열에 있는 항목을 직접 설정하는 경우, 예 : vm.items[1] = newValue
2. 배열 길이를 수정하는 경우, 예 vm.items.length = newLength

주의사항 중 1번을 극복하기 위해 필요한 작업 Vue.set(vm.items, 1, newValue) or vm.items.splice(1, newValue) or vm.$set(vm.items, 1,
newValue)
주의사항 중 2번을 극복하기 위한 작업 vm.items.splice(newLength)

#### 객체 변경 감지에 관한 주의사항

모던 JavaScript한계로 Vue는 속성 추가 및 삭제를 감지하지 못합니다.

```js
var vm = new Vue({
    data: {
        //반응형임
        a: 1
    }
})
// 반응형이 아님
vm.b = 2 
```

Vue는 이미 만들어진 인스턴스에 새로운 루트레벨의 반응형 속성을 동적으로 추가하는 것을 허용하지 않습니다. 그러나 Vue.set(object, propertyName, value)메소드를 사용해 중첩된 객체데
반응형 속성을 추가할 수 있습니다.

```js
var vm = new Vue({
    data: {
        userProfile: {
            name: 'Anika'
        }
    }
})
```

다음과 같이 중첩된 userProfile 객체에 새로운 속성 age를 추가합니다.

```js
Vue.set(vm.userProfile, 'age', 27)
```

인스턴스 메소드인 vm.$set(vm.userProfile, 'age', 27) 사용 가능

때로는 Object.assign()이나 _.extend()를 사용해 기존의 객체에 새 속성읋 할당할 수 있습니다. 이 경우 두 객체의 속성을 사용해 새 객체를 만들어야 합니다.

```js
Object.assign(vm.userProfile, {
    age: 27,
    favoriteColor: 'Vue Green'
})
```

새로운 반응형 속성을 다음과 같이 추가합니다.

```js
vm.userProfile = Object.assign({}, vm.userProfile, {
    age: 27,
    favoriteColor: 'Vue Green'
})
```

### 필터링된/정렬 된 결과 표시하기

때로 원본 데이터를 실제로 변경하거나 재설정하지 않고 배열된 필터링 된 버전이나 정렬된 버전을 표시해야 할 필요가 있습니다. 이 경우 필터링된 배열이나 정렬된 배열을 반환하는 계산된 속성을 만들 수 있습니다.

```html

<li v-for="n in evenNumbers">{{ n }}</li>
```

```js
var vm = new Vue({
    el: "example",
    data: {
        numbers: [1, 2, 3, 4, 5]
    },
    computed: {
        evenNumbers: function () {
            return this.numbers.filter(function (number) {
                return number % 2 === 0
            })
        }
    }
})
```

계산된 속성을 실행할 수 없는 상황(예 : 중첩된 v-for 루프 내부)에서는 다음 방법을 사용 할 수 있습니다.

```html

<li v-for="n in even(numbers)">{{ n }}</li>
```

```js
var vm = new Vue({
    el: "example1",
    data: {
        nubmers: [1, 2, 3, 4, 5],
        methods: {
            even: function (numbers) {
                return numbers.filter(function (number) {
                    return number % 2 === 0
                })
            }
        }
    }
})
```

### Range v-for
v-for 는 숫자를 사용할 수 있습니다.
```html
<div>
    <span v-for="n in 10">{{ n }}</span>
</div>
```

### v-for 템플릿
템플릿 v-if와 마찬가지로 <template> 태그를 사용해 여러 엘리먼트의 블럭을 렌더링 할 수 있습니다.
```html
<ul>
    <template v-for="item in items">
        <li>{{ item.msg }}</li>
        <li class="divider" role="presentation"></li>
    </template>
</ul>
```
### v-for 와 v-if
동일한 노드에 두가지 모두 있다면, v-for가 v-if보다 높은 우선순위를 갖습니다. 즉 v-if는 루프가 반복될 때마다 실행됩니다. 이는 일부 항목만 렌더링 하려는 경우 유용합니다.
```html
<li v-for="todo in todos" v-if="!todo.isComplete">
    {{ todo }}
</li>
```

위의 경우 완료되지 않은 할일만 렌더링 합니다.

위 방법 대신 실행을 조건부로 하는 것이 목적이라면 래퍼 엘리먼트(또는 <template>)을 사용하세요.

```html
<ul v-if="todo.length">
    <li v-for="todo in todos">
        {{ todo }}
    </li>
</ul>
<p v-else>No todos left!</p>
```

### v-for 와 컴포넌트
v-for 를 사용자 정의 컴포넌트에 직접 사용할 수 있습니다.
```html
<my-component v-for="item in items" :key="item.id"></my-component>
```
그러나 컴포넌트에는 자체 범위가 분리되어있기 떄문에 컴포넌트에 데이터를 자동으로 전달하지는 않습니다. 반복할 데이터를 컴포넌트로 전달하려면 props도 사용해야 합니다.
```html
<my-component
        v-for="(item, index) in items"
        :item="item"
        :index="index"
        :key="item.id"
></my-component>
```
컴포넌트에 item을 자동으로 주입하지 않는 이유는 컴포넌트가 v-for의 작동방식과 밀접하게 결합되기 때문입니다. 데이터의 출처를 명확히 하면 다른 상황에서 컴포넌트를 재사용할 수 있습니다.
```html
<div id="todo-list-example">
    <form @submit.prevent="addNewTodo">
        <label for="new-todo">Add a todo</label>
        <input 
                v-model="newTodoText"
                id="new-todo"
                placeholder="E.g. Feed the cat"
                type="text"
        >
        <button>Add</button>
    </form>
    <ul>
        <li
                is="todo-item"
                v-for="(todo, index) in todos"
                :key="todo.id"
                :title="todo.title"
                @remove="todo.splice(index,1)"
        ></li>
    </ul>
</div>
```
is 속성을 보면 잠재적인 브라우저의 구문 분석 오류를 해결합니다. 
li 태그를 todo-item 컴포넌트로 만들겠다는 속성 선언!
