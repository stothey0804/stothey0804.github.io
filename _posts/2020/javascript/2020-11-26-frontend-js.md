---
title: \[javascript] axios를 사용하여 AJAX 요청하기
categories:
- Javascript
---

> 등장하는 예시 코드는 **Node.js 교과서**에 나온 예를 가져왔습니다.

## AJAX

AJAX는 비동기적 웹 서비스를 개발할 때 사용하는 기법. 페이지의 이동 없이 서버에 요청/응답을 받는 기술이다.
AJAX 요청은 jQuery나 axios 같은 라이브러리를 주로 사용한다. 원래 브라우저에서 `XMLHttpRequest` 객체를 제공하나 복잡하고 서버에서 사용할 수 없어서 앞선 방법을 사용하게 된다.
jQuery를 이용하는 방식은 학원을 수료하며 만든 프로젝트에서 많이 써봤으니 axios를 사용하는 방법을 정리하고자 한다.


```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    axios.get('url')
        .then((result) => {
            console.log(result);
            console.log(result.data);   // {}
        })
        .catch((error) => {
            console.error(error);
        });
</script>
```

`axios.get`도 내부에 `new Promise`가 들어가있어 `.then`과 `.catch`를 사용할 수 있다.
`result.data`에는 서버로부터 보낸 데이터가 들어있다.

프로미스 이므로 `async/await` 방식으로 변경할 수 있다.


### GET 요청

```javascript
...
    (async() => {   // 익명함수
        try(){
            const result = await axios.get('url');
            console.log(result);
            console.log(result.data); // {}
        }catch(error){
            console.error(error);
        }
    })();
...
```

### POST 요청

```javascript
...
    (async() => {
        try(){
            const result = await axios.post('url', {
                name: 'sykim',
                gender: 'f',
            });
            console.log(result);
            console.log(result.data); // {}
        }catch(error){
            console.error(error);
        }
    })();
...
```

구조는 비슷하며 요청에 따라 `axios.get`과 `axios.post`를 사용하는데, POST의 경우 두 번째 인수에 데이터를 넣어보낸다.


## FormData

FormData는 주로 AJAX와 함께 `form` 태그의 데이터를 동적으로 제어할 수 있는 기능이다.

```javascript
const formData = new FormData();
formData.append('name','sykim');
formData.append('item','orange');
formData.append('item','melon');
formData.has('item');   // T
formData.has('money');  // F
formData.get('item');   // orange
formData.getAll('item');    // ['orange', 'melon']
formData.append('test',['hi','kim']);
formData.delete('test');    // test = null
formData.set('item', 'apple');
formData.getAll('item')     // ['apple']
```

생성된 객체에 `append` 메서드를 통해 키-값 형식의 데이터를 저장할 수있으며
`delete`를 사용하여 삭제하거나 `set`을 사용해 수정할 수 있다.

이렇게 생성된 폼 데이터를 서버로 보내면된다.

```javascript
...
    (async() => {
        try{
            const formData = new FormData();
            formData.append('name','sykim');
            ...
            const result = await axios.post('url', formData);   // formData 전송!
            // ...
        }catch(error){
            // ...
        }
    })();
...
```

위와 같이 `axios.post` 메서드의 두번째 인수로 formData를 보내준다.

## encodeURIComponet, decodeURIComponent

AJAX 요청을 보낼 때 주소에 한글이 들어가는 경우 `window`객체의 메서드인 `encodeURIComponent` 메서드를 사용하여 한글 부분을 감싼다.

```javascript
...
    (async() => {
        try{
            const result = await axios.get(`http://.../${encodeURIComponent('노드')}`); // 인코딩 영역 감싸기
            // ...
        }cathch(error){
            // ..
        }
    })();
...
```

위와같이 감싸주면 노드라고 적힌 한글 주소를 변환시켜준다.
주소를 받는 쪽에서는 `decondeURIComponet`를 사용하면 된다.

```javascript
decodeURIComponent('인코딩된 노드')
```