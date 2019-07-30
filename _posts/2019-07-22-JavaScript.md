---
layout: post
title:  "JavaScript 정리"
date:   2019-07-22 11:05:02 +0900
---

### 1. Array 
* 1.1 JavaScript Array 전역 객체는 배열을 생성할 때 사용하는 **리스트 형태**의 고수준 객체
    - 배열 만들기
        ```javascript
        var fruits = ['사과', '바나나'];
        ```
    - 배열 항목들 순환하며 처리하기
        ```javascript
        fruits.forEach(function (item, index, array){
            console.log(item, index);
        })
        ```
    - 배열 끝에 항목 추가하기
        - push() 메서드는 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 **새로운 길이를 반환**
        ```javascript
        var newLength = fruits.push('오렌지');
        // newLength는 3
        ```
    - 배열 끝에서부터 항목 제거하기
        - pop() 메서드는 배열의 마지막 요소를 제거하고 **그 요소를 반환**
        ```javascript
        var last = fruits.pop();
        // last는 오렌지
        ```
    - 인덱스 위치에 있는 항목 제거하기
        - splice() 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경
        - 제거한 요소를 담은 배열을 반환
        ```javascript
        var removedItem = fruits.splice(1, 1);
        // removedItem은 바나나
        ```

### 2. 타입체크
* 2.1 `typeof` 연산자는 피연산자의 평가 전 자료형을 나타내는 **문자열을 반환한다**.
    - `typeof` 연산자는 피연산자 앞에 위치
        Type | Result
        -----|-------
        Undefined|"undefined"
        Null|"object"
        Boolean|"boolean"
        Number|"number"
        BigInt|"bigint"
        String|"string"
        Symbol|"symbol"
        호스트 객체|구현체마다 다름
        Function 객체|"function"
        다른 모든 객체|"object"
    ```javascript
    //Numbers
    typeof 37 === 'number';
    typeof Math.LN2 === 'number';
    typeof Infinity === 'number';
    typeof NaN === 'number';

    //Strings
    typeof "" === 'string';
    typeof "bla" === 'string';
    typeof (typeof 1) === 'string'; // typeof always returns a string

    // Symbols
    typeof Symbol() === 'symbol';
    typeof Symbol('foo') === 'symbol';

    // Undefined
    typeof undefined === 'undefined';
    typeof declaredButUndefinedVariable === 'undefined';

    // Objects
    typeof {a: 1} === 'object';
    typeof [1, 2, 4] === 'object';
    // 정규 객체를 배열과 구별하기 위해서는 Array.isArray()나 Object.prototype.toString.call을 사용한다.
    typeof new Date() === 'object';

    // Functions
    typeof function(){} === 'function';
    typeof class C {} === 'function';
    ```

### 3. 구조 분해 할당
* 3.1 **구조 분해 할당** 구문은 배열이나 객체의 속성을 해제하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식
    ```javascript
    var a, b, rest;
    [a, b] = [10, 20];
    console.log(a); // 10
    console.log(b); // 20

    [a, b, ...rest] = [10, 20, 30, 40, 50];
    console.log(a); // 10
    console.log(b); // 20
    console.log(rest); // [30, 40, 50]

    ({a, b} = {a: 10, b: 20});
    console.log(a); // 10
    console.log(b); // 20

    ({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
    console.log(a); // 10
    console.log(b); // 20
    console.log(rest); // {c: 30, d: 40}
    ```

### 4. 객체
* 4.1 객체는 현실의 사물을 프로그래밍에 반영한 것
    - 객체의 속성(Property)
        - 속성은 키(key)와 값(value)으로 구성
        - 키는 문자열만 가능, 따옴표가 있어도 되고 없어도 된다.
        - 값은 어떤 값이든 상관 없음
        - 객체의 속성에 접근하기 위해서는 .이나 []를 사용해서 접근

### 5. 함수
* 5.1 함수 표현 방식
    - 함수 표현식 : 변수를 선언하고 함수를 대입하는 방식
    - 힘수 선언 : 일반적인 함수 표현 방식

* 5.2 함수의 반환
    - 함수가 내부에서 수행되고 밖으로 값이 리턴된다.
    - 리턴이 없는 경우 함수는 undefined를 반환한다.