### 1. 크롬 확장 도구 구조
크롬 확장 도구는 몇 가지 핵심 파일로 구성됩니다:

- **manifest.json**: 확장 도구의 기본 설정 파일로, 확장 도구의 이름, 버전, 권한, 실행할 스크립트 등을 정의합니다.
- **background script**: 백그라운드에서 실행되어 확장 도구의 전체 라이프사이클을 관리합니다. 이벤트 리스너, 알림 등의 작업을 처리합니다.
- **content script**: 웹 페이지에 삽입되어 페이지의 DOM에 접근하거나, 페이지와 상호작용합니다.
- **popup.html**: 확장 도구 아이콘을 클릭했을 때 나타나는 팝업 인터페이스입니다.

### 2. 기본 설정 (manifest.json)
`manifest.json` 파일에서 확장 도구의 기본 설정을 정의합니다. 예시는 다음과 같습니다:

```json
{
  "manifest_version": 3,
  "name": "My Chrome Extension",
  "version": "1.0",
  "description": "A sample Chrome extension.",
  "permissions": ["activeTab", "scripting"],
  "action": {
    "default_popup": "popup.html"
  },
  "background": {
    "service_worker": "background.js"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ]
}
```

### 3. 백그라운드 스크립트 (background.js)
`background.js`는 확장 도구가 설치되거나 활성화될 때 백그라운드에서 실행됩니다. 예를 들어, 아이콘 클릭 시 이벤트 처리를 할 수 있습니다.

```javascript
chrome.action.onClicked.addListener((tab) => {
  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    function: () => console.log("Extension icon clicked!")
  });
});
```

### 4. 콘텐츠 스크립트 (content.js)
`content.js`는 웹 페이지의 DOM에 접근하고, 페이지와 상호작용할 수 있도록 돕습니다. 예를 들어, 페이지의 텍스트를 변경하거나 특정 요소를 강조하는 등의 작업을 할 수 있습니다.

```javascript
document.body.style.backgroundColor = "yellow";
```

### 5. 팝업 HTML 및 JavaScript (popup.html & popup.js)
`popup.html`은 확장 도구 아이콘을 클릭했을 때 보여질 UI를 정의합니다. 이 UI에서 버튼 클릭 이벤트 등을 처리할 수 있습니다.

```html
<!-- popup.html -->
<!DOCTYPE html>
<html>
<head>
  <title>My Popup</title>
  <script src="popup.js"></script>
</head>
<body>
  <button id="myButton">Click me!</button>
</body>
</html>
```

```javascript
// popup.js
document.getElementById("myButton").addEventListener("click", () => {
  alert("Button clicked!");
});
```

### 6. 메시지 전송 및 통신
확장 도구의 여러 스크립트 간에 메시지를 주고받을 수 있습니다. 예를 들어, `popup.js`에서 `content.js`로 메시지를 보내고, 페이지에서 원하는 동작을 수행할 수 있습니다.

```javascript
// popup.js
chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
  chrome.tabs.sendMessage(tabs[0].id, { message: "Hello from popup!" });
});

// content.js
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.message === "Hello from popup!") {
    console.log("Received message from popup.");
  }
});
```

### 7. 실제 활용 예: 스크린 영역 캡처 및 전송
사용자가 크롬 확장 도구의 버튼을 클릭하여 화면에서 특정 영역을 선택하고 그 영역을 이미지로 캡처하여 서버로 전송하는 예를 만들어볼 수 있습니다. 이 예에서는 `background.js`, `content.js`, `popup.js`에서 각각 역할을 나누어 구현합니다.

- **popup.js**에서 버튼 클릭 시, **content.js**에 메시지를 보내 화면의 선택 모드로 전환합니다.
- **content.js**에서 사용자가 선택한 영역을 캡처하고 이를 서버에 전송합니다.

이러한 기능은 JavaScript와 크롬 확장 도구의 메시지 통신을 활용하여 유연하게 구현할 수 있습니다.

---

`chrome` 객체는 크롬 확장 프로그램에서 사용할 수 있는 기본 API 객체입니다. 이 객체는 브라우저와 확장 프로그램 간의 상호작용을 돕는 다양한 메서드와 프로퍼티로 구성되어 있습니다.

### 1. `chrome` 객체의 주요 구성 요소
`chrome` 객체는 확장 프로그램의 기능을 지원하는 여러 하위 API로 나뉘어 있습니다. 아래는 자주 사용되는 주요 API들입니다.

#### `chrome.tabs`
- **역할**: 현재 열려 있는 탭 정보를 가져오거나, 특정 탭에서 스크립트를 실행할 수 있습니다.
- **주요 메서드**:
  - `chrome.tabs.query()`: 특정 조건에 맞는 탭 정보를 가져옵니다.
  - `chrome.tabs.executeScript()`: 특정 탭에서 JavaScript 코드를 실행합니다.
  - `chrome.tabs.create()`: 새 탭을 엽니다.

#### `chrome.runtime`
- **역할**: 확장 프로그램의 라이프사이클을 관리하며, 확장 프로그램 간의 메시지 통신을 담당합니다.
- **주요 메서드**:
  - `chrome.runtime.sendMessage()`: 확장 프로그램의 여러 스크립트 간에 메시지를 보냅니다.
  - `chrome.runtime.onMessage`: 메시지를 수신할 때 호출되는 이벤트 리스너를 설정합니다.
  - `chrome.runtime.getURL()`: 확장 프로그램의 파일에 접근할 수 있는 URL을 반환합니다.

#### `chrome.storage`
- **역할**: 확장 프로그램의 데이터를 저장하고, 로컬 또는 동기화된 저장소에서 데이터를 가져올 수 있습니다.
- **주요 메서드**:
  - `chrome.storage.local.get()`: 로컬 저장소에서 데이터를 가져옵니다.
  - `chrome.storage.local.set()`: 로컬 저장소에 데이터를 저장합니다.
  - `chrome.storage.sync.get()`, `chrome.storage.sync.set()`: 동기화된 저장소를 사용해 여러 장치에서 공유되는 데이터를 저장 및 로드할 수 있습니다.

#### `chrome.action`
- **역할**: 확장 도구의 브라우저 액션(아이콘)을 관리합니다.
- **주요 메서드**:
  - `chrome.action.setIcon()`: 확장 도구의 아이콘을 동적으로 설정합니다.
  - `chrome.action.setBadgeText()`: 아이콘 위에 표시할 배지 텍스트를 설정합니다.
  - `chrome.action.onClicked`: 아이콘이 클릭될 때 발생하는 이벤트를 처리합니다.

#### `chrome.scripting`
- **역할**: 특정 탭에서 스크립트를 삽입하거나 실행합니다.
- **주요 메서드**:
  - `chrome.scripting.executeScript()`: 지정된 탭에서 JavaScript를 실행합니다.
  - `chrome.scripting.registerContentScripts()`: 콘텐츠 스크립트를 동적으로 등록합니다.

#### `chrome.notifications`
- **역할**: 알림을 생성하고 관리합니다.
- **주요 메서드**:
  - `chrome.notifications.create()`: 새 알림을 생성합니다.
  - `chrome.notifications.clear()`: 알림을 닫습니다.

### 2. 이벤트 리스너 구조
`chrome` 객체의 각 하위 API는 자체 이벤트 리스너를 가지고 있어 특정 이벤트가 발생할 때 이를 처리할 수 있습니다. 예를 들어, `chrome.runtime.onMessage.addListener()`는 메시지 수신 이벤트를 감지하여 특정 작업을 수행합니다.

### 3. `chrome` 객체를 사용한 예시

```javascript
// 현재 활성 탭을 가져오고, 그 탭에서 특정 작업을 수행하는 예시입니다.
chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
  let activeTab = tabs[0];
  chrome.scripting.executeScript({
    target: { tabId: activeTab.id },
    function: () => {
      console.log("This script runs in the active tab.");
    }
  });
});
```

위의 예시는 `chrome.tabs.query()`를 통해 활성화된 탭을 찾고, `chrome.scripting.executeScript()`를 사용해 해당 탭에서 콘솔에 메시지를 출력하는 스크립트를 실행하는 코드.

