# 콜백함수(Callback Function)

- 콜백함수란?
    - 파라미터로 함수를 전달받아 함수의 내부에서 실행하는 함수.

- 제어권
    - 호출시점
        ```
            var count = 0;
            var timer = setInterval(function(){
                console.lg(count);
                if(++count>4) clearInterval(timer);
            },300);
        ```
        - setInterval을 호출할 때 두 개의 매개변수를 전달(익명함수,300)

        <br/>

        ```
            // setInterval의 구조
            var intervalID = scope.setInterval(func,delay[,param1,param2, ...]);
        ```
        - 매개변수로는 func(함수), delay(ms단위의 숫자)값이 필수, 세번째 매개변수부터는 선택
        - setInterval을 실행하면 반복적으로 실행되는 내용 자체를 특정할 수 있는 고유한 ID값이 반환됨 -> 반복 실행되는 중간에 종료(clearInterval)할 수 있게 하기 위해서
        

        <br/>

        ```
            var count = 0;
            var cbFunc = function(){
                console.log(count);
                if(++count>4) clearInterval(timer);
            };
            var timer = setInterval(cbFunc,300);

            // 실행결과
            // 0 (0.3초)
            // 1 (0.6초)
            // 2 (0.9초)
            // 3 (1.2초)
            // 4 (1.5초)
        ```
        - **호출하는 시점을 스스로 판단하여 실행**
        - 코드 실행 방식과 제어권
            |code|호출 주체|제어권|
            |---|---|---|
            |cbFunc();|사용자|사용자| 
            |setInterval(cbFunc,300);|setInterval|setInterval|
        - setInterval에 첫번째 인자로 넘겨준 콜백함수(cbFunc) -> setInterval은 콜백 함수에 대한 제어권을 가지게 됨
    - 인자
        ```
            var newArr = [10, 20, 30].map(function(currentVal, index){
                console.log(currentValue,index);
                return currentValue+5;
            });
            console.log(newArr);

            // 실행결과
            // 10 0
            // 20 1
            // 30 2
            // [15, 25, 35]
        ```
        - 첫번째 매개변수로 익명 함수를 전달

        <br/>

        ```
            // prototype에 담긴 map 메서드의 구조
            Array.prototype.map(callback[,thisArg])
            callback: function(currentValue, index, array)
        ```
        - 첫 번째 인자로 callback 함수를 받음
        - 생략 가능한 두 번째 인자로 콜백 함수 내부에서 this로 인식할 대상을 특정할 수 있음 -> thisArg를 생략할 경우 전역객체가 바인딩 됨
        - map 메서드는 배열의 모든 요소들을 하나씩 꺼내며 콜백 함수를 반복 호출하고, 콜백 함수의 실행 결과들을 모아 새로운 배열을 생성함
        - **콜백 함수를 호출하면 인자로 넘겨줄 값들 순서가 정해져 있음:** `(현재값, 인덱스, map 배열 자체)`

    - this
        ```
            Array.prototype.map = function(callback, thisArg){
                var mappedArr = [];
                for(var i = 0; i < this.length; i++){
                    var mappedValue = callback.call(this.Arg || window, this[i], i, this);
                    mappedArr[i] = mappedValue;
                }
                return mappedArr;
            }
        ```
        - 메서드 구현의 핵심 : `call/apply` (?!)
        - **this에는 thisArg값이 있을 경우 thisArg, 없으면 전역객체를 바라봄 this를 바꾸고 싶은 경우 bind 메서드를 활용**


