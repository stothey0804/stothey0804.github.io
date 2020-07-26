---
title: JAVA - Collection Framework
categories:
- JAVA
---

자료구조 수업을 처음 들었을 때 심한 멘탈붕괴가 왔었다.
안그래도 생소한 자료구조 개념인데 제공되는 API가 없어 직접 구현 하다 보니 죽을 맛이었다. (C언어 베이스로 배웠었다.)


그런데 JAVA는 java.util.* 만 import 해주면 아주 편하게 쓸 수 있다.  
(ArrayList의 아주 기본적인 메서드 정도는 직접 만들어보면 도움이 될 것 같다.)


아래는 JAVA에서 제공하는 컬렉션 계층구조이다.


![](Collection_interfaces.png)
> JAVA 프레임워크에서 제공하는 API
> 출처: https://www.ntu.edu.sg/home/ehchua/programming/java/J5c_Collection.html#zz-2.




컬렉션은 특징에 따라 세 가지로 분류할 수 있다고 배웠다.


### 1. List
### 2. Set
### 3. Map


참고로 List, Set 은 Collection인터페이스의 하위 인터페이스지만,  Map은 Collection의 하위 인터페이스가 아니다.

<hr>

## List의 특징 
* 순서(index)가 있다.
* 값(value)의 중복이 허용된다.

대표적으로 ArrayList, LinkedList(Queue) 등이 있다.

<hr>

## Set의 특징
* 집합을 뜻하므로, 중복 값을 허용하지 않는다.
* 자료의 순서가 없다. (단, SortedSet, TreeSet에는 순서가 있다.)

대표적으로 HashSet, SortedSet, TreeSet이 있다.


<hr>

## Map의 특징
* 키(key) = 값(value) 쌍으로 이루어져 있다.
* value는 중복을 허용하나, key는 유일한 값이다.

대표적으로 HashMap, HashTable, SortedMap, TreeMap등이 있다.

<hr>

간략하게 살펴보면 위와 같다.

필요한 자료구조 형태에 따라 자바가 제공해주는 컬렉션을 선택하면 간단하며,
해당 인터페이스들의 메서드는 중복되는게 많고, Collection의 기본메서드를 쓸줄 알면
다른 컬렉션 클래스도 금방 사용할 수 있다!


> Colletion 인터페이스
>https://docs.oracle.com/javase/10/docs/api/java/util/Collection.html

> Map 인터페이스
>https://docs.oracle.com/javase/10/docs/api/java/util/Map.html
