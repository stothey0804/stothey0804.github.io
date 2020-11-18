---
title: JAVA - Collection Framework
categories:
- JAVA
toc: true
---

## JAVA Collection Framework

자료구조 수업을 처음 들었을 때 심한 멘탈붕괴가 왔었다.
안그래도 생소한 자료구조 개념인데 제공되는 API가 없어 직접 구현 하다 보니 죽을 맛이었다. (C언어 베이스로 배웠었다.)


그런데 JAVA는 java.util.* 만 import 해주면 아주 편하게 쓸 수 있다.  
(ArrayList의 아주 기본적인 메서드 정도는 직접 만들어보면 도움이 될 것 같다.)


아래는 JAVA에서 제공하는 컬렉션 계층구조이다.


![](https://raw.githubusercontent.com/stothey0804/stothey0804.github.io/master/_posts/2020/JAVA/Collection_interfaces.png)
> JAVA 프레임워크에서 제공하는 API
> : <https://www.ntu.edu.sg/home/ehchua/programming/java/J5c_Collection.html#zz-2>




컬렉션은 특징에 따라 세 가지로 분류할 수 있다고 배웠다.


1. List
2. Set
3. Map


참고로 List, Set 은 Collection인터페이스의 하위 인터페이스지만,  Map은 Collection의 하위 인터페이스가 아니다.

- - -
## Collection의 종류와 특징

### List
* 순서(index)가 있다.
* 값(value)의 중복이 허용된다.

대표적으로 ArrayList, LinkedList(Queue) 등이 있다.

- - -
### Set
* 집합을 뜻하므로, 중복 값을 허용하지 않는다.
* 자료의 순서가 없다. (단, SortedSet, TreeSet에는 순서가 있다.)

대표적으로 HashSet, SortedSet, TreeSet이 있다.


- - -

### Map
* 키(key) = 값(value) 쌍으로 이루어져 있다.
* value는 중복을 허용하나, key는 유일한 값이다.

대표적으로 HashMap, HashTable, SortedMap, TreeMap등이 있다.

- - -

간략하게 살펴보면 위와 같다.

필요한 자료구조 형태에 따라 자바가 제공해주는 컬렉션을 선택하면 간단하며,
해당 인터페이스들의 메서드는 중복되는게 많고, Collection의 기본메서드를 쓸줄 알면
다른 컬렉션 클래스도 금방 사용할 수 있다!

<br><br>

## Collection의 기본 메서드

다음은 Collection 객체의 메서드의 일부다.



|타입|메서드|설명|
|---|---|---|
|boolean|add(E e)|컬렉션에 파라미터로 받은 요소를 추가한다.|
|boolean|	addAll(Collection<? extends E> c)|파라미터로 받은 컬렉션의 모든 요소를 추가한다.  ``합집합``|
|void|clear()|현재 컬렉션의 **모든 요소**를 삭제한다.|
|boolean|contains(Object o)|파라미터로 받은 요소의 존재 여부를 리턴|
|boolean|containsAll(Collection<?> c)|파라미터로 받은 컬렉션의 모든 요소를 포함하는지|
|boolean|equals(Object o)|파라미터로 받은 객체와 컬렉션이 동일한지|
|Iterator\<E>|iterator()|해당 컬렉션의 Iterator(반복자)를 반환|
|boolean|remove(Object o)|파라미터로 받은 요소를 삭제|
|boolean|removeAll(Collection<?> c)|파라미터로 받은 컬렉션의 모든 값을 삭제 ``차집합``|
|boolean|retainAll(Collection<?> c)|중복되는 부분을 제외하고 삭제 ``교집합``|
|int|size()|컬렉션의 요소수를 리턴|
|Object[]|toArray()|현재 컬렉션의 모든 요소를 array로 리턴한다.|


Collection 메서드의 대부분은 Set과 호환된다.


> Colletion 인터페이스
> : <https://docs.oracle.com/javase/10/docs/api/java/util/Collection.html>

<br>

### List의 기본 메서드
List는 Collection의 하위 인터페이스이며, 인덱스로 접근하는 메서드를 추가로 갖고있다.
이 포스트에서는 List에서 추가되며 자주 사용해본 것만을 정리한다.

|타입|메서드|설명|
|---|---|---|
|void|add(int index, E element)|index 위치에 요소 추가|
|E|get(int index)|index 위치의 값 리턴|
|int|indexOf(Object o)|해당 파라미터의 값이 있는 인덱스를 리턴|
|int|lastIndexOf(Object o)|↑ 마지막 인덱스부터 탐색하여 리턴|
|ListIterator\<E>|listIterator()|ListIterator를 반환|
|ListIterator\<E>|listIterator(int index)|지정 위치부터 시작되는 ListIterator 반환|
|E|remove(int index)|index 위치 값 삭제|
|E|set(int  index, E element)|해당 index의 값을 element로 대체한다.|
|default void|sort(Comparator<? super E> c)|목록을 정렬한다. (Comparator가 있을 시 파라미터로)|
|List<E>|	subList(int fromIndex, int toIndex)|fromIndex ~ toIndex _미만_ 까지 범위의 요소를 리턴 |

<br><br>

### Map의 기본 메서드

**[참고]** Map은 Collection의 하위 인터페이스가 아닌 점에 주의하자.
{: .notice}

|타입|메서드|설명|
|---|---|---|
|V|put(K key, V value)||
|void|putAll(Map<? extends K,? extends V> m)||
|V|get(Object key)||
|V|remove(Object key)||
|default boolean|	remove(Object key, Object value)||
|boolean|containsKey(Object key)||
|boolean|containsValue(Object value)||
|int|size()||
|void|clear()||
|static <K,V> Map<K,V>|copyOf(Map<? extends K,? extends V> map)||
|static <K,V> Map.Entry<K,V>	|entry(K k, V v)||
|Set<Map.Entry<K,V>>|entrySet()||
|boolean|equals(Object o)||
|Set\<K>|	keySet()||
|Collection<V>|values()||


<br>

> Map 인터페이스
> : <https://docs.oracle.com/javase/10/docs/api/java/util/Map.html>


#### Map.Entry
