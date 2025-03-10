# [2] DomAPI - 2 innerHTML의 비효율성과 그 대안들

웹 개발에서 DOM을 조작하는 건 자주 일어나는뎅, 그 중에서도 `innerHTML` 속성을 사용하여 요소의 HTML을 쉽게 변경할 수 있지만, innerHTML은 몇가지 비효율적인 부분이 있다고 해서 정리해보게 되었습니다. 이 글에서는 `innerHTML`의 작동 방식과 그 한계, 대안점을 함께 공부해보려 합니다.

#### `innerHTML`의 작동 원리

`innerHTML` 속성은 문자열을 통해 HTML 요소의 내용을 바꾸는 방법 중 하나인데, 예를 들어, 다음과 같은 코드가 있다고 가정해 보겠습니다:

```js
document.querySelector('#app').innerHTML = '<p>Updated content</p>';
```
이 코드는 ID가 'app'인 요소의 내부 HTML을 새로운 내용으로 교체하고 있습니다.

#### `innerHTML` 사용 시의 문제점

하지만 `innerHTML`을 사용할 때 문제가 발생할 수 있는데, 아래와 같이 알아보겠습니다.

1. **성능 저하**: `innerHTML`을 사용하면 지정된 요소의 자식 요소 전체를 파싱하고 새로운 노드로 교체해야 하므로, 대규모 DOM에서는 성능에 부정적인 영향을 줄 수 있습니다.
    
2. **스크립트 재실행**: `innerHTML`로 삽입된 HTML 안에 `<script>` 태그가 있으면, 이 스크립트는 실행되지 않습니다.
    
3. **상태 손실**: 기존 요소를 새 요소로 완전히 교체하면, 이전 요소의 모든 상태(예: 입력 필드의 값, 포커스 상태)가 손실됩니다. 예를 들어, 사용자가 입력 필드에 데이터를 입력하는 동안 `innerHTML`이 실행되면 입력 중이던 데이터와 포커스가 모두 사라집니다.
    

#### 효율적인 대안

이러한 문제를 해결하기 위해 다른 DOM 조작 방법들이 있습니다.

1. **`textContent`와 `innerText`**: 단순 텍스트 내용만 변경해야 할 경우, `textContent` 또는 `innerText` 속성을 사용하면 불필요한 HTML 파싱 과정을 거치지 않아도 됩니다.
    
2. **`createElement`와 `appendChild`**: 새 요소를 추가할 때는 `createElement`로 요소를 만든 후 `appendChild`를 사용하여 DOM에 추가하는 방법이 효율적입니다. 이건 기존의 DOM 구조를 유지하면서 새 요소만 추가하므로, 전체를 다시 로드할 필요가 없습니다.
    
3. **프레임워크 사용**: React나 Vue 같은 현대적인 프론트엔드 프레임워크는 DOM을 효율적으로 조작하는 자체 메커니즘을 제공합니다. 예를 들어, React는 가상 DOM을 사용하여 변경이 필요한 부분만 실제 DOM에 반영합니다.
    

#### 결론

`innerHTML`은 특정 상황에서 유용할 수 있지만, 큰 규모의 어플리케이션 또는 동적인 요소가 많은 인터랙티브 웹 페이지에서는 그 한계가 명확합니다. 효율적인 DOM 조작을 위해서는 `innerHTML`의 대안을 적극적으로 고려하고, 가능한 경우 현대적인 프론트엔드 프레임워크를 활용하는 것이 좋을 것 같습니다.