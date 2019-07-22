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