# Polyfills
FuseJS는 FuseJS가 지원하는 모든 플랫폼에서, 최소 EcmaScript 5.1 이상의 환경에서 실행됩니다. 또한 FuseJS는 웹 브라우저를 포함하지 않고, 웹 브라우저의 표준 라이브러리의 부분 집합만을 제공합니다. FuseJS는 주로 3rd-Party 라이브러리들을 작동시키기 위해 브라우저의 일부 기능들에 대한 Polyfill을 제공합니다. 이 구현체들은 현재는 완벽하지 않지만, [FuseJS Roadmap](https://www.fusetools.com/learn/fusejs#keyword-fusejs-roadmap)을 참고하여 개발 상태를 확인할 수 있습니다.

## fetch
이 기능은 HTTP 요청을 처리하기 위한 일반적인 방법입니다.

아래 예제를 통해 `fetch`를 이용해서 어떻게 JavaScript Object (JSON)을 가져오는지 확인할 수 있습니다. JSON 형태로 POST 요청을 보내고 JSON 데이터를 수신합니다.

```javascript
var status = 0;
var response_ok = false;

fetch('http://example.com', {
    method: 'POST',
    headers: { "Content-type": "application/json"},
    body: JSON.stringify(requestObject)
}).then(function(response) {
    status = response.status;  // HTTP 상태 코드를 가져옵니다.
    response_ok = response.ok; // response.status가 200(요청 성공)범위 안에 있는가?
    return response.json();    // Promise를 return 합니다.
}).then(function(responseObject) {
    // 요청 성공, 결과는 responseObject에 반영되어 있습니다.
}).catch(function(err) {
    // JSON을 파싱하다 오류가 발생했을 때
});
```

아래 경로에서 더 많은 정보를 확인할 수 있습니다.

- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API

## XMLHttpRequest
FuseJS는 `XMLHttpRequest`를 제공합니다. 아래 경로에서 더 많은 정보를 확인할 수 있습니다.

- https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest

## Promise
FuseJS는 `Promise`를 지원합니다. 아래 경로에서 더 많은 정보를 확인할 수 있습니다.

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## setTimeout
2초 후 함수를 실행하기 위해서는 다음과 같이 작성합니다.
```javascript
setTimeout(function() { alert("Alert"); }, 2000);
```

또한 루프를 만들기 위해서도 `setTimeout`을 사용할 수 있습니다.
```javascript
function poller() {
    // 실행 될 코드 (...)

    // 1초 후 다시 실행한다.
    setTimeout(poller, 1000);
}
```