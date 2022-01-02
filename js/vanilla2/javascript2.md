

---

다음 내용은 인프런 '김영보'의 [자바스크립트 중고급: 근본 핵심 이해](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%A4%91%EA%B3%A0%EA%B8%89/dashboard ) 강의 정리 내용 입니다.

### [SECTION1   중고급 강좌 소개, 범위] 

#### L1 - ES3/ES5 스펙의 아키텍처 / 메커니즘 관련 키워드

---

ES3 스팩

1. Execution Contexts

   >  실행 콘텍스트. 함수 호출 시 함수를 실행하는 환경. 함수가 실행 되었을때 결과를 저장하는 영역. 즉, 함수의 모든 처리는 이 안에서 이루어진다.

2. Definition

   * Function Objects

     : 엔진이 Function 키워드를 만났을때 만드는 Function Object를 뜻한다.

   * Types of Executable Code 

      : 실행 가능 코드. (Global, Eval, Function 코드가 있다.)

   * Global, Function Code

      : 둘은 위치만 다르고 코드는 같다.  단, 실행하는 영역이 다름.

   * Variable Instantiation 

      : 변수를 어떻게 인스턴스화 시켜서 해결할 것이냐.

   * Scope Chain and Identifier Resolution:

     * Identifier Resolution
       : 함수를 호출할때 어떻게 함수 이름을 찾을것이냐. 
       변수의 값을 설정할때 어떻게 변수의 이름을 찾을것이냐. (여기서 Id는 함수, 변수 이름을 뜻한다.)

       1. Global Object

          : 전역 객체이다. (Function Object와 구분하는 이유는 영역이 다르기때문이다.)

       2. Activation Object

          : 함수 호출 시 함수를 실행할 수 있는 환경.  
          그리고 함수가 실행되었을 때 실행되는 결과를 저장하는 오브젝트 
          (Object니까 Property이다. 즉,  이 안에는 또 다른 Object가 존재할 수 있다.)

   * This

     : 인스턴스에서 중요한 위치이다.

   * Arguments Object

     함수의 파라미터를 처리하는 오브젝트

3. Entering An Execution Context

   * Global Code
   * Eval Code
   * Function Code

ES5 스펙

1. Executable Code and Execution Contexts

2. Types of Executable Code

   * Strict Mode Code

     : use strict 로 작성했을때의 실행 모드

3. Lexical Environments

   > Lexical(정적), Dynamic(동적)이다.  
   > 자바스크립트는 Lexical 환경을 취한다. 그래서 실행 콘텍스트가 하나의 실행하는 묶음이라면 그 안에서 환경적인 측면을 처리하는것이다.

   * Environment Records

     : 함수가 호출되어 실행될 때, 실행되기 전 상황을 기록

   * Lexical Environment Operations

     : 어떤 변수에 값을 할당했을때 그것을 정적인 환경에 설정한다는 것.

   * The Global Environment

     : Global Object를 처리하는 Environment 

4. Execution Contexts

   > Identifier Resolution: 실행 Context가 실행하는 묶음이면. 식별자 해결은 바탕이 되는 개념. 함수를 호출했을때 어디서 찾을꺼냐? 이러한 개념이 여기 들어가있다.

5. Establishing an Execution Context

   >  Entering이란? 함수 안으로 엔진 컨트롤이 이동했을때 함수 안에 작성된 코드를 어떻게 처리할 것인가. 이러한 개념적인 이야기.

   * Entering Global Code 
   * Entering Eval Code
   * Entering Function Code

6. Declaration Binding Instantiation

   > 바인딩은 변수, this와 관련이다. 
   > 변수를 어떻게 Execution Contexts의 Lexical Environments에 바인딩시켜서 Recording 할것인가.
   > 또한 This를 어떻게 바인딩할 것인가. (This는 함수 안에서만 되는데 ..)
   > 그리고 이것들은 Record 될것이고 Environment에 정리가 될것이다.

7. Arguments Object

   > 파라미터 처리 오브젝트.

   

**함수가 실행되기전에 어떻게 할 것인가. 그리고 함수가 호출되었을때 어떻게 처리할 것인가에 대한 내용이 위에 서술한 모든 내용에 담겨있는것이다.**



#### L2 - 엔진 관점의 핵심 키워드

---

1. 엔진 처리 : 크게 해석, 실행으로 나뉜다.

   * 해석: 컴파일, 실행을 할 환경을 설정
   * 실행: 해석 단계에서 설정된 환경을 바탕으로 코드를 실행하는 것.
     * 함수가 호출 된 다음 단계에 하는것.
     * 키워드는 Context(함수라는 단위를 어떠한 묶음 ,덩어리로 가져갈 것인지 정하는 것)

2. 엔진 관점에서의 부담

   > 단일 Context 개념으로 하나의 box 안에서 모든 함수, 변수 관련된 일을 처리하는 것이 심플하고 빠르다. 여기저기 선언된 변수, 함수를 찾아다니는것은 복잡하고 시간도 오래거린다.
   > 이미 Context 실행중인데 Scope Chain 개념으로 어디가에서 다시 끌고오는 ... 이것은 부담스럽다.

   -> 이것은 궁극적으로 Identifier Resolution(식별자 해결)을 위한것이다. 즉, 함수, 변수이름을 어떻게 빨리 찾고 실행할 것인가.



#### L3 - Execution Context 형태

---

```javascript
function book(){
    var point = 123;   
    function show(){   
        var title = "js책";
        // getPoint();
        // this.bookAmount;
    };
    function getPoint(){
        return point;
    };
    show();
}
book();
```

1. book() 함수 호출 

   1. show function 오브젝트 생성

   2. show의 [[Scope]]에 스코프 설정 (point, getPoint() (변수이름,함수이름)을 설정)
      [[Scope]]: 대괄호 두개([[)는 엔진이 설정하는 프로퍼티를 뜻한다.

   3. getPoint function 오브젝트 생성

   4. show() 호출

      1. 엔진 컨트롤이 show() 함수 안으로 이동

         1. 이동하기전 show 실행 콘텍스트를 만든다. (함수 실행을 위한 Context 환경 구축)

            즉, 하나의 덩어리 안에서 함수가 실행될 수 있도록 만드는 것.

            * 실행 컨텍스트 구성요소

              ``` javascript
              show 실행 콘텍스트(EC): {
              	렉시컬 환경 컴포넌트(LEC): {  //1
                      환경 레코드(ER): {  //3
                          선언적 환경 레코드(DER): {
              				title: "JS책"
                          },
                          오브젝트 환경 레코드(OER): {}
                      },
                      외부 렉시컬 환경 참조(OLER):{
                          point: 123,
                          getPoint: function(){}
                      }
                  },
                  변수 환경 컴포넌트(VEC): {},  //1
                  this 바인딩 컴포넌트(TBC): {  //2
                      글로벌 오브젝트(window)
                  }
              }
              ```

              1. LEC와 같은 레벨에 VEC이 있다. 그리고 LEC, VEC의 초기값은 같다.
              2. TBC를 만든다. (this로 참조할 오브젝트를 바인딩 시키는 것)
              3. 함수코드에서 var title을 만나게되면 ER의 DER의 title:"JS책"을 프로퍼티 형태로 설정
              4. 또한 OER도있지만 나중에 다루자.
              5. show 밖의 변수를 값으로 사용할 수 있다. 이 원리는 show Function Object를 만들때 [[Scope]]에 설정된 것을 OLER에 설정해놨기 때문이다. (point, getPoint)
              6. 함수 밖에있는것이지만 하나의 show 실행 컨텍스트(하나의 덩어리)이기 때문에 내 것처럼 쓸수있는것이다.
              7. this로 참조할 수 있는것은 TBC에 바인딩 시킨다.
              8. show() 오브젝트는 Global Object이다. 하지만 Global Object는 실체가없기때문에 Host Object 개념으로 window 오브젝트를 참조하게된다.

              >  즉, 실행 콘텍스트안에 함수에서 구할 수 있는 값의 덩어리를 만들어 놓는것이다. 
              > 따라서 함수가 메모리위에 올라갔을때, 실행될때 함수에서 다른 값을 구하려고 메모리에서
              > 빠져나오거나 메모리 올리거나 할 필요가없다는 것. 하나의 Context개념으로 묶어놨기 때문이다.

              **:sparkles: :sparkles:이것이 자바스크립트의 Lexical Context(정적 컨텍스트) 개념이다. 이러한 환경을 정적으로 만든다는 것. 이에 맞추어 코드를 작성하면 js엔진이 심플하고 빠르게 처리할 수 있을것이다. 그래서 실행 컨텍스트를 알아야한다.**

            





#### L4 - 식별자 해결

---

1. Identifier Resolution

   : 사용할 변수/함수를 결정하는 것. Resolution의 사전적의미는 해결, 결정이다.

   ```javascript
   var point = 100;
   function getPoint(){
       var point = 200;
       return point;
   };
   
   var result = getPoint();
   console.log(result);
   ```

   1. getPoint() 를 호출하면 point 변수에 값이 200으로 초기화되고 return한다.
   2. 근데 왜 100이아닌 200이 return 될까? Scope 때문이다.
   3. Scope에 설정된 범위안에서 point 변수가있으면 그 값을 할당하고 없으면 그 상위 함수에서 찾아 할당하는 식
   4. 스코프에 이름을 설정한다. 스코프안에서 찾을수있도록. 스코프에 설정된 이름은 변경되지않는다. 단 값으 변경가능하다. 즉, 식별자 해결대상은 이름이다.

2. Scope 용도

   :식별자 해결을 위한 수단과 방법이다. (스코프가 목적이 아님)



#### L5 - Scope Chain / 스펙의 Scope Chain 사용

---

1. .ES3 - Scope Chain

   * 식별자를 검색하기 위한 Object({name:value}) 리스트이다. 
     실행 콘텍스트(Execution Context)와 관련이 있고 식별자 해결(Identifier Resolution)을 위해 사용한다
     (다만 ES5는 Scope는 사용하지만 Scope Chain은 사용하지 않는다.)

   * 1. 함수가 호출되면 Scope를 생성 
     2. 함수의 변수, 함수를 {name: value} 형태로 설정 
     3. Scope Chain에 연결
     4. Scope Chain에서 식별자를 해결 

   * 동적(Dynamic)  처리이다. (ES5는 Lexical이며 Lexical한 화면이 더 빠르다.)

   * ES3 실행 콘텍스트 환경 

     * Scope Chain

     * Activation Object : 함수가 실행될 때 실행한 결과를, 그리고 실행하기 위한 환경을 제공.

       (ES5에는 Activation Object가없고 이에 대응하는 Lexical Environment가 있다.)

2. ES5 - Lexical Enviroment의 Declarative Environment Record에 함수의 변수와 함수 이름을 바인드

   (Scope Chain을 사용하지 않으며 DER에서 변수와 함수 이름을 검색)

   [참고 내용]

   ``` javascript
   show 실행 콘텍스트(EC): {
   	렉시컬 환경 컴포넌트(LEC): {  //1
           환경 레코드(ER): {  //3
               선언적 환경 레코드(DER): {
   				title: "JS책"
               },
               오브젝트 환경 레코드(OER): {}
           },
           외부 렉시컬 환경 참조(OLER):{
               point: 123,
               getPoint: function(){}
           }
       },
       변수 환경 컴포넌트(VEC): {},  //1
       this 바인딩 컴포넌트(TBC): {  //2
           글로벌 오브젝트(window)
       }
   }
   ```

3. Scope Chain은 Scope외에 또다른 Scope를 하나의 Context 개념으로 가져가야하는데 이것을 완전한 하나의 Context 개념으로 보기엔 무리가있다.
4. 하지만, ES5는 완전하게 하나의 콘텍스트 개념으로 가져간다. 이것이 중요한 이유는 함수가 호출되어 메모리에 올라갈때 Context 하나만 올라가면 된다는 이야기이다.
5. 물론 여기에 맞춰 우리가 프로그램 코드를 작성해야한다는 전제가있지만 아키텍쳐가 받쳐준다는건 이야기가 다르다. ES3는 아키텍쳐가 받쳐주지 못했고, ES5는 완전하게 정적환경에서 아키텍쳐가 받쳐준다.



#### L6 - Lexical Environment / var 키워드 문제와 해결 / 동적 환경

1. ES5의 정적환경 (Lexical Environment)

   * 함수가 호출될때 Scope가 설정되는것이 아니라, 
     function 키워드를 만나면 function Object를 생성하고, 스코프가 FO(Function Object)의 [[Scope]]에 결정되는것 (=함수 안이 아니라 함수 밖의 Scope가 결정되는 것)

     ``` javascript
     var point = 123;
     function book(){  // function 키워드를 만나면 Scope가 결정된다.
         function getPoint(){};
     };
     book(); // 호출할때 Scope가 설정되는것이 아니라
     ```

     **그래서 동적이아니라 정적인 것이다.**

   * 함수가 호출되면 ??
     : FO에 설정된 [[Scope]]를 실행 콘텍스트(Execution Context)의 
     렉시컬 환경 컴포넌트(LEC)의 외부 렉시컬 환경 참조(OLER)에 설정한다.

2. ES5의 var 키워드 문제

   * 함수에서 var 키워드를 사용하지않고 변수를 선언하면 Global Object에 설정된다.
     Lexical 환경 구조에 맞지않다.
   * 왜냐하면 지금 Lexical 환경은 함수 안에있는것과 함수 밖에있는 것. 두 개 단계의 계층만 갖고있는데 이것을 몇단계 올라간다. 그리고 그것을 끌고다녀야한다면. Lexical 환경 구조에 맞지 않는 것이다.
   * 그래서 ES5에선 "use strict" 사용으로 그것을 해결한다. use strict 미사용시 var 없이 변수선언을하면 에러 처리된다.
   * ES5에선 use strict 사용이 필수아닌 필수이다. 
   * ES6에선 let, const를 사용해 좀 더 근본적으로 var 변수를 사용하지 않아서 생기는 정적 환경 구조의 문제를 해결. (변수 자체에 스코프 제약을 두어 lexical 환경을 유지하겠다는 것)

3.  동적환경

   * 실행 시점에 스코프 결정. 

     * with 문

       strict 모드에서 에러 발생

     * eval() 함수

       보안에 문제가 있음



#### L7 - Node.js 코드 형태

---

1. Node.js 코드 형태

   * 서버 프로그램 고려 사항 : 동시 접속자 10K (10,000명)

   * JS는 싱글 스레드이다. (하나의 함수가 호출돼 끝나야 다른 함수를 부를 수 있다.)

   * 그런데 Node.js에서 JS는 비동기 처리이다.

     > 하나의 함수가 호출되어 끝나기전에 다른 함수가 시작되는것을 말한다.
     >
     > 순차적으로, 동기적으로 처리하지않게된다.
     >
     > (C++의 semapore, Mutex때문에 비동기처리를 한다.)

   * 이러한 환경에서 Context 형태가 효율성이 높다. 

     - ES3는 Scope Chain과 Activation Object가 메모리에 올라가게된다.
       이것을 실행하는도중에 다른 함수가 호출되면 그 함수도 Scope Chain과 Activation Object를 갖고 올라오게된다. 그러면 그 두개가 왔다갔다하게된다.
     - 만약 이것을 동접 10k에서 실행하게된다면 ?  엄청난 부담.
     - 반면, ES5의 실행 콘텍스트(Execution Context)는 Scope Chain이 없이 딱 하나이다.
       그래서 엔진처리가 더 빠르다

 ✨**실행 콘텍스트에 최적화된 형태로 코드를 작성해야하며 이를 위해 엔진 처리를 이해할 필요가있다.**



#### L8 - 강좌 범위

---

1. 자바스크립트(ES5) 엔진 처리 중심이고 ES5 스펙의 모든 항목을 하나도 빠짐없이 다룬다.
2. 각 항목의 바탕이되는 개념과 이로 인해 파생되는 활용을 다룬다.
3. 기능보다 논리에 중점을 두고 접근한다.
   * Function Object 구조와 생성
   * ✨**Execution Context**
   * ✨**Identifier Resolution**, ✨**Scope**
   * ✨**this**, prototype, OOP
   * Hoisting, Overloading, Argument
   * Recursive, Immediately Invoked Function Expression, Closure



### [SECTION2 function 오브젝트]

---





#### L9 - function 형태 / function 오브젝트 생성 / 오브젝트 저장 / 생각의 전환

---

1. function 형태

   * 빌트인 Function 오브젝트 
     - Function.prototype.call()  : 빌트인 Function 오브젝트 prototype의 메소드와 연결된 형태
   * function 오브젝트
     * function book() {...} : function 키워드를 사용해 함수를 선언
     * var book = function(){...}: function 키워드를 사용해 function 오브젝트를 만들어 변수에 할당하는 형태
     * 인스턴스지만, new 연산자로 생성한 인스턴스와 구분하기위해 강좌에서 function 오브젝트로 표기
   * function 인스턴스
     * new Book(): new 연산자를 사용하여 Book.prototype에 연결된 메소드로 생성

2. function 오브젝트 생성

   ```javascript
   var book = function(){...};
   ```

   * 엔진이 function 키워드를 만나면,
     빌트인 Function 오브젝트의 prototype에 연결된 메소드로 function 오브젝트를 생성한다.
   * 생성한 오브젝트를 book 변수에 할당.
   * book() 형태로 호출. (코드는 단지 Text이다. function 키워드를 만나 엔진이 function 오브젝트를 만들었기때문에 호출할 수 있는것이다. )

3. 오브젝트 저장

   * 함수를 호출하려면 생성한 function 오브젝트를 저장해놔야한다.
   * function 오브젝트 저장형태는 {name:value} 형태이다.
     ex) {book: 생성한 function 오브젝트} 형태이다.
   * 함수를 호출하면
     * 저장된 오브젝트에서 함수 이름(book)으로 검색
     * value값을 구하고
     * value가 function 오브젝트이면 호출한다. (number이면 연산을하고 String이면 문자열처리를 할수있고 등등 ...)

4. 생각의 전환

   * 함수가 호출되면,
     * 엔진은 함수의 변수, 함수를 {name: value} 형태로 실행 환경을 설정한다. 
     * 그리고 함수코드를 실행한다.
   * function(){} 코드를 보면 함수의 변수와 함수가 {name:value} 형태로 연상되어야한다. (엔진 관점)

✨프로퍼티({name:value}) 형태로 생각을 전환해야 JS의 아키텍처와 메커니즘을 쉽게 이해할 수 있다. 
내가 만약 엔진이 된것처럼 key, value({name:value})관점에서 보는것이다.



#### L10 - function 오브젝트 생성 과정 / function 오브젝트 구조

---

1. function 오브젝트 생성 과정

   * function sports(){...} 형태에서 function 키워드를 만나면
   * 오브젝트를 생성하고 저장한다. ({sports:  {...}}의 형태 )
   * sports는 function 오브젝트 이름,   오브젝트{...}에 프로퍼티가 없는 상태이다.

   ```javascript
   >sports: f ()
   	arguments: (...)
       caller: (...)
   	name: "sports"
      >prototype: {constructor: f}
   	 >__proto__:          // 빌트인 Object 관련 메소드
          >constructor: f Object()
          >hasOwnProperty: f hasOwnProperty()
          >propertyIsEnumerable: f hasOwnProperty()
          >toLocaleString: f hasOwnProperty()
          >toString: f hasOwnProperty()
          >valueOf: f hasOwnProperty()
   		...
      >__proto__: f ()       // 빌트인 Function 관련 메소드
       >apply: f apply()
        arguments: (...)
       >bind: f bind()
       >call: f call()
       ...
       [[FunctionLocation]]: ...
      >[[Scopes]]: Scopes[1]
     >this: Window
   >Global
                  
   ```

   * 위와 같은 구조의 function 오브젝트 생성과정에대해 알아보자

     ```javascript
     sports = {
         prototype: {
             constructor: sports
             __proto__: {}
         }
     }
     ```

     1. sports 오브젝트에 prototype 오브젝트 첨부

     2. prototype에 constructor 프로퍼티를 첨부한다.
        * prototype.constructor가 sports 오브젝트 참조
     3. prototype에  __ proto __ 오브젝트 첨부
        * ES5 스펙에 __ proto __가 기술되어 있지 않으며 ES6 스펙에 기술되어있다.
          그리고 엔진이 사용한다는 늬앙스로 정의되어있다.
        * ES5 기준으로 보면 표준이 아니지만 2000년대 초부터 파이어폭스에서 사용했다.

     4. 빌트인 Object.prototype의 메소드로 Object 인스턴스를 생성하여
        prototype.__ proto __에 첨부한다.
     5. sports 오브젝트에 __ proto __ 오브젝트를 첨부한다. (sport.__ proto __)
     6. 빌트인 Function.prototype의 메소드로 function 인스턴스를 생성하여
        sports.__ proto __에 첨부한다.
     7. sports 오브젝트 프로퍼티에 초기값 설정
        * arguments, caller, length, name 프로퍼티]

2. function 오브젝트 구조

   * function 오브젝트에 prototype이 있으며 constructor가 연결된다.

     * __ proto __ 가 연결되어 있으며 Object 인스턴스가 연결된다.

   * function 오브젝트에 __ proto __가 있으며 Function  인스턴스가 연결된다.

     * Array이면 Array 인스턴스가 연결되고 String이면 String 인스턴스가 연결된다.

     ``` javascript
     sports = {
         arguments: {},
         caller: {},
         length: 0,
         name: "sports",
         prototype: {
             constructor: sports,
             __proto__: Object.prototype
         },
         __proto__:Function.prototype //String.prototye,   Array.prototype ...
     }
     ```

✨**이름(키):value 형태로 바라보자!**



#### L11 - 함수 실행 환경 인식 / 함수 실행 환경 저장 / 내부 프로퍼티

1. 함수 실행 환경 인식
   * 함수 실행 환경 인식이 필요한 이유는 ? 
     * 함수가 호출되었을 때 실행될 환경을 알아야 실행 환경에 맞춰 실행할 수 있기 때문
   * function 키워드를 만나 function 오브젝트를 생성할 떄 실행 환경 설정을 한다.
   * 설정하는 것은 실행 영역(함수가 속한 Scope (Lexical Scope)), 파라미터, 함수 코드
2. 함수 실행 환경 저장
   * function 오브젝트를 생성하고 바로 실행하지 않는다.
   * 함수가 호출되었을 때 사용할 수 있도록 환경을 저장한다.
   * 저장하는 위치는 생성한 function 오브젝트이다.
   * 인식한 환경을 function 오브젝트의 내부 프로퍼티에 {name: value} 형태로 저장한다.
3. 내부 프로퍼티
   * 엔진이 내부 처리에 사용하는 프로퍼티. 스펙 표기로 외부에서 사용 불가
   * [[ ]] 형태.   ( ex. [[Scope]])



#### L12 - 내부 프로퍼티 분류: 공통 내부 프로퍼티 /  선택적 내부 프로퍼티

---

1. 내부 프로퍼티 분류
   * 공통 프로퍼티 
     * 모든 오브젝트에 공통으로 설정되는 프로퍼티
   * 선택적 프로퍼티
     * 오브젝트에 따라 선택적으로 설정되는 프로퍼티
     * 해당되는 오브젝트에만 설정
2. 공통 내부 프로퍼티
   * [[Prototype]] : 오브젝트의 prototype   (Object 또는 Null)
   * [[Class]] : 오브젝트 유형 구분 (String)
   * [[Extensible]] : 오브젝트에 프로퍼티 추가 가능 여부 (Boolean)
   * [[Get]] : 이름의 프로퍼티 값 (any)
   * [[GetOwnProerty]] : 오브젝트 소유의 프로퍼티 디스크립터 속성 (프로퍼티 디스크립터)
   * [[GetProperty]] : 오브젝트의 프로퍼티 디스크립터 (프로퍼티 디스크립터)
   * [[Put]] : 프로퍼티 이름으로 프로퍼티 값 설정
   * [[CanPut]] : 값 설정 가능 여부 (Boolean)
   * [[HasProperty]] : 프로퍼티의 존재 여부 (Boolean)
   * [[Delete]] : 오브젝트에서 프로퍼티 삭제 가능 여부 (Boolean)
   * [[DefaultValue]] : 오브젝트의 디폴트 값 (any)
   * [[DefinedOwnProperty]] : 프로퍼티 추가, 프로퍼티 값 변경 가능 여부 (Boolean)
3. 선택적 내부 프로퍼티
   * [[PrimitiveValue]]: Boolean, Date, Number, String 오브젝트에서만 제공 (프리미티브 값)
   * [[Construct]]: new 연산자로 호출되며 인스턴스를 생성한다. (Object)
   * [[Call]]: 함수 호출 (any)
   * [[HasInstance]]: 지정한 오브젝트로 생성한 인스턴스 여부 (instance of 연산자로 생각하면 쉬움) (Boolean)
   * [[Scope]]: Function 오브젝트가 실행되는 렉시컬(정적) 환경 (렉시컬 환경)
   * [[FormalParameters]]: 호출된 함수의 파라미터 이름 리스트 (문자열 리스트)
   * [[Code]]: 함수에 작성한 JS 코드 설정, 함수가 호출되었을 때 실행 (JS 코드)
   * [[TargetFunction]]: Function 오브젝트의 bind()에 생성한 타깃 함수 오브젝트 설정 (Object)
   * [[BoundThis]]: bind()에 바인딩된 this 오브젝트 (any)
   * [[BoundArguments]]: bind()에 바인딩된 아규먼트 리스트 (리스트)
   * [[Match]]: 정규표현식의 매치 결과 (매치 결과)
   * [[ParameterMap]]: 아규먼트 오브젝트와 함수의 파라미터 매핑 (Object)



#### L13 - 함수 정의 형태: 함수 정의 / 함수 선언문 / 함수 표현식

---

1. 함수 정의 (Function Definition)
   * 함수 코드가 실행될 수 있도록 JS 문법에 맞게 함수를 작성하는 것
   * 함수 정의 형태
     * 함수 선언문(Function Declaration)
     * 함수 표현식(Function Expression)
     * new Function(param, body) 문자열로 작성

2. 함수 선언문

   | 구분      | 타입     | 데이터(값)               |
   | --------- | -------- | ------------------------ |
   | function  | Function | function 키워드          |
   | 식별자    | String   | 함수 이름                |
   | 파라미터  | Any      | 파라미터 리스트 opt      |
   | 함수 블록 | Object   | {실행 가능 코드 opt}     |
   | 반환      | Function | 생성한 Function 오브젝트 |

   ```javascript
   function book(one, two){
   	return one + ", " + two;
   };
   log(book("JS", "DOM"));
   ```

   * function getBook(title){함수 코드}
     * function, 함수이름, 블록{} 작성은 필수
     * 파라미터, 함수 코드는 선택
   * 엔진이 function 키워드를 만나면 function  오브젝트를 생성하고
     함수 이름을 function 오브젝트 이름으로 사용

3. 함수 표현식

   ```javascript
   var getBook = function(title){
       return title;
   };
   getBook("JS책");
   ```

   * var getBook = function(title){함수 코드}
     * function 오브젝트를 생성하여 변수에 할당
     * 변수 이름이 function 오브젝트 이름

   ```javascript
   var getBook = function inside(value){
       if(value === 102){
           return value;
       };
       console.log(value);
       return inside(value+1);
   };
   getBook(100);
   ```

   * 식별자 위치의 함수 이름은 생략 가능
   * var name = function abc(){} 에서 abc가 식별자 위치의 함수 이름
     (최근에는 이러한 형태로 안쓴다)



#### L14 - 엔진 해석 방법: 엔진 해석 순서 / 함수 코드 작성 형태 / 엔진 처리 상태

---

1. 엔진 해석 순서

   * 자바스크립트는 스크립팅 언어다. 
     스크립팅 언어는 작성된 코드를 위에서부터 한줄씩 해석한다. 하지만!

   * 자바스크립트는 중간에 있는 코드가 먼저 해석될수도있다.

   * 첫번째, 함수 선언문을 순서대로 해석한다.

     function sports(){};

   * 두번째, 표현식을 순서대로 해석한다. (표현식 = 변수 형태로 할당한 것)

     var value = 123;    (정수 표현식?)

     var book = function(){};     (함수 표현식)

2. 함수 코드 작성 형태

   ``` javascript
   function book(){
   	debugger;
       var title = "JS책";
       function getBook(){
           return title;
       };
       var readBook = function(){};
       getBook();
   };
   
   book();
   ```

   * 마지막 줄에서 book 함수 호출 (  book(); )
   * title 변수 선언 (var title = "JS책"; )
   * 함수 선언문 작성 (function getBook(){return title;})
   * 함수 표현식 작성 (var readBook = function(){};)

3. 엔진 처리 상태

   ``` javascript
   function book(){
   	console.log(title);
       console.log(readBook);
       console.log(getBook);
       debugger;
       var title = "JS책";
       function getBook(){
           return title;
       }
       var readBook = function(){};
       getBook();
   }
   book();
   
   //실행결과
   // undefined
   // undefined
   // function getBook(){ return title; }
   ```

   * undefined도 해석을했다는 뜻이다. 
     해석을 했다라는 뜻은 Scope에 식별자해결을위해 등록했다는 뜻이다.
   * 함수 선언문은 값이 전체가 등록되어있고,
     title과 readBook은 이름은 정상적으로 등록되어있는데 값은 아니다라는 것.
   * 자바스크립트는 프로퍼티를 등록할때 이름만 있으면 값을 자동적으로 undefined로 설정해버린다. 그래야 name, value의 구조가 맞기 때문이다.
   * 프로퍼티로 등록하는데 값이없고 이름만등록할수는 없다. 그럴때 undefined를 사용한다. 시멘틱 그대로 defined 하지 않았다는 뜻.
   * 이것이 바로 자바스크립트 엔진이 프로퍼티를 등록하는 기준이다.



#### L15 - 함수 코드 해석 순서

---

1. 함수 코드 해석 순서

   ``` javascript
   function book(){
       debugger;
       var title = "JS책";
       function getBook(){
           return title;
       };
       var readBook = function(){};
       getBook();
   };
   book();
   ```

   * 함수 선언문 해석
     * function getBook(){};

   * 변수 초기화
     * var title = undefined;
     * var readBook = undefined;
   * 코드 실행
     * var title = "JS책";
     * var readBook = function(){};
     * getBook();



#### L16 - Hoisting / 함수 앞에서 호출 

---

1. 호이스팅

   ``` javascript
   var result = book();
   console.log(result);
   
   function book(){
   	return "호이스팅";
   };
   book = function(){
       return "함수 표현식";
   }
   
   [실행 결과]
   >> 호이스팅
   ```

   * 함수 선언문은 초기화 단계에서 function 오브젝트를 생성하므로 어디에서도 함수를 호출할 수 있다.
   * 함수 앞에서 호출 가능하다. (호이스팅)
   * 초기화 단계에서 값이 있으면 초기화하지 않는다.
     (book이 이미 있으므로 undefined 하지않는다.)

   2. 코딩 연습

   ``` javascript
   // 1. 함수 선언문, 함수 호출(), 함수 선언문
   function book(){
       function getBook(){
           return "책1";
       };
       console.log(getBook());
       function getBook(){
           return "책2";
       };
   };
   book();
   
   // 2. 함수 표현식, 함수 호출(), 함수 표현식
   function book(){
       var getBook = function(){
       	return "책1";
       };
       console.log(getBook());
       var getBook = function(){
           return "책2";
       }    
   }
   book();
   
   // 3. 함수 선언문, 함수 호출(), 함수 표현식
   function book(){
   	function getBook(){
           return "책1";
       }
       console.log(getBook());
       var getBook = function(){
   		return "책2";
       }
   }
   book();
   
   // 4. 함수 표현식, 함수 호출(), 함수 선언문
   function book(){
       var getBook = function(){
       	return "책1";
       };
       console.log(getBook());
       function getBook(){
           return "책2";
       }    
   }
   book();
   
   ```

   

#### L17 - 오버로딩

---

1. 오버로딩

	``` javascript
   function book(one){};
   function book(one, two){};
   function book(one, two, three){};
   
   book(one, two);
   ```
   
   * 함수 이름이 같더라도 파라미터 수 또는 값 타입이 다르면 각각 존재
   * 함수를 호출하면 파라미터 수와 값 타입이 같은 함수가 호출된다.
   * JS는 오버로딩을 지원하지 않는다.
   * JS는 파라미터 수와 값 타입을 구분하지 않고 {name: value} 형태로 저장하기 때문이다.
   
2. 오버로딩 미지원: 함수 선언문 초기화

   ``` javascript
   function book(){
   	function getBook(){
   		return "책1";
   	};
   	getBook();
   	function getBook(){
   		return "책2";
   	};
   };
   book();
   ```

   * 마지막 줄에서 book() 함수 호출시
   * function getBook(){return "책1";}을 만나 getBook 오브젝트를 생성
   * getBook()호출 하지 않고 아래로 이동
   * function getBook(){return "책2";}를 만나 getBook 오브젝트를 생성
   * 위의 오브젝트와 이름이 같으므로 마지막에 만났던 getBook 오브젝트로 대체된다.
   * {name: value} 형태에서 name이  같으므로 value가 변경된다.

3. 오버로딩 미지원: 변수 초기화

   * getBook() 함수를 호출하면, return "책2"의 getBook 함수가 실행된다.
   * 함수 이름이 같으므로 위의 함수가 아래 함수로 대체되었기 때문.
   * "책2"가 실행결과에 출력된다.

✨**자바스크립트는 오버로딩을 지원하지않는다 {key:value}, {name:value} 의 프로퍼티인것이다. 이것이 자바스크립트의 가장 기본이되는 구조이다.**



### [SECTION3 argument]

#### L18 - Argument 처리 메커니즘 / Argument 처리 구조 / 엔진의 파라미터 처리

---

1. Argument 처리 구조

   ```javascript
   function get(){
   	return arguments;
   };
   console.log(get('A','B'));
   
   //실행 결과
   // {0:A, 1:B}
   ```

   * 파라미터를 {key: value} 형태로 저장한다.
   * 근데 key가 없기때문에 파라미터 수만큼 0부터 인덱스를 부여한다.
   * 이러한 형태를 Array-like라고 한다. 
     * key값이 0부터 1씩 증가하며 (0,1,2,3,4,...)
     * length 프로퍼티가 있어야한다. (for문을 돌릴 수 있다는 뜻)

2. 엔진의 파라미터 처리

   ``` javascript
   var get = function(one){
   	return one;
   };
   get(77,100);
   ```

   * get() 함수를 호출하면서 77과 100을 파라미터 값으로 넘겨준다.
   * 넘겨받은 값을 함수의 파라미터 이름에 설정한다.
     * 정적 환경의 선언적 환경 레코드에 설정한다.
     * one:77
   * Argument 오브젝트를 생성한다
   * 넘겨받은 파라미터 수를 Argument 오브젝트의 length 프로퍼티에 설정
   * 파라미터 수만큼 반복하면서 0부터 key로 하여 순서대로 파라미터 값을 설정
     * {0: 77}, {1: 100} 형태가 된다.
   * 함수의 초기화 단계에서 실행한다.

### [SECTION4 스코프]

#### L19 - 스코프

---



1. 스코프 목적

   * 범위를 제한하여 식별자를 해결하려는 것
     * 식별자 해결(Identifier Resolution) 
       * 변수 이름, 함수 이름을 찾는 것
       * 이때 스코프를 사용한다
       * 이름을 찾게 되면 값을 구할 수 있다.
       * 크게는 이름을 설정하는 것도 식별자 해결이다.
   * 식별자 해결을 위해 스코프가 있는 것 (스코프를 위해 식별자해결이 있는게 아님)

2. 스코프 설정

   ``` javascript
   function book(){
   	var point = 123;
     function get(){
   		console.log(point);
     };
     get();
   };
   book();
   ```

   * 엔진이 function book(){} 을 만나면 function 오브젝트를 생서한다.
   * 그리고 스코프를 설정한다
     * 생성한 function 오브젝트의 [[Scope]]에 스코프를 설정한다.
     * 즉, 이때 스코프가 결정된다.
   * 이러한 것을 정적 스코프라고한다.
     (function 오브젝트를 만드는 시점에 스코프를 결정하는 것)
   * 한편 함수를 함수를 호출할 때 스코프를 결정하는 것은 동적스코프
     (만약 for문으로 1만번을 돌면 1만번 결정해줘야한다. 즉 비효율적)

 ✨**여기서 [[Scope]]는 영역, 범위의 개념으로 get() 함수가 속한 영역이다. 그러므로 Scope범위내의 함수(var point)에도 get함수는 접근 할 수 있다.**



#### L20 - Global 오브젝트 / Global 오브젝트 특징

---

``` javascript
var value = 100;
function book(){
	var point = 200;
  return value;
};
book();
```

1. 글로벌 오브젝트
   * 100을 value변수에 할당한 것은 value로 검색하여 겂을 사용하기 위한 것
   * 함수 안에 변수를 선언하면 변수가 함수에 속하게되지만, value 변수는 ? 어느 함수에도 포함되어있지않다.
   * value 변수가 속하는 오브젝트가 없다는 뜻이다. 이 때, global 오브젝트에 설정된다.
   * 이런 메커니즘을 구현할 수 있는 것은 global 오브젝트가 하나만 있기 때문이다.
2. 글로벌 오브젝트 특징
   * Global 오브젝트는 JS 소스파일 전체에서 하나만있다.
   * 그래서 new 연산자로 인스턴스 생성 불가하다.
     (만약 new 연산자로 생성 가능하면 하나만 있다는 전제가 깨지는 것이다.)
   * JS 소스 파일 전체 = <script>에 작성된 모든 코드



#### L21 - Global 스코프

---

1. 글로벌 스코프

   * 오브젝트는 함수와 변수를 작성 (개발자 관점) 
   * 스코프는 식별자 해결을 위한 것 (엔진 관점) 

   * 글로벌 오브젝트가 글로벌 스코프이다 (Window를 사용하기도함)
   * 글로벌 스코프는 최상위 스코프이며 함수에서보면 최종 스코프이다.

2. 글로벌 스코프 (코드)

   ``` javascript
   var value = 100;
   function book(){
   	return value;
   };
   book();
   ```

   * function book(){코드} : book 함수가 속한 오브젝트가 없으므로 book 함수가 글로벌 오브젝트에 설정된다. (글로벌 함수)
   * var value =100; : value 변수가 글로벌 오브젝트에 설정된다. (글로벌 변수)
     * 글로벌 오브젝트에 설정된다 (오브젝트 관점)
     * Scope가 글로벌 스코프 (식별자 해결 관점)
   * book();  : book 함수를 호출하려면 오브젝트.book() 형태로 작성해야하는데 오브젝트를 작성하지 않고 함수만 작성한다.
   * 오브젝트를 작성하지않으면 글로벌 오브젝트를 오브젝트로 간주한다.
     즉, 글로벌 오브젝트의 book() 함수를 호출하게된다.
     (여기서의 식별자해결 : 글로벌오브젝트의 book() 함수를 찾아서 호출하는 것.)

✨**글로벌 오브젝트는 실체가 없다. 하지만 HOST 오브젝트 개념으로 window 오브젝트를 사용한다. 따라서 window 오브젝트의 book함수를 호출한다고도 할 수 있다.**



#### L22 - 스코프 바인딩 / 정적,동적 바인딩 / 바인딩 시점의 중요성

---

1. 바인딩
   * 구조적으로 결속된 상태로 만드는 것
   * 바인딩의 목적은 스코프 설정, 식별자 해결
   * 바인딩 시점에따라 정적바인딩(lexical, static binding)과
     동적바인딩(dynamic binding)으로 구분한다.

2. 정적, 동적 바인딩

   * 정적(렉시컬) 바인딩
     * 초기화 단계에서 바인딩한다.
     * 함수 선언문 이름을 바인딩한다.
     * 표현식(변수, 함수) 이름을 바인딩한다.
   * 동적(다이나믹) 바인딩
     * 실행할 때 바인딩
     * Eval() 함수, with 문

3. 바인딩 시점의 중요성

   * 바인딩 할 때 스코프가 결정되기 때문

   * function 오브젝트 생성 시점에 스코프를 결정하면 스코프를 function 오브젝트의 [[Scope]]에 설정한다.

   * 한 번 결정된 스코프는 변경되지 않는다. (그래서 정적 스코프인 것)

   * 함수 안의 모든 함수의 스코프가 같다.

     ```javascript
     function book(){
     	var point = 100;
       function add() {point += 200;};
       function get() {return point;};
     }
     ```

   * book 스코프에있는 모든 변수, 함수가 공유할 수 있다. (하나의 스코프안에 있으니까)

   * 이 환경을 내부 프로퍼티인 [[Scope]]에 설정한다. 그리고 이것은 변경되지않기는다.

4. 스코프 바인딩 예시

   ```javascript
   function book(){
     var point = 100;
     function add(param){
   		point += param;
     };
     var get = function(){
       return point;
     };
     add(200);
     console.log(get());
   };
   book();
   ```

   1. 마지막 줄 book() 함수 호출.
      * 초기화 단계에서 함수, 변수 이름을 선언적 환경 레코드에 바인딩한다. 
   2. function add(param){...}
      * function 오브젝트 생성
      * add 함수가 속한 스코프(영역)을 add 오브젝트의 [[Scope]]에 설정
      * add 이름을 선언적 환경 레코드에 바인딩한다.
   3. var point = 100;
      * point 이름을 레코드에 바인딩한다.
   4. var get = function(){...}
      * get 이름을 레코드에 바인딩한다.
   5. 바인딩으로 함수와 변수의 식별자가 해결된다.

   ------ 코드 실행 ------

   6. var point = 100;
      * point 변수에 100 할당
   7. var get = function(){...}
      * Function 오브젝트 생성, get에 할당
      * get 함수가 속한 스코프(영역)를 get 오브젝트의 [[Scope]]에 설정

   ----- add() 함수 호출 -----

   8. add(200) 함수를 호출한다.
   9. point += param;
      * 먼저 선언적 환경 레코드에서 point 이름을 찾는다.
      * point가 없다면 다시 검색한다.
      * 검색 시 add 오브젝트의 [[Scope]]를 스코프로 사용한다.
      * book 오브젝트가 스코프이며 point가 있으므로 값을 더한다.
        (내가 만든 변수가 아니라 함수 밖에있는것도 변경. 함수가 속한 스코프를 내부 프로퍼티인 스코프에 설정해놓고 마치 내 것 처럼 쓰기 때문이다.)

   ----- get() 함수 호출 -----

   8. get() 함수를 호출한다.
   9. return point;
      * 선언적 환경 레코드에 point가 없으므로 다시 검색한다.
      * get 오브젝트의 [[Scope]]를 스코프로 사용한다.
      * book 오브젝트가 스코프이며 point가 있으므로 값을 반환

5. 동적 바인딩
   * 코드를 실행할 때 마다 바인딩
     * with문, eval() 함수
   * with문 
     * 'use strict' 환경에서 에러가 발생한다. (사용하지 말라는 뜻)
   * eval() 함수
     * 보안에 문제가 있다.

### [SECTION5 execution Context]

---

#### L23 - 실행 콘텍스트 / 실행 콘텍스트 상태 컴포넌트

---

```javascript
function music(title){
  var musicTitle = title;
};
music('음악')
```

1. 실행 콘텍스트

   * 함수가 실행되는 영역, 묶음
   * 함수 코드를 실행하고 실행 결과를 저장
   * 스팩상의 사양이다. (즉, 개발자가 실행 콘텍스트의 뭔가를 바꿀수는없고 엔진이 처리하는 부분이다.)
   * music('음악')으로 함수를 호출하면 엔진은 실행 콘텍스트를 생성하고 실행 콘텍스트 안으로 이동한다.
   * 실행 콘텍스트 실행단계
     * 준비, 초기화, 코드실행 단계
   * 실행 콘텍스트 생성 시점
     * 실행 가능한 코드를 만났을 때
   * 실행 가능한 코드 유형
     * 함수 코드, 글로벌 코드, eval 코드
   * 코드 유형을 분리한 이유는 실행 콘텍스트에서 처리 방법과 실행 환경이 다르기 때문이다.
     * 함수 코드: 렉시컬 환경
     * 글로벌 코드: 글로벌 환경
     * eval 코드: 동적 환경

2. 실행 콘텍스트 상태 컴포넌트

   ``` javascript
   실행 콘텍스트(EC): {
     렉시컬 환경 컴포넌트(LEC): {
       //이 안에 프로퍼티 형태로 작성
     },
     변수 환경 컴포넌트(VEC): {},
     this 바인딩 컴포넌트(TBC): {}
   }
   ```

   * 실행 콘텍스트 상태를 위한 오브젝트 (실행 콘텍스트 안에 생성)
     * 렉시컬 환경 컴포넌트 (LEC)
     * 변수 환경 컴포넌트 (VEC)
     * this 바인딩 컴포넌트 (TBC)



#### L24 - 렉시컬 환경 컴포넌트 / 렉시컬 환경 컴포넌트 구성,설정 / 외부 렉시컬 환경 참조 / 변수 환경 컴포넌트

---

1. 렉시컬 환경 컴포넌트

   * 함수와 변수의 식별자 해결을 위한 환경 설정
   * 함수 초기화 단계에서 해석한 함수, 변수를 {name: value} 형태로 저장한다.
   * 변수 {name: undefined}, 함수{name: function 오브젝트}
     즉, 이름으로 함수와 변수를 검색할 수 있게 된다.
   * 함수 밖의 함수와 변수를 참조할 수 있는 환경을 설정한다.
     즉, 함수 밖의 함수, 변수를 사용할 수 있게 된다.

2. 렉시컬 환경 컴포넌트 구성

   ```javascript
   실행 콘텍스트(EC): {
     렉시컬 환경 컴포넌트(LEC): {
   		환경 레코드(ER):{
         point: 100
       },
       외부 렉시컬 환경 참조(OLER): {
         title: "책",
         getTitle: function(){}
       }
     }
   }
   ```

   * 렉시컬 환경 컴포넌트 생성
     * function, with, try-catch에서 생성
   * 컴포넌트 구성
     * 환경 레코드(ER: Environment Record)
     * 외부 렉시컬 환경 참조(OLER: Outer Lexical Environment Reference)

3. 렉시컬 환경 컴포넌트 설정

   * 환경 레코드(ER)에 함수 안의 함수와 변수를 기록
   * 외부 렉시컬 환경 참조(OLER)에 function 오브젝트의 [[Scope]]를 설정한다.
   * 따라서, 함수 안과 밖의 함수와 변수를 사용할 수 있게 된다.

4. 외부 렉시컬 환경 참조

   * 스코프와 실행중인 함수가 Context 형태이다.
   * 스코프의 변수와 함수를 별도의 처리없이 즉시 사용할수있다.
   * 실행 콘텍스트에서 함수 안과 밖의 함수, 변수를 사용할 수 있으므로 함수 변수를 찾기위해 실행 콘텍스트를 벗어나지 않아도 된다.

5. 변수 환경 컴포넌트

   ```javascript
   실행 콘텍스트(EC): {
   	렉시컬 환경 컴포넌트(LEC): {},
     변수 환경 컴포넌트(VEC): {},
     this 바인딩 컴포넌트(TBC): {}
   }
   ```

   * 실행 콘텍스트 초기화 단계에서 렉시컬 환경 컴포넌트와 같게 설정한다.
   * 초기값을 복원할때 사용하기 위한 것이다. 
     * 함수코드가 실행되면 실행결과를 LEC에 설정한다. 
     * 초기값이 변하게 되므로 이를 유지하기 위한 것.



#### L25 - 실행콘텍스트 실행 과정

``` javascript
var base = 200;
function getPoint(bonus){
  	var point = 100;
  	return point + base + bonus;
}
console.log(getPoint(70));
```

1. 실행 과정

   * getPoint 오브젝트의 [[Scope]]에 글로벌 오브젝트를 설정

   * getPoint() 함수 호출

   * 엔진은 실행 콘텍스트 생성 -> 실행 콘텍스트 안으로 이동

   * ---준비단계---

   * 컴포넌트를 생성해 실행 콘텍스트에 첨부한다.

     * 렉시컬 환경 컴포넌트
     * 변수 환경 컴포넌트
     * this 바인딩 컴포넌트

     ```javascript
     실행 콘텍스트(EC):{
         렉시컬 환경 컴포넌트(LEC): {},
         변수 환경 컴포넌트(VEC): {},
         this 바인딩 컴포넌트(TBC): {}
     }
     ```

   * 환경 레코드를 생성해 렉시컬 환경 컴포넌트에 첨부

     * 함수 안의 함수, 변수를 바인딩한다.

       ```javascript
       실행 콘텍스트(EC): {
           렉시컬 환경 컴포넌트(LEC) = {
               환경 레코드(ER): {}
           },
           변수 환경 컴포넌트(VEC): {},
           this 바인딩 컴포넌트(TBC): {}
       }
       ```

     * 외부 렉시컬 환경 참조를 생성하여 렉시컬 환경 컴포넌트에 첨부하고 function 오브젝트의 [[Scope]]를 설정한다.

       ```javascript
       실행 콘텍스트(EC): {
           렉시컬 환경 컴포넌트(LEC) = {
               환경 레코드(ER): {},
               외부 렉시컬 환경 참조(OLER): {
                   base: 200
               }
           },
           변수 환경 컴포넌트(VEC): {},
           this 바인딩 컴포넌트(TBC): {}
       }
       ```

   * ------ 초기화 단계 -----

   * 호출한 함수의 파라미터값을 호출된 함수의 파라미터 이름에 매핑 -> 환경 레코드에 작성 (여기까지는 외부 실행 상태를 제공하지 않는다.)

     ```javascript
     실행 콘텍스트(EC): {
         렉시컬 환경 컴포넌트(LEC) = {
             환경 레코드(ER): {
                 bonus: 70,
                 point: undefined
             },
             외부 렉시컬 환경 참조(OLER): {
                 base: 200
             }
         },
         변수 환경 컴포넌트(VEC): {},
         this 바인딩 컴포넌트(TBC): {}
     }
     ```

   * ----- 실행단계 -----

   * 함수 안의 코드를 실행한다. (var point = 100;)

   * 실행 콘텍스트 안에서 관련된 함수와 변수를  사용할 수 있다.

   

#### L26 - 환경 레코드 구성

----

1. 환경 레코드 구성

   ``` javascript
   실행 콘텍스트(EC): {
   	랙시컬 환경 컴포넌트(LEC):{
           환경 레코드(ER): {
               선언적 환경 레코드(DER): { //function 변수, catch 문
                   point: 123
               },
               오브젝트 환경 레코드(OER): { //글로벌 함수,변수, with문 (동적)
                   
               }
           },
           외부 렉시컬 환경 참조(OLER): {}
       },
       변수 환경 컴포넌트(VEC):{},
       this 바인딩 컴포넌트(TBC): {}
   }
   ```

   * 환경 레코드를 구분하는 이유는 기록 대상이 다르기 때문이다.
   * 선언적 환경 레코드
     * DER : Declarative Environment Record
     * function, 변수, catch 문에서 사용한다.
     * 앞 절에서 환경 레코드에 설정한다고 했는데 설명을 위한 것이었다.
       실제로는 여기에 설정한다.
   * 오브젝트 환경 레코드
     * OER: Object Environment Record
     * 글로벌 함수와 변수, with 문에서 사용한다.
     * 정적이 아니라 동적이기 때문이다.

2. 글로벌 환경

   ```javascript
   실행 콘텍스트(EC):{
       글로벌 환경(GE):{ // 글로벌 오브젝트
           환경레코드(ER):{
               오브젝트 환경 레코드: 글로벌 오브젝트
           },
           외부 렉시컬 환경 참조(OLER): null
       }
   }
   ```

   * Global Environment
     * 글로벌 오브젝트에서 사용
     * 렉시컬 환경 컴포넌트와 형태 같음
   * 동적으로 함수와 변수를 바인딩한다.
     * 함수에서 var 키워드를 사용하지않고 변수를 선언하면 글로벌 오브젝트에 선언되기 때문이다.
     * 이런 이유로 오브젝트 환경 레코드를 사용한다.
   * 외부 렉시컬 환경 참조 값은 항상 null이다.



#### L27 - this 바인딩 컴포넌트

---

1. this 바인딩 컴포넌트

   * 목적
     * this로 함수를 호출한 오브젝트의 프로퍼티에 엑세스
       (ex. this.propertyName)
   * 엑세스 매커니즘
     * obj.book() 형태여서 this로 obj를 참조할 수 있도록
     * this 바인딩 컴포넌트에 obj 참조를 설정
   * obj의 프로퍼티가 변경되면 동적으로 참조
     * 설정이 아닌 참조이기 때문이다.

2. 예시

   ```javascript
   var obj = {point: 100};
   obj.getPoint = function(){
   	return this.point;
   };
   obj.getPoint();
   ```

   ```javascript
   실행 콘텍스트:{
   	렉시컬 환경 컴포넌트{
           환경레코드(ER):{
               선언적 환경 레코드(DER):{},
               오브젝트 환경 레코드(OER): {}
           }
       },
       변수 환경 컴포넌트:{
           
       },
       this 바인딩 컴포넌트:{
           point: 100,
           getPoint: function(){}
       }
   }
   ```

   * 준비단계
     * 마지막 줄에서 obj.getPoint() 함수 호출
     * 실행 콘텍스트 생성
     * 3개의 컴포넌트 생성
       렉시컬 환경 컴포넌트 / 변수 환경 컴포넌트 / this 바인딩 컴포넌트
     * this 바인딩 컴포넌트에 getPoint()에서 this로 obj의 프로퍼티를 사용할 수 있도록 바인딩
   * 초기화단계
     * 파라미터, 함수 선언문, 변수 선언 없는 상태
   * 실행 단계
     * return this.point; 실행
     * this 바인딩 컴포넌트에서 point 검색
       (getPoint() 함수를 호출한 오브젝트가 this 바인딩 컴포넌트에 설정(참조)된 상태)
   * 추가설명
     * obj.getPoint()에서 obj의 프로퍼티가 this 바인딩 컴포넌트에 바인딩되도록 의도적으로 설계해야한다.



#### L28 - 호출 스택(call stack)

---

1. 호출 스택
   * call stack
     * 실행 콘텍스트의 논리적 구조
   * First In Last Out 순서
     * 함수가 호출되면 스택의 가장 위에 실행 콘텍스트가 위치한다.
     * 다시 함수 안에서 함수를 호출하면 호출된 함수의 실행 콘텍스트가
       스택의 가장 위에 놓이게 된다.
     * 함수가 종료되면 스택에서 빠져나온다 (FILO)
     * 가장 아래는 글로벌 오브젝트의 함수가 위치해있다.



#### L29 - 파라미터 매핑 / 함수 호출 / 파라미터 값 매핑 / 파라미터 이름에 값 매핑 방법

---

1. 함수 호출

   * 함수가 호출되면 3개의 파라미터 값을 실행 콘텍스트로 넘겨준다.
     * 함수를 호출한 오브젝트
     * 함수 코드
     * 호출한 함수의 파라미터 값
   * 함수를 호출한 오브젝트를 this 바인딩 컴포넌트에 설정해 this로 참조한다.
   * 함수 코드는 function 오브젝트의 [[Code]]에 설정되어 있다.
   * 호출한 함수의 파라미터 값은 호출된 함수의 Argument 오브젝트에 설정한다.

2. 파라미터 값 매핑

   * 파라미터 값 매핑이란 ?
     * 호출한 함수에서 넘겨 준 파라미터 값을 호출된 함수의 파라미터 작성 순서에 맞추어 값을 매핑하는 것
   * 엔진처리 관점에서 실행 콘텍스트로 넘겨준 파라미터 값과 function 오브젝트의 [[FormalParameters]]에 작성된 이름에 값을 매핑하고 결과를 선언적 환경 레코드에 설정하는 것

3. 파라미터 이름에 값 매핑 방법

   ``` javascript
   var obj = {};
   obj.getTotal = function(one, two) {
   	return one + two;
   };
   console.log(obj.getTotal(11,22,77));
   ```

   * getTotal 오브젝트의 [[FormalParameters]]에서 호출된 함수의 파라미터 이름을 구한다.(설명 편의를 위해 name이라고 하자.)
     * name은 ["one", "two"] 형태이다.
     * [[FormalParameters]]는 function 오브젝트를 생성할 때 설정한다.
   * name 배열을 하나씩 읽는다.
   * param에서 index 번째의 값을 구한다.
     * index에 값이 없으면 undefined 반환

   * name의 파라미터 이름과 4번에서 구한 값을 선언적 환경 레코드에



#### L30 - 파라미터 값 할당 기준

---

1. 파라미터 값 할당 기준

   ``` javascript
   var obj = {};
   obj.getTotal = function(one, two){
       var one;
       console.log(one + two);
       two = 77;
       console.log("two:"+ two);
   }
   obj.getTotal(11, 22);
   ```

   * 초기화 단계
     * obj.getTotal(11,22) 함수가 호출되면 파라미터 값을 실행 콘텍스트로 넘겨준다.
     * 파라미터 이름에 값을 매핑하여 선언적 환경 레코드에 설정한다.
       {one: 11, two: 22}
     * var one;
       선언적 환경 레코드에서 one의 존재를 체크한다. 파라미터 이름을 설정하였으므로 존재하며 one을 기록하지 않는다.
     * two = 77;
       선언적 환경 레코드에서 two의 존재를 체크한다. 파라미터 이름을 설정하였으므로 존재하며 two를 기록하지 않는다.
     * 함수에 초기화할 코드가 없으므로 첫째줄로 이동해 함수 코드를 실행한다.
   * 실행 단계
     * 선언적 환경 레코드는 {one: 11, two:22} 상태
     * var one;
       단지, 변수 선언이므로 처리하지 않는다.
     * console.log(one + two);
       선언적 환경 레코드에서 one과 two의 값을 구한다.
       11 + 22의 결과인 33이 [실행 결과]에 출력된다.
     * two = 77;
       값을 할당하는 코드이며 실행 단계이므로 선언적 환경 레코드의 two에 77을 할당한다. ({two:22}가  {two:77} 로 변경)
     * console.log("two: "+ two);
       선언적 환경 레코드에서 two의 값을 구한다. (two: 77이 출력된다.)



### [SECTION6 function instance]

#### L31 - function 인스턴스 기준 /  function 인스턴스 생성

---

1. function 인스턴스 기준

   ``` javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point + 200;
   };
   var obj = new Book(100);
   obj.getPoint();
   
   ```

   * function 부분
     * 빌트인 Function 오브젝트
     * function 오브젝트: function 키워드로 생성
     * function 인스턴스: new 연산자로 생성
   * function 오브젝트도 인스턴스
     * 빌트인 Function 오브젝트로 생성하기 때문
     * 성격적으로는 인스턴스이지만 new 연산자로 생성한 인스턴스와 구분하기 위해 강좌에서는 function 오브젝트로 표기
     * new 연산자로 생성하는 인스턴스는 일반적으로 prototype 프로퍼티를 작성

2. 인스턴스 생성 순서, 방법

   * function Book(point) {...}
     * Book 오브젝트를 생성한다.
     * Book.prototyppe이 만들어진다.
   * Book.prototype.getPoint = function(){}
     * Book.prototype에 getPoint를 연결하고 function(){} 을 할당한다.
     * Book.prototype이 오브젝트이므로 프로퍼티를 연결할 수 있다.
   * var obj = new Book(100);
     * Book()을 실행하며 인스턴스를 생성하고 생성한 인스턴스에 point값을 설정한다.
     * Book.prototype에 연결된 프로퍼티를 생성한 인스턴스에 할당한다.
   * console.log(obj.point);
     * obj 인스턴스에서 프로퍼티 이름으로 값을 구해 출력한다.
   * console.log(obj.getPoint());
     * obj 인스턴스의 메소드를 호출한다
   * return this.point + 200;
     * this가 obj 인스턴스를 참조한다.
   * 강좌의 함수/메소드 사용 기준 (프로토타입에 연결되어있으면 메소드, 아니면 함수)
     * Book()은 함수,    getPoint()는 메소드
     * prototype에 연결한다.



#### L32 - 생성자 함수

---

1. 생성자 함수

   * new 연산자와 함께 인스턴스를 생성하는 함수
     * new Book()에서 Book()이 생성자 함수
   * new 연산자
     * 인스턴스 생성을 제어
     * 생성자 함수 호출
   * 생성자 함수
     * 인스턴스 생성, 반환
     * 인스턴스에 초기값 설정
   * 코딩 관례로 생성자 함수의 첫 문자는 대문자
     * new Number(), new Array(), new Book()

2. 생성자 함수 실행 과정

   * new 연산자로 인스턴스 생성을 제어하고 생성자 함수인 Book()으로 인스턴스를 생성하여 반환한다.

   ```javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point;
   };
   
   var obj = new Book(10);
   ```

   * 엔진이 new 연산자를 만나면 function의 [[Construct]]를 호출하면서 파라미터 값으로 10을 넘겨준다.
   * function 오브젝트를 생성할 때 Book() 함수 전체를 참조하도록 [[Construct]]에 설정했다.
   * [[Construct]]에서 인스턴스를 생성하여 반환한다.
   * 반환된 인스턴스를 new 연산자가 받아 new 연산자를 호출한 곳으로 반환한다.
   * new라는 뉘앙스로 인해 new 연산자가 인스턴스를 생성하는것으로 생각할 수 있지만, function 오브젝트으 [[Construct]]가 인스턴스를 생성한다.
   * 그래서 Book()이 인스턴스 생성자 함수이다.

3. 인스턴스 생성 과정

   * new Book(10) 실행

     * Book 오브젝트의 [[Construct]] 호출
     * 파라미터 값을 [[Construct]]로 넘겨준다.

   * [[Construct]]는 빈 Object를 생성

     * 이것이 인스턴스이다. 지금은 빈 오브젝트{ }이며 하나씩 채워나간다.

   * 오브젝트에 내부 처리용 프로퍼티 설정

     * 공통 프로퍼티와 선택적 프로퍼티가 있다.

   * 오브젝트의 [[Class]]에 "Object" 설정

     * 생성한 인스턴스 타입은 Object이다.

   * Book.prototype에 연결된 프로퍼티(메소드)를 생성한 인스턴스의 [[Prototype]]에 설정한다. (Constructor도 같이 설정된다.)

     ``` javascript
     Book 인스턴스:{
         poin:10,
         __proto__ ={
             constructor: Book,  //외부 프로퍼티로 [[Construct]]와는 다르다
            	getPoint: function(){},
             __proto__: Object
         }
     }
     ```

     

####  L33 - constructor 프로퍼티, constructor 비교

---

1. constructor 프로퍼티

   ```javascript
   Book fucntion 오브젝트:{
       prototype:{
           constructor: Book
       }
   }
   ```

   * 생성하는 function 오브젝트를 참조한다.
     * function 오브젝트를 생성할 때 설정한다.
     * prototype에 연결되어 있다.
   * 개인경험
     * constructor가 없더라도 인스턴스가 생성된다. 하지만, 필요하지 않다는 의미는 아니다.
   * ES5: constructor 변경 불가
     * 생성자를 활용할 수 없다.
   * ES6: constructor 변경 가능
     * 활용성이 높다.

2. constructor 비교

   ``` javascript
   var Book = function(){};
   var result = Book === Book.prototype.constructor;
   console.log("1:", result); //1: true
   
   var obj = new Book();
   console.log("2:", Book === obj.constructor); //2: true
   
   console.log("3:", typeof Book); //3: function
   cossole.log("4:", typeof obj); //4: object
   ```

   * Book === Book.prototype.constructor;
     * [실행결과] 1번에 true가 출력된 것은 Book 오브젝트와 Book.prototype.constructor가 타입까지 같다는 뜻
     * Book 오브젝트를 생성할 때 Book.prototype.constructor가 Book 오브젝트를 참조하기 떄문이다.
   * Book === obj.constructor;
     * obj의 constructor가 Book 오브젝트를 참조하므로 [실행결과] 2번에 true가 출력된다.
   * typeof Book;
     * Book 오브젝트의 타입은 function
   * function 오브젝트를 인스턴스로 생성했더니 object로 타입이 변경되었다.
     * 이것은 [[Construct]]가 실행될 때 생성한 오브젝트의 [[Class]]에 'Object'를 설정하기 때문이다.
   * 오브젝트 타입이 바뀐다는 것은 오브젝트 성격과 목적이 바뀐 것을 뜻한다.
     * 다른 관점에서 접근해야 한다.

✨**인스턴스의 가장 큰 특징은 prototype에 있으며 이 prototype에 많은 메소드들이 연결되는점이다.**



#### L34 - prototype / 상속 / prtotype 오브젝트 목적 / 인스턴스 상속

---

1. prototype 오브젝트 목적

   * prtotype 확장
     * prototype에 프로퍼티를 연결하여 prototype 확장
     * Book.prototype.getPoint = function(){}
   * 프로퍼티 공유
     * 생성한 인스턴스에서 원본 prototype의 프로퍼티 공유
     * var obj = new Book(123);
       obj.getPoint();
   * 인스턴스 상속(Inheritance)
     * function 인스턴스를 연결하여 상속
     * Point.prototype = new Book();

2. 인스턴스 상속

   ```javascript
   function Book(title){ // Book 생성자 함수
       this.title = title;
   };
   Book.prototype.getTitle = function(){ //getTitle 함수를 프로토타입에 연결
       return this.title;
   };
   function Point(title){  // Point 생성자 함수
       Book.call(this, title);
   };
   //Book prototype을 Point prototype에 연결시켜준다.
   Point.prototype = Object.create(Book.prototype, {}); 
   var obj = new Point("자바스크립트");
   console.log(obj.getTitle());
   ```

   * 인스턴스 상속 방법

     * prototype에 연결된 프로퍼티로 인스턴스로 생성하여 상속받을 prototype에 연결
     * 그래서, prototype-based 상속이라고도 한다.

   * JS에서 prototype은 상속보다 프로퍼티 연결이 의미가 더 크다.

     * 인스턴스 연결도 프로퍼티 연결의 하나이다.

   * ES5 상속은 OOP의 상속 기능 부족

     * ES6의 Class로 상속 사용

       ```javascript
       class Book{
           constructor(title){
               this.title =title;
           }
           getTitle(){
               return this.title;
           }
       };
       
       class Point extends Book{
           constructor(title){ //constructor 오버라이딩 
               super(title);
           };
       };
       
       const obj = new Point("자바스크립트");
       console.log(obj.getTitle());
       ```



#### L35 - prototype 확장 방법 / 프로퍼티 연결 고려사항 / constructor 연결 / prototype 확장과 인스턴스 형태

---

1. prototype 확장 방법

   * prototype에 프로퍼티를 연결하여 작성
     * prototype.name = value 형태
   * name에 프로퍼티 이름 작성
   * value에 JS 데이터 타입 작성
     * 일반적으로  function을 사용
   * prototype에 null을 설정하면 확장 불가

2. 프로퍼티 연결 고려사항

   * prototype에 연결할 프로퍼티가 많을때 
     * Book.prototype.name1, 2, 3 ~ n 형태는 Book.prototype을 반복해서 작성해야 하므로 번거롭다.
     * Book.prototype={name1:value, ...} 형태로 작성 가능하다.
   * constructor가 지워지는 문제와 대책
     * {name1:value, ...} 형태로 설정한 후 prototype에  constructor를 다시 연결한다.

3. constructor 연결

   ```javascript
   function Book(){};
   Book.prototype = {
       constructor: Book,
       setPoint: function(){}
   };
   var obj = new Book();
   console.log(obj.constructor);
   ```

   * 오브젝트 리터럴{}을 사용하여 프로퍼티를 연결할때는 constructor가 지워지는 것을 고려해야한다.
   * constructor가 없어도 인스턴스가 생성되지만 constructor가 연결된 것이 엊ㅇ상이므로 코드처럼 constructor에 Book function을 할당한다.

4. prototype 확장과 인스턴스 형태

   * prototype을 확장하는 방법과 생성한 인스턴스 형태를 살펴보자.

   ```javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point;
   };
   var obj = new Book(1000);
   obj.getPoint();
   ```

   ```javascript
   obj:{
   	point: 100,
       __proto__ = {
           constructor: Book,
           getPoint: function(){},
           __proto__: Object
       }
   }
   ```

   ---prototype 확장---

   * function Book(point){};
     * Book 오브젝트 생성
   * Book.prototype.getPoint = function(){}
     * Book.prototype에 getPoint 메소드 연결
   * var obj = new Book(100);
     * 인스턴스를 생성하여 obj에 할당
   * obj.getPoint()
     * obj 인스턴스의 getPoint() 호출
   * 인스턴스를 생성하면 prototype에 연결된 메소드를 인스턴스.메소드이름() 형태로 호출한다.



#### L36 - this와 prototype / this로 인스턴스 참조 / this와 prototype / prototype 메소드 직접 호출

---

1. this로 인스턴스 참조

   * this로 메소드를 호출한 인스턴스 참조
     * var obj = new Book();
     * obj.get() 형태에서 this로 obj 참조
   * 인스턴스에서 메소드 호출 방법
     * prototype에 연결된 프로퍼티가 __ proto__ 에 설정되며 인스턴스 프로퍼티가 된다.
     * this.prototype.setPoint() 형태가 아닌 this.setPoint() 형태로 호출된다.

2. this와 prototype

   ```javascript
   function Book(){
       console.log("1:", this.point);  //1: undefined
   };
   Book.prototype.getPoint = function(){
       this.setPoint();
       console.log("2:", this.point);  //2: 100
   };
   Book.prototype.setPoint = function(){
       this.point = 1 00;
   };
   var obj = new Book();
   obj.getPoint();
   ```

   * console.log("1:", this.point);
     * 생성자 함수에서 this는 생성하는 인스턴스 참조
     * 생성하는 인스턴스에 ppoint 프로퍼티가 없더라도 에러가 나지 않고 undefined를 반환 
   * obj.getPoint();
     * this가 메소드를 호출한 인스턴스 참조
     * 즉, 메소드 앞에 작성한 인스턴스 참조 (obj)
   * this.setPoint();
     * this가 인스턴스 참조하며 인스턴스에 있는 setPoint() 호출
   * this.point = 100;
     * this가 인스턴스를 참조
     * 인스턴스의 point 프로퍼티에 100을 할당

3. prototype 메소드 직접 호출

   ```javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point;
   };
   var obj = new Book(100);
   console.log(obj.getPoint()); //100  (this가 obj를 참조한다.)
   console.log(Book.prototype.getPoint()); //undefined
   ```

   * Book.prototype.getPoint();
     * 인스턴스를 생성하지 않고 직접 메소드 호출
   * Book.prototype을 getPoint()에서 this로 참조
   * obj 인스턴스에는 point가 있지만 Book.prototype에 point가 없으므로 undefined를 반환
     * 인스턴스를 생성하여 메소드를 호출하는 것과 직접 prototype을 작성하여 호출하는 것의 차이

   ✨**인스턴스를 생성하여 메소드를 호출하는것과 직접 prototype을 작성하여 호출하는 것의 차이이다.**

   **물론, Book.prototype을 안쓰는건 아니다. 그러나 바로 메소드를 호출하는게 아닌 call()이나 bind() 등의 메소드를 사용한다.**

   

#### L37 - propertype 프로퍼티 공유 시점

---

1. 프로퍼티 공유 시점

   * 사용하는 시점에 prototype의 프로퍼티를 공유한다
     * 메소드를 호출하는 시점에 프로토타입의 메소드를 공유하는 것이다.
   * prototype의 프로퍼티로 인스턴스를 생성하지만 인스턴스의 프로퍼티는 원본 prototype의 프로퍼티를 참조한다.
     * 복사하여 갖고 있는 개념이 아니다.
   * 인스턴스의 메소드를 호출하면 원본 prototype의 메소드를 호출한다.
   * 원본 prototype에 메소드를 추가하면 생성된 모든 인스턴스에서 추가한 메소드를 사용할 수 있다.
     * 원본 prototype의 메소드를 호출하기 때문이다.

2. 프로퍼티 공유 시점

   ```javascript
   function Book(){
       this.point = 100;
   };
   
   var obj = new Book();
   console.log(obj.getPoint); // undefined
   
   Book.prototype.getPoint = function(){
       return this.point;
   };
   var result = obj.getPoint();
   console.log(result); //100
   ```

   * var obj = new Book();

     * 인스턴스를 생성하여 obj에 할당한다.

   * console.log(obj.getPoint);

     * obj 인스턴스에 getPoint()가 없다.

   * Book.prototype.getPoint = function(){

     ​	return this.point;

     };

     * Book.prototype에 getPoint() 추가
     * 앞에서 생성한 obj 인스턴스에서 getPoint()를 사용할 수 있다.

   * var result = obj.getPoint();

     * 인스턴스를 생성할 때는 obj에 getPoint가 없었지만 getPoint()를 호출하기 전에 Book.prototype에 getPoint를 추가했으므로 호출할 수 있다.

   * retun this.point;

     * 추가하더라도 this가 인스턴스를 참조한다.

   * 이런 특징을 활용하여 상황(필요)에 따라 메소드를 추가, 역동적인 프로그램 개발이 가능하다.



#### L38 - 인스턴스 프로퍼티

---

1. 인스턴스 프로퍼티

   ```javascript
   obj 인스턴스 = {
       point: 100,
       getPoint: function(){},
       __proto__:{
           getPoint: function(){}
       }
   }
   ```

   * prototype에 연결된 프로퍼티도 인스턴스 프로퍼티가 된다.
     * 직접 인스턴스에 연결된 프로퍼티와 차이가 있다.
   * 인스턴스의 프로퍼티를 prototype으로 만든 인스턴스 프로퍼티보다 먼저 사용한다.
   * 인스턴스마다 값을 다르게 가질 수 있다
     * 인스턴스를 사용하는 중요한 목적

2. 인스턴스 프로퍼티 우선 사용

   ```javascript
   function Book(pppoint){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return 100;
   };
   var obj = new Book(200);
   obj.getPoint = function(){
       return this.point;
   };
   console.log(obj.getPoint());
   ```

   * Book.prototype.getPoint = function(){ return 100;};
     * prototype에 getPoint를 연결한다.
     * 인스턴스의 getPoint()를 호출하면 100을 반환한다.
   * obj.getPoint = function(){ return this.point;};
     * 생성한 인스턴스에 getPoint를 연결한다.
     * 함수가 호출되면 200을 반환한다.
   * obj 인스턴스 구성 형태
   * obj.getPoint();
     * obj 인스턴스의 getPoint() 호출
     * prototype의 getPoint()가 호출되지 않고 인스턴스의 getPoint()가 호출된다.
     * 인스턴스에ㅔ 연결한 프로퍼티를 먼저 사용하기 때문이다.
   * 인스턴스 프로퍼티는 공유되지 않는다.
   * Class 접근
     * 설계가 중요
     * OOP 개념 이해 필요



### [SECTION7 this]

#### L39 - this 개요 / this와 글로벌 오브젝트 / this와 window 오브젝트

---

1. this 개요

   * 키워드이다.
   * obj.name() 형태로 호출한 함수(메소드)에서 this로 인스턴스(오브젝트)를 참조
   * 실행 콘텍스트의 this 바인딩 컴포넌트에 바인딩된다.

2. this와 글로벌 오브젝트

   * 글로벌 오브젝트에서 this는 글로벌 오브젝트를 참조한다.

   * this와 window 오브젝트

     * window는 JS에서 만든것이 아니며 글로벌 오브젝트의 스코프도 아니다.
     * window와 글로벌 오브젝트를 같은 선상에서 사용한다.

   * Host 오브젝트 개념 적용

   * 글로벌 오브젝트에 코드 작성

     window.onload = function(){  // 여기말고 밖에 작성되는 코드들};

     * this가 window 참조  (글로벌 오브젝트)

     ```javascript
     console.log(this === window);  //true
     ```

     * this가 글로벌 변수 사용

     ```javascript
     var value = 100;
     console.log(this.value); //100
     ```

     * window로 글로벌 변수 사용 (window와 글로벌 오브젝트를 같은 선상)

     ```javascript
     var value=100;
     console.log(window.value); //100
     ```

     * this.value = 100; 형태로 값 할당

     ```javascript
     this.value = 100;
     console.log(window.value); //100
     ```

   * window.onload = function(){//여기에 작성되는 코드들};

     * this가 window 참조
       * window 실행 컨텍스트에서 this 바인딩 컴포넌트에서 this를 참조한다.

     ```javascript
     window.onload = function(){
         console.log(this === window);  //true
     };
     ```

     * this로 로컬(지역) 변수 사용

     ```javascript
     window.onload = function(){
         var value = 100;
         console.log(this.value); //undefined
     }
     ```

     * this.value = 100; 형태로 값 할당

     ```javascript
     window.onload = function(){
         this.value = 100;
         console.log(window.value); //100
     }
     ```

     

#### L40 - this 참조 범위 / this와 strict 모드 / this 참조 오브젝트

---





#### L41 -this와 인스턴스

---



#### L42 - this와 call() 메소드 / this 사용 / Object 사용 / 숫자 작성 / this 참조 변경

---



#### L43 - this와 apply() 메소드 / this와 arguments

---



#### L44 - this와 콜백 함수

---



#### L45 - this와 bind() 메소드 / function 오브젝트 생성,호출 / 파라미터 병합

---



#### L46 - bind() 활용 / 이벤트 처리

---



### [SECTION8 논리적 정리]

#### L47 -

---



#### L48 -

---



#### L49 -

---



#### L50 -

---





