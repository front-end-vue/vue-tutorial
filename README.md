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
var data = { a: 1};
var vm = new Vue({
data : data
});
// 인스턴스에 있는 속성은 원본 데이터에도 영향을 미칩니다.
vm.a == data.a  // => true
vm.a= 2
console.log(data.a) // => 2

// ... 반대로 마찬가지입니다.
data.a = 3
vm.a //=>3
```

2. 데이터가 변경되면 화면은 다시 렌더링 됩니다. Vue객체 생성 당시에 data 에 선언한 속성들만 반응형 다음과 같이 새 속성을 추가하면
```js
vm.b= 'hi';
```
b가 변경되어도 화면이 갱신되지 않습니다. 어떤 속성이 나중에 필요하다는 것을 알고 있으며, 빈 값이거나 존재하지 않은 상태로 시작한다면 아래와 같이 초기값을 지정할 필요가 있습니다.
```js
data : {
    newTodoText : '',
    visitCount: 0,
    hideCompletedTodos: false,
    todos: [].
    error: null
}
```
여기에서 유일한 예외는 Object.freeze()를 사용하는 경우입니다. 이는 기존 속성이 변경되는 것을 막아 반응성 시스템이 추적할 수 없다는 것을 의미합니다.

2. Vue인스턴스는 데이터 속성 이외에도 유용한 인스턴스 속성 및 메소드를 제공합니다. 다른 사용자 정의 속성과 구분하기 위해 $접두어를 붙여 구현해두었습니다.
```js
var data = { a : 1 };
var vm = new Vue({
    el : '#example',
    data : data
});

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true
vm.$watch('a', function(newVal, oldVal){
    // 'vm.a'가 변경되면 호출 됩니다.
})
```

### 인스턴스 라이프사이클 훅
1. 각 Vue 인스턴스는 생성될 때 일련의 초기화 단계를 거칩니다. 데이터 관찰 설정이 필요한 경우, 템플릿을 컴파일하는 경우, 인스턴스를 DOM에 마운트하는 경우, 그리고 데이터가 변경되어 DOM을 업데이트하는 경우 그 과정에서 사용자 정의 로직을 실행할 수 있는 라이프사이클 훅 도 호출됩니다. 예를 들어
created 훅은 인스턴스가 생성된 후에 호출됩니다.
```js
new Vue({
    data:{
        a:1
    },
    created : function(){
        // 'this' 는 vm 인스턴스를 가리킵니다.
        console.log('a is:' + this.a)
    }
})
// => "a is : 1"
```
인스턴스 라이프사이클의 여러 단계에서 호출될 다른 훅도 있습니다. 그 예로 mounted, updated 및 destroyed가 있습니다. 모든 라이프사이클 훅은 this 컨텍스트가 호출하는 Vue 인스턴스를 가리키며 호출됩니다. Vue 세계에서 "컨트롤러"의 컨셉이 어디에 있는지 궁금할 수 있습니다. 답은 컨트롤러가 없습니다. 컴포넌트의 사용자 지정 로직은 이러한 라이프사이클 훅으로 분할됩니다.
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
* v-html디렉티브 사용된 span의 내용은 rawHtml로 대체됩니다. 이 때 데이터 바인딩은 무시되는데 이유는 Vue가 문자열 기반 템플릿 엔진이 아니기 때문에 v-html 디렉티브를 이용한다면 mustache 보간 즉 템플릿 문법을 사용할 수 없습니다.
이와 달리 컴포넌트는 UI 재사용 및 구성을 위한 기본단위로 사용하는 것을 추천합니다.
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
- 디렉티브는 v- 접두사가 있는 특수 속성입니다. 디렉티브 속성 값은 단일 JavaScript 표현식이 됩니다. (나중에 설명할 v-for은 예외입니다.) 디렉티브의 역할은 표현식의 값이 변경될 때 사이드이펙트를 반응적으로 DOM에 적용하는 것 입니다.
```html
<p v-if="seen">이제 나를 볼 수 있어요</p>
```
여기서 v-if 디렉티브는 seen표현의 참/거짓에 기반하여 <p> 엘리멘트를 제거 또는 생성 합니다.
#### 전달인자
1. 일부 디렉티브는 콜론으로 표시되는 "전달인자"를 사용할 수 있습니다. 예를 들어, v-bind 디렉티브는 반응적으로 HTML 속성을 갱신하는데 사용됩니다.
```html
<a v-bind:href="url">...</a>
```
여기서 href는 전달인자로, 엘리먼트의 href속성을 표현식 url의 값에 바인드하는 v-bind 디렉티브에게 알려줍니다.
또 다른 예로 DOM 이벤트를 수신하는 v-on 디렉티브 입니다.
```html
<a v-on:click="doSomething">...</a>
```
전달인자는 이벤트를 받을 이름입니다. 우리는 이벤트 핸들링에 대해 더 자세하게 살펴 볼 것입니다.

#### 동적 전달인자.
2.6.0 버전에서 추가됨.
2.6.0 버전부터 JavaScript 표현식을 대괄호로 묶어 디렉티브의 야규멘트로 사용하는것도 가능해졌습니다.
```html
<!--
동적 전달인자는 "동적 전달인자의 형식 제약"의 부분에서 후술되는바와 같이, 조금의 제약이 있는 점에 주의해주세요.
-->
<a v-bind:[attributeName]="url">...</a>
```
여기서 attributeName은 JavaScript형식으로 동적 변환되어, 그 변환결과가 전달인자의 최종적인 벨류로 사용됩니다. 예를들어 당신의 Vue 인스턴스에 href 라는 값을 가진 attributeName 데이터 속성을 가진 경우 이 바인딩은 v-bind:href 와 동등합니다.
이와 유사하게, 동적인 이벤트명에 핸들러를 바인딩할 때 동적 전달인자를 활용할 수 있습니다.
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
이를 피하는 방법은, 스페이스나 따옴표를 포함하지 않는 형식을 사용하거나, 복잡한 표현식을 계솬된 속성(Computed)으로 대체하는 것입니다.
in-DOM템플릿을 사용할 때에는 (템플릿이 HTML파일에 직접 쓰여진 경우) 브라우저가 모든 속셩명을 소문자로 만드는 관계로 대문자 사용을 피하는 것이 좋습니다.

```html
<!--
in-DOM 템플릿에서는 이 부분이 v-bind:[someattr]로 변환됩니다.
인스턴스에 "someattr"속성이 없는 경우, 이 코드는 동작하지 않습니다.
-->
<a v-bind:[someAttr]="value"">...</a>
```


#### 수식어
 수식어는 점으로 표시되는 특수 접미사로, 디렉티브를 특별한 방법으로 바인딩 해야 함을 나타냅니다. 예를 들어, .prevent 수식어는 트리거된 이벤트에서 event.preventDefault()를 호출하도록 v-on디렉티브에게 알려줍니다.
 ```html
<form v-on:submit.prevent="onSubmit">...</form>
```
나중에 v-on과 v-model을 더 자세히 살펴볼 때 수식어를 더 많이 사용할 것 입니다.

###약어
v- 접두사는 템플릿의 Vue 특정 속성을 식별하기 위한 시각적인 신호 역할을 합니다. 이 기능은 Vue.js를 사용하여 기존의 마으컵에 동적인 동작을 적용할 떄 유용하지만 일부 자주 사용되는 디렉티브에 대해 너무 장황하다고 느껴질 수 있습니다. 동시에 Vue.js가 모든 템플릿을 관리하는 SPA v- 접두어의 필요성이 떨어집니다. 따라서 가장 자주 사용되는 두개의 디렉티브인 v-bind와 v-on에 대해 특별한 약어를 제공합니다.

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
이들은 일반적인 HTML과 조금 다르게 보일 수 있습니다. 하지만 :와 @ 속성 이름에 유효한 문자이며 Vue.js를 지원하는 모든 브라우저는 올바르게 구문 분석을 할 수 있습니다. 또한 최종 렌더링 된 마크업에는 나타나지 않습니다. 약어는 완전히 선택사항이지만 나중에 익숙해지면 편할 것 입니다.




## computed와 watch
 템플릿 내에 표현식을 넣으면 편리합니다. 하지만 간단한 연산일 때만 이용하는 것이 좋습니다. 너무 많은 연산을 템플릿 안에서 하면 코드가 비대해지고 유지보수가 어렵습니다.
```html
<div id="example">
    {{ message.split(''}.reverse().join('') }
</div>
```
이 템플릿은 더 이상 간단하고 명료하지 않습니다. message를 역순으로 표시한다는 것을 알려면 찬찬히 살펴봐야 합니다. 템플릿에 역순할 일이 더 많아진다면 힘듬
복잡한 로직이라면 반드시 computed 속성을 사용해야 하는 이유입니다.

#### 기본 예제
```html
<div id="example">
    <p>원본 메세지 : "{{ message }}"</p>
    <p>역순으로 표시한 메시지 : "{{ reverseMessage }}"</p>
</div>
```

```js
var vm = new Vue({
    el : "#example",
    data : {
        message : "안녕하세요"
    },
    computed : {
        // 계산된 getter
        reverseMessage : function(){
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
computed 속성 대신 메소드와 같은 함수를 정의할 수도 있습니다. 최종 결과에 대해 두 가지 접근 방식은 서로 동일합니다. 차이점은 computed 속성은 종속 대사을 따라 저장(캐싱)된다는 것 입니다. computed 속성은 해당 속성이 종속된 대상이 변경될 때만 함수를 실행합니다. 즉 message가 변경되지 않는 한 computed 속성인 reverseMessage를 여러 번 요청해도 계산을 다시 하지 않고 계산되어 있던 결과를 즉시 반환합니다. 또한 Date.now()처럼 아무 곳에도 의존하지 않는 computed 속성의 경우 절대로 업데이트되지 않는다는 뜻입니다.

```js
computed: {
  now: function () {
    return Date.now()
  }
}
```
이에 비해 메소드를 호출하면 렌더링을 다시 할 때마다 항상 함수를 실행합니다.

캐싱이 왜 필요할까요? 계산에 시간이 많이 걸리는 computed 속성인 A를 가지고 있다고 해봅시다. 이 속성을 계산하려면 거대한 배열을 반복해서 다루고 많은 계산을 해야합니다. 그런데 A 에 의존하는 다른 computed 속성 값도 있을 수 있습니다. 캐싱을 하지 않으면 A의 getter함수를 꼭 필요한 것보다 더 많이 실행하게 됩니다! 캐싱을 원하지 않는 경우 메소드를 사용하십시오.

#### computed 속성 vs watch 속성
Vue는 Vue인스턴스의 데이터 변경을 관찰하고 이에 반응하는 보다 일반적인 watch 속성을 제공합니다. 다른 데이터 기반으로 변경할 필요가 있는 데이터가 있는 경우, 특히 AngularJS를 사용하던 경우 watch를 남용하는 경우가 있습니다. 하지만 명령적인 watch 콜백보다 computed 속성을 사용하는 것이 더 좋습니다. (역자 주: watch 속성은 감시할 데이터를 지정하고 그 데이터가 바뀌면 이런 함수를 실행하라는 방식으로 소프트웨어 공학에서 이야기하는 '명령형 프로그래밍'방식. computed속성은 계산해야 하는 목표 데이터를 정의하는 방식으로 소프트웨어 공학에서 이야기하는 '선언형 프로그래밍'방식 )
```html
<div id="demo">{{ fullName }}</div>
```

```js
var vm = new Vue({
    el : "#demo",
    data : {
        firstName : 'Foo',
        lastName : 'Bar',
        fullName : 'Foo Bar'
    },
    watch:{
        firstName : function(val){
            this.fullName = val + ' ' + this.lastName
        },
        lastName : function(val){
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
    data : {
        firstName : 'Foo',
        lastName : 'Bar',
    }
    computed : {
        fullName : {
            get : function(){
                return this.firstName + ' ' + this.lastName
            },
            set : function(newValue){
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
 대부분의 경우 computed 속성이 더 적합하지만 사용자가 만든 감시자가 필요한 경우가 있습니다. 그래서 Vue는 watch 옵션을 통해 데이터 변경에 반응하는 보다 일반적인 방법을 제공합니다. 이는 데이터 변경에 대한 응답으로 비동기식 또는 시간이 많이 소요되는 조작을 수행하려는 경우에 가장 유용합니다.
 예제입니다.
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
        el : '#watch-example',
        data : {
            question: '',
            answer : '질문을 하기 전까지는 대답할 수 없습니다.'
        },
        watch : {
            // 질문이 변경될 때 마다 이 기능이 실행됩니다.
            question : function(newQuestion){
                this.answer = "입력을 기다리는 중..."
                this.debouncedGetAnswer()
            }
        },
        created: function(){
            // _.debounce는 loadash가 제공하는 기능으로
            // 특히 시간이 많이 소요되는 작업을 실행할 수 이쓴ㄴ 빈도를 제한합니다.
            // 이 경우, 우리는 yesno.wtf/api 에 엑세스 하는 빈도를 제한하고,
            // 사용자가 ajax 요청을 하기 전에 타이핑을 완전히 마칠 때까지 기다리길 바랍니다.
            // _.debounce 함수 (또는 이와 유사한 _.throttle)에 대한
            // 자세한 내용을 보려면 https://loadash.com/docs#debounce 를 방문하세요
            this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
        }
        methods : {
            getAnswer : function(){
                if (this.question.indexOf('?') === -1){
                    this.answer = "질문에는 일반적으로 물음표가 포함 됩니다. ;-"
                    return
                }
                this.answer='생각중...'
                var vm = this;
                axios.get('https://yesnow.wtf/api')
                    .then(function(response){
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
이 경우 watch 옵션을 사용하면 비동기 연산(API 액세스 )를 수행하고, 우리가 그 연산을 얼마나 자주 수행하는지 제한하고, 최종 응답을 얻을 때까지 중간 상태를 설정할 수 있습니다. 계산된 속성은 이러한 기능을 수행할 수 없습니다.
watch 옵션 외에도 명령형 vm.$watch API를 사용할 수 있습니다.


## 클래스와 스타일 바인딩
데이터 바인딩은 엘리먼트의 클래스 목록과 인라인 스타일을 조작하기 위해 일반적으로 사용됩니다. 이 두 속성은 v-bind 를 사용하여 처리할 수 있습니다. 우리는 표현식으로 최종 문자열을 계산하면 됩니다. 그러나 문자열 연결에 간섭하는 것은 짜증나는 일이며 오류가 발생하기 쉽습니다. 이러한 이유로, Vue는 class와 style에 v-bind를 사용할 때 특별히 향상된 기능을 제공합니다. 표현식은 문자열 이외에 객체 또는 배열을 이용할 수 있습니다.

## HTML 클래스 바인딩하기
#### 객체 구분
 클래스를 동적으로 토글하기 위해 v-bind:calss에 객체를 전달할 수 있습니다.
 ```html
<div v-bind:class="{active : isActive}"></div>
```    
위 구문은 active 클래스의 존재 여부가 데이터 속성 isActive의 참 속성에 의해 결정되는 것을 의미합니다.
객체에 필드가 더 있으면 여러 클래스를 토글 할 수 있습니다. 또한 v-bind:class 디렉티브는 일반 class 속성과 공존할 수 있습니다. 그래서 다음과 같은 템플릿이 가능합니다.
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
    hasError : false,
}
```
아래와 같이 렌더링 됩니다. : 
```html
<div class="static active"></div>
```
isActive 또는 hasError 가 변경되면 클래스 목록도 그에 따라 업데이트됩니다. 예를 들어, hasError가 true가 되면 클래스 목록은 "static active text-danger" 가 됩니다. 바인딩 된 객체는 인라인 일 필요는 없습니다.

```html
<div v-bind:class="classObject"></div>
```

```js
data : {
    classObject:{
        active : true,
        'text-danger': false,
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
    error : null,
},
computed : {
    classObject : function(){
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
  errorClass: 'text-danger'
}
```
```html
<div class="active text-danger"></div>
```
목록에 있는 클래스를 조건부 토글하려면 삼항 연산자를 이용할 수 있습니다.
```html
<div v-bind:class="[isActive ? activeClass:'', errorClass"></div>
```
이것은 항상 errorClass를 적용하고 isActive가 true일 때만 activeClass를 적용합니다. 그러나 여러 조건부 클래스가 잇는 경우 장황해질 수 있습니다. 그래서 배열 구문 내에 객체 구문을 사용할 수 있습니다.
```html
<div v-bind:class="[{active:isActive}, errorClass]"></div>
```

#### 컴포넌트와 함께 사용하는 방법
이 섹션은 Vue 컴포넌트에 대해 안다고 가정합니다. 부담없이 건너 뛰고 나중에 다시 봐도 됩니다.
사용자 정의 컴포넌트로 class 속성을 사용하면, 클래스 컴포넌트의 루트 엘리먼트에 추가 됩니다. 이 엘리먼트는 기존 클래스는 덮어쓰지 않습니다.
예를 들어, 이 컴포넌트를 선언하는 경우에:
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
v-bind:style 객체 구문은 매우 직설적입니다. 거의 css 처럼 보이겠지만 JavaScript 객체 입니다. 속성 이름에 camelCase와 kebab-case(따옴표를 함께 사용해야 합니다.)를 사용할 수 있습니다.
```html
<div v-bind:style="{color:activeColor, fontSzie:fontSize + 'px' }"></div>
```

```js
data : {
    activeColor: 'red',
    fontSize:30,
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
        fontSize : '13px'
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
